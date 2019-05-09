---
title: 【Python】 sys模块
date: 2019-05-08 16:12:00
---

sys模块用于提供对python解释器的相关操作。

#### 常用函数
```txt
sys.argv   命令行参数List，第一个元素是程序本身路径
sys.modules 返回系统导入的模块字段，key是模块名，value是模块
sys.exit(n)        退出程序，正常退出时exit(0)
sys.version        获取Python解释程序的版本信息
sys.maxint         最大的Int值
sys.path           返回模块的搜索路径，初始化时使用PYTHONPATH环境变量的值
sys.platform       返回操作系统平台名称
sys.stdout.write('please:')
val = sys.stdin.readline()[:-1]
sys.modules.keys() 返回所有已经导入的模块名
sys.modules.values() 返回所有已经导入的模块
sys.exc_info()     获取当前正在处理的异常类,exc_type、exc_value、exc_traceback当前处理的异常详细信息
sys.exit(n)        退出程序，正常退出时exit(0)
sys.hexversion     获取Python解释程序的版本值，16进制格式如：0x020403F0
sys.version        获取Python解释程序的
sys.api_version    解释器的C的API版本
sys.version_info
‘final’表示最终,也有’candidate’表示候选，serial表示版本级别，是否有后继的发行
sys.displayhook(value)      如果value非空，这个函数会把他输出到sys.stdout，并且将他保存进__builtin__._.指在python的交互式解释器里，’_’ 代表上次你输入得到的结果，hook是钩子的意思，将上次的结果钩过来
sys.getdefaultencoding()    返回当前你所用的默认的字符编码格式
sys.getfilesystemencoding() 返回将Unicode文件名转换成系统文件名的编码的名字
sys.setdefaultencoding(name)用来设置当前默认的字符编码，如果name和任何一个可用的编码都不匹配，抛出 LookupError，这个函数只会被site模块的sitecustomize使用，一旦别site模块使用了，他会从sys模块移除
sys.builtin_module_names    Python解释器导入的模块列表
sys.executable              Python解释程序路径
sys.getwindowsversion()     获取Windows的版本
sys.copyright      记录python版权相关的东西
sys.byteorder      本地字节规则的指示器，big-endian平台的值是’big’,little-endian平台的值是’little’
sys.exc_clear()    用来清除当前线程所出现的当前的或最近的错误信息
sys.exec_prefix    返回平台独立的python文件安装的位置
sys.stderr         错误输出
sys.stdin          标准输入
sys.stdout         标准输出
sys.platform       返回操作系统平台名称
sys.path           返回模块的搜索路径，初始化时使用PYTHONPATH环境变量的值
sys.maxunicode     最大的Unicode值
sys.maxint         最大的Int值
sys.version        获取Python解释程序的版本信息
sys.hexversion     获取Python解释程序的版本值，16进制格式如：0x020403F0
```


[sys — System-specific parameters and functions](https://docs.python.org/3/library/sys.html)
