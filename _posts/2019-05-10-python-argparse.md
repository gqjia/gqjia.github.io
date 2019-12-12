---
title: 【Python】 argparse模块
date: 2019-05-10 13:57:00
---

argparse是python标准库里面用来处理命令行参数的库


### 使用步骤

1. import argparse    首先导入模块
2. parser = argparse.ArgumentParser()    创建一个解析对象
3. parser.add_argument()    向该对象中添加你要关注的命令行参数和选项
4. parser.parse_args()    进行解析


### ArgumentParser()
```txt
prog=None     - 程序名
description=None,    - help时显示的开始文字
epilog=None,     - help时显示的结尾文字
parents=[],        -若与其他参数的一些内容一样，可以继承
formatter_class=argparse.HelpFormatter,     - 自定义帮助信息的格式
prefix_chars='-',    - 命令的前缀，默认是‘-’
fromfile_prefix_chars=None,     - 命令行参数从文件中读取
argument_default=None,    - 设置一个全局的选项缺省值，一般每个选项单独设置
conflict_handler='error',     - 定义两个add_argument中添加的选项名字发生冲突时怎么
处理，默认处理是抛出异常
add_help=True    - 是否增加-h/--help选项，默认是True
```


### add_argument()
```txt
name or flags...    - 必选，指定参数的形式，一般写两个，一个短参数，一个长参数
action	表示值赋予键的方式，这里用到的是bool类型，action意思是当读取的参数中出现指定参
数的时候的行为
help		可以写帮助信息
required    - 必需参数，通常-f这样的选项是可选的，但是如果required=True那么就是必须的
了
type   - 指定参数类型
choices    - 设置参数的范围，如果choice中的类型不是字符串，要指定type
表示该参数能接受的值只能来自某几个值候选值中，除此之外会报错，用choice参数即可
nargs    - 指定这个参数后面的value有多少个，默认为1
nargs还可以'*'用来表示如果有该位置参数输入的话，之后所有的输入都将作为该位置参数的值；
‘+’表示读取至少1个该位置参数。'?'表示该位置参数要么没有，要么就只要一个。（PS：跟正则表
达式的符号用途一致。
```

---
转自[一个运维菜鸟的成神路-python3中argparse模块](https://www.cnblogs.com/dengtou/p/8413609.html)
