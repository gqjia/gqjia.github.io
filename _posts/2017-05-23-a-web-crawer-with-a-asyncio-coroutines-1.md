---

layout: post
title: 用异步协程写一个爬虫 (1)
tag: "crawer"
categories: posts
math: y
published: true

---

本文译自： [A Web Crawler With asyncio Coroutines](http://www.aosabook.org/en/500L/a-web-crawler-with-asyncio-coroutines.html).

由于本文篇幅很长, 在阅读本文之前不妨看一下 [如何入门 Python 爬虫](https://www.zhihu.com/question/20899988) 了解爬虫的基本概念, 有时间再继续阅读.

### Introduction

传统的计算机科学总是将关注点放在研究更加有效率的算法上， 放在如何才能够更快地完成计算上。 但是实际上，对于很多网络程序， 它们的大部分时间并非花在计算， 而是耗费在维持低速连接或是一些很少发生的事件上。 这类程序面临的是一个非常不同以往的挑战： 高效地等待海量的网络事件。 当前应对该挑战的方法是使用异步 I/O (asynchronous I/O), "async" .

本文将会展示一个简单的爬虫。 这个爬虫仅仅是一个异步应用的原型， 因为尽管它等待很多响应，但实际并没有做多少计算。 爬虫一次能够获取的页面越多，整个任务完成地就会越快。 如果它对每个请求都消耗一个线程 (thread), 那么当并发请求数增加时， 在耗尽 socket 资源之前, 它就将耗尽内存或是其他线程相关的资源。 通过使用异步 I/O 可以避免对线程的需求。

我们将会以三个阶段来解读案例：

- 第一阶段， 我们展示了一个异步的事件循环 (event loop)，大致勾勒了使用带有回调 (callback) 事件循环的爬虫框架。 虽然十分有效， 但是要想对它进行扩展来处理更复杂的问题, 将会导致产生很多不可控制的意大利面式代码 (译者注: 意大利面式代码, spagjetti code, 大意指代码的控制结构复杂、混乱而难以理解)。

- 第二阶段， 我们将会展示兼具效率与扩展性的 Python coroutine (协程) 。 我们还将通过使用 Python 的 generator(生成器) 函数， 实现一个简单的 coroutine。

- 第三阶段， 我们将会使用有着完整协程特性的 Python 标准库 "asyncio" [^1],  同时使用一个异步队列进行协调, 实现一个异步的爬虫。

### The Task

一个爬虫会查找并下载一个网站上的所有页面， 并且可能还会存储或者给它们加上索引。 从根 URL (root URL) 开始， 它会爬取每一个页面，并且对其进行解析得到 “尚未见过” 页面的链接，然后将这些链接加入到待爬取的队列中。 只有当爬虫获取到的页面上的所有链接都已经 “见过”， 或者等待爬取的队列为空， 爬虫才会停止工作。

我们可以通过并发地下载多个页面来加快整个爬虫的进程。 当爬虫发现新的链接时， 它同时通过独立的 socket 开始获取新的页面. 当新页面到达时， 它会解析响应并将新的链接添加到队列中。 当存在大量并发时，性能会有所下降，因此有必要压缩返回信息, 同时我们也会限制并发请求数，并且直到一些正在进行中的请求完成后, 才继续开始待爬取队列中的剩余链接。

### The Traditional Approach

如何才能够让爬虫并发工作？ 传统上， 我们会创建一个线程池， 每个线程将会负责通过一个 socket 每次下载一个页面. 比如， 从 `xkcd.com` 下载一个页面：

```python

def fetch(url):
    sock = socket.socket()
    sock.connect(('xkcd.com', 80))
    request = 'GET {} HTTP/1.0\r\nHost: xkcd.com\r\n\r\n'.format(url)
    sock.send(request.encode('ascii'))
    response = b''
    chunk = sock.recv(4096)
    while chunk:
        response += chunk
        chunk = sock.recv(4096)

    #  Page is now downloaded
    links = parse_links(response)
    q.add(links)

```

默认情况下， socket 操作是 *阻塞式(blocking)* 的： 当线程调用像 `connect` 或 `recv` 这样的方法时， 它会暂停进入等待状态直至操作完成 [^2]。 因此, 如果想要一次性下载多个页面， 我们就需要很多线程。 一个成熟的应用, 通常会通过保持线程池中的闲置线程来分摊线程创建的成本, 然后在后续的任务中对这些线程进行检查并进行重用； 对于连接池的 socket 也是同样的处理。

然而， 使用线程有着很高的成本，并且操作系统会给线程强加很多限制，用户，机器都会受到影响。 在 Jesse (原作者)的系统上， 一个 Python 线程将会花费 50k 的内存，而且如果创建非常多的线程时会导致失败。 把规模继续扩大, 如果使用并发的 socket 执行成百上千的同步操作， 那么在 socket 资源耗尽之前, 我们就会用光线程资源。 单个线程的消耗, 或者系统对于线程的限制, 是这其中的瓶颈。

在 Dan Kegel 一篇很有影响力的文章 “The C10K problem [^3]” 中， 他大致描述了 I/O 并发中多线程的一些局限。 开头是这么说的：

>难道你不认为如今的网络服务器应该有能力同时应付成千上万的客户端请求吗？

Kegel 在 1999 年创造了 "C10K" 这个词。 虽然上万的连接现在听起来并不少，但其实只是问题的规模发生了改变， 其问题类型却并未改变。 在那个时候， 对于 C10K 的每个连接采用一个线程是十分不实用的.  的确，仅仅通过使用线程, 我们写的这个玩具级别的爬虫也可以工作。 但是对于有着成千上万连接的大规模应用来说，它的天花板依旧存在： 大多数系统虽然仍然能够创建 socket, 但是却已经耗尽了 thread. 那么如何解决这个问题呢？

### Async

异步的 I/O 框架在一个单一 thread 上使用 *non-blocking (非阻塞)*  的 socket 完成并发操作。 在我们的异步爬虫中，在开始连接到服务器之前，我们将 socket 设置为非阻塞式的:

```python

sock = socket.socket()
sock.setblocking(False)
try:
    sock.connect(('xkcd.com', 80))
except BlockingIOError:
    pass

```

恼人的是，即便当一个非阻塞的 socket 正常工作的时候， 它也会从 `connect` 中抛出一个异常. 这个异常复制了底层 C 函数一个令人心烦的行为 -- 它会发送 `errno` 到 `EINPROGRESS` 来告诉你已经开始工作了。

爬虫需要有一个途径来知道连接何时已经建立， 以便于它能够发起 HTTP 请求。那么, 我们可以在一个简单紧凑的循环中不停地尝试:

```python

request = 'GET {} HTTP/1.0\r\nHost: xkcd.com\r\n\r\n'.format(url)
encoded = request.encode('ascii')

while True:
    try:
        sock.send(encoded)
        break  # Done
    except OSError as e:
        pass

print('sent')

```

但是， 这个方法不仅浪费电能，而且无法在 *多个* socket 上高效地等待事件。 在以前， BSD unix 对这个问题的解决方案是使用 `select`, 这是一个在一个或多个 socket 上等待事件的 C 函数。 由于今天对于海量连接的网络应用的要求，已经出现了像 `poll` 这样的替代品，以及随后的 BSD 上的 `kqueue`， Linux 上的 `epoll`. 这些 API 与 `select` 相类似，但是在面对大量连接时表现十分优异。

Python 3.4 的 `DefaultSelector`,  会使用了当前系统上已有最好的类 `select` 函数。 为了注册网络 I/O 的通知， 我们创建一个非阻塞的 socket 并使用默认的 selector 对其进行注册:

```python

from selectors import DefaultSelector, EVENT_WRITE

selector = DefaultSelector()

sock = socket.socket()
sock.setblocking(False)

try:
    sock.connect(('xkcd.com', 80))
except BlockingIOError:
    pass

def connected():
    selector.unregister(sock.fileno())
    print('connected')

selector.register(sock.fileno(), EVENT_WRITE, connected)

```

我们忽略了上面无关紧要的错误, 并调用了 `selector.register`,  它传入的参数有 socket 的文件描述符， 一个表示我们正在等待事件类型的常数。 我们还传入一个 Python 函数 `connected`, 当事件发生时, `connected` 就会运行。这样的函数通常叫做 *回调 (callback)* 函数.

在一个循环中，当我们接收到 I/O 通知时就会对其进行处理:

```python

def loop():
    while True:
        events = selector.select()
        for event_key, event_mask in events:
            callback = event_key.data
            callback()

```

`connect` 回调被存储为 `event_key.data`, 一旦非阻塞 socket 连接完成, 我们就会进行检索和执行。

并不像上面的快速循环， 这里对 `select` 的调用会暂停等待下一个 `I/O` 事件。 尚未完成的操作将会被挂起, 直至事件循环的未来 tick.

至此, 我们已经阐明了哪些内容？ 我们展示了如何开始一个操作，当操作准备完成后如何执行一个回调。 基于我们已经展示的特性 -- 非阻塞的 socket 和事件循环, 构建一个异步 *框架 (framework)* 在单一线程上执行并发操作。

到目前为止, 我们已经完成了 "并发 (concurrency)" 这一目标，但还不是传统意义上的 "并行 (parallelism)". 也就是说，我们构建一个能够完成 I/O 重叠的小型系统。 当其他操作正在进行时，它能够开始一个新的操作。 实际上，它并没有利用多核并行地执行计算。不过， 这个系统是针对 I/O 限制问题而设计的，而并非 CPU 限制 [^4]。

所以我们的时间循环在并发 I/O 上是十分有效的， 因为它不会对每一个连接都耗尽线程资源。 不过在我们继续往下之前，需要纠正有一个常见但十分重要的误解: 异步比多线程要 *快* 。 通常并非如此，实际上，在 Python 里，像我们这样的一个事件循环, 通常会稍慢于服务于一小部分非常活跃的连接的多线程。 在一个没有全局解释器锁 (GIL) 的运行态中，线程在这样的一个负载上将会表现得更好。 异步 I/O 所适用的是那些有很多不常发生的事件, 以及低速与休眠连接的应用 [^5].

### Programming With Callbacks

到这里, 我们已经完成了一个短小的异步框架， 那么如何才能构建一个爬虫？ 要知道即使是一个简单的 URL 获取器 (URL-fetcher), 可能都不是那么简单。

我们以一个还没有抓取和已抓取的 URL 的全局集合开始，

```python

urls_todo = set(['/'])
seen_urls = set(['/']')

```

集合 `seen_urls` 等于 `urls_todo` 加上未完成的 URL, 两个集合同被初始化为根 URL "/" .

抓取一个页面会需要一系列的回调。 当连接建立时， `connected` 回调就会启动, 并向服务器发送一个 GET 请求. 但是它必须等待回应，所以它注册了另一个回调。 如果当回调启动后，它还没有读取到完整的信息，那么它会再次注册，如此往复。

现在, 让我们将这些回调组合到一个 `Fetcher` 对象中。 它需要一个 URL, 一个 socket 对象，还有一个能够存储响应消息的地方。

```python

class Fetcher:
    def __init__(self, url):
        self.response = b''   # Empty array of bytes.
        self.url = url
        self.sock = None

```

调用 `Fetcher.fetch`:

```python

# Method on Fetcher class.
def fetch(self):
    self.sock = socket.socket()
    self.sock.setblocking(False)
    try:
        self.sock.connect(('xkcd.com', 80))
    except BlockingIOError:
        pass

    # Register next callback
    selector.register(self.sock.fileno(),
                      EVENT_WRITE,
                      self.connected)


```

`fetch` 方法首先开始连接一个 socket. 但是注意, 在连接建立之前, 该方法已经返回了。 它必须返回时间循环的控制以等待连接。 要想理解为什么， 想想我们整个应用的结构：

```python

# Begin fetching http://xkcd.com/353/
fetcher = Fetcher('/353/')
fetcher.fetch()

while True:
    events = selector.select()
    for event_key, event_mask in events:
        callback = event_key.data
        callback(event_key, event_mask)

```

当它调用 `select` 时， 所有的事件通知都会在事件循环中进行处理。因此 `fetch` 必须能够控制事件循环，以便于程序知道何时 socket 已经连接. 这样循环才会运行 `connected` 回调，也就是上面 `fetch` 末尾注册的那个函数。

这里是 `connected` 的实现:

```python

# Method on Fetcher class.
def connected(self, key, mask):
    print('connected!')
    selector.unregister(key.fd)
    request = 'GET {} HTTP/1.0\r\nHost: xkcd.com\r\n\r\n'.format(self.url)
    self.sock.send(request.encode('ascii'))

    # Register the next callback.
    selector.register(key.fd,
                      EVENT_READ,
                      self.read_response)

```

方法发送一个 GET 请求。在实际使用中, 一个应用将会检查 `send` 的返回值, 以免整个消息并未被一次性发送出去。不过由于我们的请求很小，应用也并不复杂。 它可以不用考虑太多, 直接调用 `send` ， 然后等待一个响应。当然了，它必须注册另外的一个回调, 并且重新将控制权交还给事件循环。下一个和最后的回调， `read_response`, 处理服务器的回复：

```python

# Method on Fetcher class.
def read_response(self, key, mask):
    global stopped

    chunk = self.sock.recv(4096)  # 4k chunk size.
    if chunk:
        self.response += chunk
    else:
        selector.unregister(key.fd) # Done reading.
        links = self.parse_links()

        # Python set-logic:
        for link in links.difference(seen_urls):
            urls_todo.add(link)
            Fetcher(link).fetch() # <- New Fetcher

        seen_urls.update(links)
        urls_todo.remove(self.url)
        if not urls_todo:
            stopped = True

```

每当选择器 (selector) 看到 socket 是 “可读的”, 就会执行回调。这里的 “可读”，可能表示：socket 有数据或者被关闭。

回调会从 socket 请求 4 kb 的数据。如果不足, `chunk` 将会有多少取多少, 包含能够获取的所有数据。如果多于 4 kb, `chunk` 将会填满 4 kb, 并且 socket 会保持可读的状态，因此事件循环能够在下一个 tick 再次运行这个回调。 当响应完毕， 服务器会关闭 socket，此时 `chunk` 为空。

尚未出现的 `parse_links` 方法返回一些 URL 的集合。 对于每个新的 URL, 我们启动一个新的 fetcher, 没有并发限制。 注意带有回调的异步程序有一个很好的特性: 我们不需要应对共享数据发生变化的信号量， 比如我们在 `seen_urls` 中添加链接。 因为并没有抢占式的多任务，所以在代码中的任一点都不会被中断。

我们添加了一个全局的 `stopped` 变量，并通过它来控制循环:

```python

stopped = False

def loop():
    while not stopped:
        events = selector.select()
        for event_key, event_mask in events:
            callback = event_key.data
            callback()

```

一旦所有的页面被下载完毕， fetcher 停止全局的事件循环, 然后程序退出。

这个示例使得 async 问题十分显然: spaghetti code. 我们需要一些途径来传递一些计算和 I/O 操作，还需要安排多个这样的一系列操作来并发运行。 但是如果没有线程，这一系列的操作就无法被整合到一个单一的函数中: 无论何时一个函数开始一个 I/O 操作，它会显式地保存在未来需要用到的任何状态。 你需要负责考虑完成这样的状态保存代码。

让我们来简单解释一下。传统上使用一个阻塞式的 socket, 我们可以通过一个线程十分简单地获取一个 URL:

```python

# Blocking version.
def fetch(url):
    sock = socket.socket()
    sock.connect(('xkcd.com', 80))
    request = 'GET {} HTTP/1.0\r\nHost: xkcd.com\r\n\r\n'.format(url)
    sock.send(request.encode('ascii'))
    response = b''
    chunk = sock.recv(4096)
    while chunk:
        response += chunk
        chunk = sock.recv(4096)

    # Page is now downloaded.
    links = parse_links(response)
    q.add(links)

```

在两个 socket 操作之间， 这个函数会记住什么状态呢？ 它有 socket, 一个 URL， 和不断增加的 `response` . 运行于一个线程上的函数, 利用了编程语言的基本特性将这个临时状态存储在本地变量中，在它的栈上。 这个函数也有一个 “延续” - 这是说，在 I/O 完成后预期执行的代码。通过存储线程的执行指针，运行期会记住这个 “延续”。 你不必考虑存储这些本地变量和 I/O 完成后的 “延续”。 这是语言所内置的功能。

不过对于一个基于回调的异步框架而言，这些语言特性并没有什么帮助。 当等待 I/O 时，一个函数必须显式地保存它的状态，因为在 I/O 完成以前函数就会返回并且失去它的栈帧。 为了代替本地变量， 我们基于回调的示例将 `sock` 和 `response` 保存为 `self` 的属性， Fetcher 的实例。 为了取代指令指针，它通过注册回调 `connected` 和 `read_response` 存贮其延续。 当应用的特性不断增加时， 我们手动保存回调间状态的复杂性也会随之增加。这样繁琐的记账式代码会使程序员十分头痛。

更糟糕的是，如果一系列中安排中, 下一个回调它抛出异常怎么办？ 假如我们写的 `parse_links` 并不完美，当解析某些 HTML 时它抛出了一个异常：

```
Traceback (most recent call last):
  File "loop-with-callbacks.py", line 111, in <module>
    loop()
  File "loop-with-callbacks.py", line 106, in loop
    callback(event_key, event_mask)
  File "loop-with-callbacks.py", line 51, in read_response
    links = self.parse_links()
  File "loop-with-callbacks.py", line 67, in parse_links
    raise Exception('parse error')
Exception: parse error
```

追踪栈仅指示出事件循环正在执行一个回调。 我们不知道到底什么导致了这个错误。 在链的两端都出现了问题： 我们忘了去向何方和来自哪里。 这种背景信息的丢失叫做 "stack ripping", 在很多时候它都会使调试人员更加糊涂。 stack ripping 也阻止我们对回调设置一个异常捕获, `try / except` 块会将一个函数调用及其后续包装起来。

所以，即使除了长久以来有关多线程与异步在效率上的争论，还有另一个问题 -- 哪一个更加容易出错：如果在同步线程的时候犯了错误，那么线程对数据会十分敏感。但是由于 stack ripping 回调又十分难于调试。


[^1]: Guido introduced the standard asyncio library, called "Tulip" then, at [PyCon 2013](http://pyvideo.org/video/1667/keynote).
[^2]: Even calls to send can block, if the recipient is slow to acknowledge outstanding messages and the system's buffer of outgoing data is full.
[^3]: http://www.kegel.com/c10k.html
[^4]: Python's global interpreter lock prohibits running Python code in parallel in one process anyway. Parallelizing CPU-bound algorithms in Python requires multiple processes, or writing the parallel portions of the code in C. But that is a topic for another day.
[^5]: Jesse listed indications and contraindications for using async in "What Is Async, How Does It Work, And When Should I Use It?":. Mike Bayer compared the throughput of asyncio and multithreading for different workloads in "Asynchronous Python and Databases":
[^6]: For a complex solution to this problem, see http://www.tornadoweb.org/en/stable/stack_context.html
[^7]: The @asyncio.coroutine decorator is not magical. In fact, if it decorates a generator function and the PYTHONASYNCIODEBUG environment variable is not set, the decorator does practically nothing. It just sets an attribute, _is_coroutine, for the convenience of other parts of the framework. It is possible to use asyncio with bare generators not decorated with @asyncio.coroutine at all.
[^8]: Python 3.5's built-in coroutines are described in PEP 492, "Coroutines with async and await syntax."
[^9]: This future has many deficiencies. For example, once this future is resolved, a coroutine that yields it should resume immediately instead of pausing, but with our code it does not. See asyncio's Future class for a complete implementation.↩
[^10]: In fact, this is exactly how "yield from" works in CPython. A function increments its instruction pointer before executing each statement. But after the outer generator executes "yield from", it subtracts 1 from its instruction pointer to keep itself pinned at the "yield from" statement. Then it yields to its caller. The cycle repeats until the inner generator throws StopIteration, at which point the outer generator finally allows itself to advance to the next instruction.↩
[^11]: https://docs.python.org/3/library/queue.html
[^12]: https://docs.python.org/3/library/asyncio-sync.html
[^13]: The actual asyncio.Queue implementation uses an asyncio.Event in place of the Future shown here. The difference is an Event can be reset, whereas a Future cannot transition from resolved back to pending.
[^14]: https://glyph.twistedmatrix.com/2014/02/unyielding.html
