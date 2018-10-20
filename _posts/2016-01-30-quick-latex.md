---

layout: post
title: 从零开始 LaTeX 快速入门
tag: LaTeX
categories: posts
math: y
published: true

---

此篇为写给一些想快速入门 LaTeX 的朋友. 为什么叫从零开始? 因为我就是从零开始学会的 LaTeX。本人学识与能力有限，以下内容如有纰漏或错误，欢迎来信纠正。

我几乎没有看到有将 LaTeX 与网页的渲染进行比较学习，这也可以算作是迁移学习，只要稍微懂得一点网页的知识都可以了解 LaTeX 的运作方式。本文并不十分正统，仅当快速学习的经验分享。

LaTeX 始终是个排版工具，快速使用并且活学活用才是硬道理。笔者从第一次使用 LaTeX 从完成几十页毕业论文的 LaTeX 大工程(文末有提供我的毕业论文 LaTeX 源文件)，期间不过两三个月而已， 而除去内容上的准备，在 LaTeX 调整样式上也不过几周时间。所以要相信只要得法， 其实 LaTeX 很简单，不要因为有一些 LaTeX 学习曲线很陡的说法而心生畏惧。

### LaTeX概览

摘自维基百科：

>LaTeX， 是一种基于TEX的排版系统，由美国电脑学家莱斯利·兰伯特在20世纪80年代初期开发，利用这种格式，即使用户没有排版和程序设计的知识也可以充分发挥由TEX所提供的强大功能，能在几天，甚至几小时内生成很多具有书籍质量的印刷品。对于生成复杂表格和数学公式，这一点表现得尤为突出。因此它非常适用于生成高印刷质量的科技和数学类文档。这个系统同样适用于生成从简单的信件到完整书籍的所有其他种类的文档。

简单点说：LaTeX 基于 TeX，主要目的是为了方便排版。在学术界的论文，尤其是数学、计算机等学科论文都是由 LaTeX 编写, 因为用它写数学公式非常漂亮。

**我的一点理解：**

在稍微了解一点 LaTeX 后，你会发现 **LaTeX 的工作方式类似 web page**，都是由源文件（.tex or .html）经由引擎（TeX or browser）渲染产生最终效果（得到 PDF 文件 或者 生成页面）。两者极其神似，包括语法规则与工作方式。所以呢，与 HTML 一样，入门其实很简单。

![sketch]({{ site.baseurl }}{{ site.images }}/posts/sketch.png)

