---

layout: post
title: 在 Shell 脚本中调用另一脚本的三种方式
tag: shell
categories: posts
published: false

---

先来说一下主要以下有几种方式：

- **fork**: 如果脚本有执行权限的话，`path/to/foo.sh`。如果没有，`sh path/to/foo.sh`。
- **exec**: `exec path/to/foo.sh`
- **source**: `source path/to/foo.sh`

### fork

`fork` 是最普通的, 就是直接在脚本里面用 `path/to/foo.sh` 来调用
 foo.sh 这个脚本，比如如果是 foo.sh 在当前目录下，就是 `./foo.sh`。运行的时候 terminal 会新开一个子 Shell 执行脚本 foo.sh，子 Shell 执行的时候, 父 Shell 还在。子 Shell 执行完毕后返回父 Shell。 子 Shell 从父 Shell 继承环境变量，但是子 Shell 中的环境变量不会带回父 Shell。

### exec

`exec` 与 `fork` 不同，不需要新开一个子 Shell 来执行被调用的脚本.  被调用的脚本与父脚本在同一个 Shell 内执行。但是使用 `exec` 调用一个新脚本以后, 父脚本中 `exec` 行之后的内容就不会再执行了。这是 `exec` 和 `source` 的区别.

### source

与 `fork` 的区别是不新开一个子 Shell 来执行被调用的脚本，而是在同一个 Shell 中执行. 所以被调用的脚本中声明的变量和环境变量, 都可以在主脚本中进行获取和使用。

其实从命名上可以感知到其中的细微区别，下面通过两个脚本来体会三种调用方式的不同:

第一个脚本，我们命名为 `1.sh`:

```sh
#!/usr/bin/env bash

A=1

echo "before exec/source/fork: PID for 1.sh = $$"

export A
echo "In 1.sh: variable A=$A"

case $1 in
        --exec)
                echo -e "==> using exec…\n"
                exec ./2.sh ;;
        --source)
                echo -e "==> using source…\n"
                . ./2.sh ;;
        *)
                echo -e "==> using fork by default…\n"
                ./2.sh ;;
esac

echo "after exec/source/fork: PID for 1.sh = $$"
echo -e "In 1.sh: variable A=$A\n"
```

第二个脚本，我们命名为 `2.sh`：

```sh
#!/usr/bin/env bash

echo "PID for 2.sh = $$"
echo "In 2.sh get variable A=$A from 1.sh"

A=2
export A

echo -e "In 2.sh: variable A=$A\n"
```

注：这两个脚本中的参数 `$$` 用于返回脚本的 PID , 也就是进程 ID。这个例子是想通过显示 PID 判断两个脚本是分开执行还是同一进程里执行，也就是是否有新开子 Shell。当执行完脚本 `2.sh` 后，脚本 `1.sh` 后面的内容是否还执行。

`chmod +x 1.sh 2.sh` 给两个脚本加上可执行权限后执行情况：

### fork

![fork]({{ site.baseurl }}{{ site.images }}/posts/fork.png)

`fork` 方式可以看出，两个脚本都执行了，运行顺序为1-2-1，从两者的 PID 值(1.sh PID=82266, 2.sh PID=82267)，可以看出，两个脚本是分成两个进程运行的。

### exec

![exec]({{ site.baseurl }}{{ site.images }}/posts/exec.png)

`exec` 方式运行的结果是，2.sh 执行完成后，不再回到 1.sh。运行顺序为 1-2。从 PID 值看，两者是在同一进程 PID=82287 中运行的。

### source

![source]({{ site.baseurl }}{{ site.images }}/posts/source.png)

source方式的结果是两者在同一进程里运行。该方式相当于把两个脚本先合并再运行。

Command | Explanation
:---:   | :---:
fork    | 新开一个子 Shell 执行，子 Shell 可以从父 Shell 继承环境变量，但是子 Shell 中的环境变量不会带回给父 Shell。
exec    | 在同一个 Shell 内执行，但是父脚本中 `exec` 行之后的内容就不会再执行了
source  | 在同一个 Shell 中执行，在被调用的脚本中声明的变量和环境变量, 都可以在主脚本中进行获取和使用，相当于合并两个脚本在执行。

参考：
- [在shell脚本中调用另一个脚本的三种不同方法(fork, exec, source)](http://www.361way.com/shell-process/1126.html)
