---
title: Linux普通用户在anaconda虚拟环境中使用高版本gcc的方法
date: 2021-01-21 16:25:31
---
首先创建一个虚拟环境：

```shell
conda create -n test_env python=3.6
```

进入这一虚拟环境，后续会在这虚拟环境中使用gcc：
```shell
conda activate test_env
```

查看当前版本gcc：
```shell
gcc -v
```

安装gcc_impl_linux-64：
```shell
conda install gcc_impl_linux-64
```

进入虚拟环境下的bin目录，创建指向libexec下gcc的连接:
```shell
cd ~/anaconda3/env/test_env/bin
ln -s gcc ../libexec/gcc/x86_64-conda-linux-gnu/9.3.0/gcc
```

安装gcc_linux-64:
```shell
conda install gcc_linux-64
```

查看当前版本，成功安装9.3.0版本gcc：
```shell
gcc -v
```