一般的规范写法中都是在 HTML 文件中写入 web page 的结构与内容，再由 css 控制页面生成的样式。当然你也可以选择在 HTML 中直接写入样式内容，不过这并不提倡。同样，在 LaTeX 有着同样的情况，你可以在 tex 源文件中同时写入内容和样式，也可以内容与样式分离，以网络上流传广泛的 [清华大学 LaTeX 模板](https://github.com/xueruini/thuthesis)为例，以.cls(class)结尾的 thuthesis.cls 便可看作是与 css 起到同样作用的样式文件。

LaTeX 有所谓宏包的概念，`\usepackage{foo}` 即可使用宏包 foo 中定义的内容。所谓宏包就是一些写好的内容打包出来以便大家使用而已。这跟 C 语言的 `include` 是一致的，将文件加载进来进行使用。利用宏包，我们可以使用很多现成的好用的样式。当然了，如果要编写一个自己的个性化的宏包也是可以的，不过需要学习成本。

初期的话，我们可以选择一个 LaTeX 模板进行改造。不过第一次见到一些模板，可能会对其中很多文件的作用一头雾水. 下面是简单的介绍，详细内容可见[在 LaTeX 中进行文学编程](http://liam0205.me/2015/01/23/literate-programming-in-latex/)，当然更多介绍的话可以自行搜索。


LaTeX模板常见文件类型 | 功能简要介绍
:---:                 | :---:
.dtx                  | **D**ocumented La**T**e**X** sources，宏包重要部分
.ins                  | installation，控制 TeX 从 .dtx 文件里释放宏包文件
.cfg                  | config， 配置文件，可由上面两个文件生成
.sty                  | style files，使用<code>\usepackage{...}</code>命令进行加载
.cls                  | classes files，类文件，使用<code>\documentclass{...}</code>命令进行加载
.aux                  | auxiliary， 辅助文件，不影响正常使用
.bst                  | BibTeX style file，用来控制参考文献样式

class 与 style 似乎十分相像，它们在功能上的确很相似，但是也有区别。[这里](https://tug.org/pracjourn/2005-3/asknelly/nelly-sty-&-cls.pdf) 是关于 .cls 与 .sty 文件的区别.


额外推荐阅读材料: 来自北京大学李东风老师的 [LaTeX 排版心得](http://www.math.pku.edu.cn/teachers/lidf/docs/textrick/tricks.pdf).

### 安装配置LaTeX

LaTeX 配置环境很简单，只需 2 步：

1. 根据平台选择一个 **TeX 发行版** 进行安装，建议选择最全功能最多的版本。

    TeX 发行版的概念相当于 Linux 及其发行版，Linux 内核虽然只有一个，但是有很多基于内核的不同特色的 Linux 发行版，Ubuntu，Fedora 等等不胜枚举。

    OS             | TeX Distribution
    :---:          | :---:
    Windows        | [CTeX](http://www.ctex.org/CTeXDownload)
    Mac            | [MacTeX](http://tug.org/mactex/)
    Windows, Linux | [TeXLive](https://www.tug.org/texlive/)

    Windows 用户推荐 TeXlive，不推荐 CTeX。我一开始安装的是 CTeX，在 TeXstudio 里面时常有一些莫名其妙的错误，比如明明定义了一个命令，在 log 里面还是会显示 `error：undefined control sequence`，换了 TeXlive 就没有那些莫名其妙的错误了。

    不过 TeXlive 在线安装太慢了，安装包太大，两三个 G，这里是百度云链接 [2015 TeXlive 离线安装包](http://pan.baidu.com/s/1jHfUzWy)， 提取密码2cj2，解压缩后运行 install-tl-windows.bat 即可。Mac用户推荐使用 MacTeX.

2. 选择一个合适的 **LaTeX 编辑器**。

    在安装好LaTeX环境以后，通常都会有一个自带的编辑器，比如 CTex 的WinEdt， MacTeX的TeXShop， 不过功能并不强大，好比 Windows 记事本，只有一些基本的文本编辑功能。

    在这里推荐一个我觉得还不错的LaTeX编辑器：**TeXstudio**。我试过 WinEdt，TeXnicle，不过都比不上 TeXstudio。在 WinEdt 下面无法编译的文件，居然可以在 TeXstudio 中编译生成最终效果 (虽然 log 里面显示 error，但的确产生了效果)。总之，用 TeXstudio 就对了, 而且它是用 qt 写的，还跨平台。

    TeXmacs 有兴趣的也可以了解一下，[王垠也在博客中推荐过](http://www.yinwang.org/blog-cn/2012/09/18/texmacs)。

### 开始第一个 LaTeX 文档

打开 TeXstudio，新建一个 TeX 文件，写入以下内容：

``` tex
\documentclass{article}
\begin{document}
Here comes \LaTeX!
\end{document}
```

点击 <kbd>F5</kbd>（默认快捷键）`compile and view`，即可看到效果。

![TeXstudio]({{ site.baseurl }}{{site.images}}/posts/texstudio.png)

至此，一个极简易的 LaTeX 文档已经完成。以后要做的事情不过是多用多查，熟能生巧。此外记得找本 LaTeX 的书籍看一下，一来对于更为精细的知识做一个了解，二来可以作为工具书查询。我经常查的是 <<LaTeX入门与提高 第二版>>。

#### LaTeX数学公式

学习 LaTeX 的一大初衷便是为了写漂亮的数学公式。而于我个人而言，数学公式的练习始于 markdown，很多 markdown 编辑器是支持 LaTeX 数学公式的，比如 haroopad。

以下内容直接在支持数学公式的 markdown 编辑器中即可操作，而且是即时显示效果，对新手很有帮助。如果使用 haroopad，请在 **偏好设置** 中 **启用数学表达式**。

**学会写 LaTeX 公式，只需要了解 4 个概念：**

1. 数学公式环境。

    LaTeX 的数学模式有两种：行内模式(inline)和行间模式(display)。前者在正文的行文中，插入数学公式；后者独立排列单独成行。

    在行文中，使用 <code>\$ ... \$</code> 可以插入行内公式，使用 <code>\$\$ ... \$\$</code> 可以插入行间公式，如果需要对行间公式进行编号，可以使用 equation 环境.

2. 控制序列。

    凡是键盘不能够直接表示的符号或者起着特定作用的皆有命令，类似转义，叫做**控制序列（control sequence）**。比如求和符号$\sum$对应的命令为 <code>\sum</code>.

3. 上下标。

    <code>_{...}</code>表示下标，<code>^{...}</code>表示上标。它默认只作用于之后的一个字符，如果想对连续的几个字符起作用，请将这些字符用花括号{}括起来， 也就是下面分组的概念。

4. 分组。

    很简单，就是用<code>{...}</code>将内容包含起来视作整体，比如上下标很长的时候。遇到什么时候得到的效果不是预期，那么很可能你需要加个分组，也就是添个大括号<code>{...}</code>.

| LaTeX命令                       | 预览效果       |
| :--------:                      | :--------:     |
| <code>\$ x_i \$</code>          | $x_i$          |
| <code>\$ x^2 \$</code>          | $x^2$          |
| <code>\$ x^ {y^z}\$</code>      | $x^{y^z}$      |
| <code>\$ \int_a^b f(x)\$</code> | $\int_a^bf(x)$ |
| <code>\$ \frac ab \$</code>     | $\frac ab$     |

有了这几个概念以后，再动手写几个就大概懂了。无论多么复杂的公式都是有一个个简单的东西构成。推荐一个网站：[MathJax basic tutorial ](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference).

#### LaTeX 中文支持

不同环境具体操作有所不同，造成这种不同的主要是各平台下的字体不同。下面介绍 Windows 与 Mac 平台。

Windows平台比较简单， 引入 CJK 宏包并应用 CJK 环境即可。

```tex
\documentclass[11pt]{article}  %百分号表示注释
\usepackage{CJK}               %引入CJK宏包
\begin{document}               %begin与end成对出现
\begin{CJK}{UTF8}{song}        %应用CJK环境
你好
\end{CJK}
\end{document}
```

LaTeX将

```tex
\begin{...}
content
\end{...}
```

称为 <code>...</code> 环境。在对应环境中 content 产生对应效果。

![winedt]({{ site.baseurl }}{{site.images}}/posts/winedt.png)

还有一个更方便的方式，直接使用<code>ctexart</code>模板:

```tex
\documentclass[UTF8]{ctexart}
```

或者使用 ctex 宏包:

```tex
\usepackage{ctex}
```

如果 Mac 下直接使用 ctex 有问题的话， 可以使用 xeCJK ，不过可能需要稍微多几个操作，除了引入xeCJK宏包，还要设置字体名称。测试系统为osx 10.11.3，
关于设置字体名称，spotlight 输入 font 打开 Mac 的字体册，从字体中选择一个，将其名称填入，如华文楷体的名称为 STKaiti 。
如果没有显示字体名称，请 <kbd>command</kbd> + <kbd>I</kbd> 或在显示-->显示字体信息即可。

![font]({{ site.baseurl }}{{site.images}}/posts/mac-font.png)

![mac-chinese]({{ site.baseurl }}{{site.images}}/posts/mac-chinese.png)

### LaTeX 资源推荐

- [Detexify LaTeX handwritten symbol recognition](http://detexify.kirelabs.org/classify.html).

    通过手写识别 LaTeX 符号，识别率很高。尤其是当看到一个符号却不知道其 LaTeX 命令的时候它很有用。只要画出记忆中符号的样子，就会自动出现各种可能想要的表示方法。

- [LaTeX公式编辑器](http://zh.numberempire.com/texequationeditor/equationeditor.php)

    对于尚不熟悉的人书写 LaTeX 公式提供一点便利。

- [在线LaTeX编辑器shareLaTeX](https://cn.sharelatex.com/)

    好处就是不用本地搭建环境，有中文界面，直接在线操作。还有很多 LaTeX 模板可供选择。

###  对于 LaTeX 初学者的建议

起初，我想通过将清华大学的 LaTeX 模板改造为我的本科院校给出的 word 模板样式，不过最后这条路没有走到底。反思其原因是一直想写个宏包出来，即从 thuthesis.cls 到 szuthesis.cls，最好能够一次性做出一个模板出来。但是始终由于各种原因没有时间给我去折腾，太多错误无法解决。最后我选择基于 ctexart 的基本样式进行修改，在 tex 源文件混杂了样式内容，从源代码的角度看虽然不漂亮，但是对于完成本科论文绰绰有余了。在 tex 源文件里修改 ctexart 的各种样式实在是容易上手的多。

**初学者轻易不要尝试修改现有的模板样式文件**，除非你知道如何写一个宏包。完成样式即可，不用在乎源代码是否优雅。

下面修改样式的过程的一些经验：

- 查看宏包说明

    比如我装的 TeXLive 2015 在 `C://texlive` 目录下，打开 `C://texlive/2015/doc.html`，你就会发现各种文档。请**仔细阅读一遍ctex.pdf**会很有用。

- 查看宏包手册

    打开 cmd， 输入 `texdoc 你想要查询的宏包名 `， 比如 <code>texdoc caption</code>，就会打开 caption 宏包手册。诚然可以网上查找解决办法，但是如果有空的话必然是查看官方手册更靠谱更全面.

#### 额外推荐

我的本科论文 LaTeX 源文件已经放到了 [GitHub](https://github.com/liuchengxu/szuthesis) 上，对于初次使用 LaTeX 写论文的人应当具有一定的借鉴意义，在源文件中我做出了诸多注解。此外, 论文关于推荐系统，如果有人做相关方向也可看一下。

- [论文：szuthesis](https://github.com/liuchengxu/szuthesis)
- [论文简介](https://liuchengxu.github.io/szuthesis/)
- [论文 beamer slide](https://liuchengxu.github.io/szuthesis/dissertation_defence.pdf)
- [beamer 指南](http://math.ecnu.edu.cn/~latex/slides/beamer/beamer_guide_cn.pdf)
- [Beamer 演示学习笔记](https://bbs.pku.edu.cn/attach/cb/40/cb401e254626b3f9/beamerlog-1112.pdf)

如果您已经懂得了基础操作，不妨看一下我在 CSDN 记录的一些 LaTeX 使用注意点，里面积累了我在 LaTeX 使用过程中的很多经验：[LaTeX实战经验：新手须知](http://blog.csdn.net/simple_the_best/article/details/51244631)
