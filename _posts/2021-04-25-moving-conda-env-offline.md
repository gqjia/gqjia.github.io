---
title: 离线环境下迁移anaconda虚拟环境的一些方法
date: 2021-04-25 11:37:31
---

在线的环境下迁移anaconda的环境是真的很方便。
最常用的：
```shell
pip freeze > requirements.txt
conda create -n [new_env] python=3.x.x
pip install -r requirements.txt
conda install --yes --file requirements.txt
```

```shell
conda list --explicit > requirement.txt
conda create -n [new_env] --file requirement.txt
```
或者：
```shell
conda env export > requirement.yml
conda env create -f requirement.yml
```


而离线环境就比较难受了。

在同一个服务器下最好用的方案是采用anaconda创建环境时clone原先的环境：
```shell
conda create -n [new_env] --clone [env]
```
这一方法最好是两台服务器上的anaconda采用同一版本，
不然还会请求下载一些需要安装的库。

但是当我们将一台离线服务器上的虚拟环境迁移到另一台离线服务器上，
这个方法就没那么好用了。
因为难免会遇到离线安装的包，同样会遇到下载请求。
这样的情况用conda-pack这个包会比较好一些：
```shell
conda pack -n my_env
conda pack -n my_env -o out_name.tar.gz
conda pack -p /explicit/path/to/my_env

mkdir -p my_env
tar -xzf my_env.tar.gz -C my_env
./my_env/bin/python
source my_env/bin/activate
```
虽然这样的做法在遇到可编辑模式的包时，会出现打包错误的情况，
但是已经可以解决绝大部分的问题了。

其实简单粗暴的方法也有，
可以直接把原先path/to/anaconda/env/路径下的虚拟环境拷贝到新的服务器上，
这样可以继续使用的。

这样的做法远好于把整个path/to/anaconda/文件拷贝到新的服务器上，
可以继续在新的环境下安装包。
直接拷贝整个环境则需要将所有的环境变量进行修改，
如果新服务器路径跟原先服务器路径一致就还好，如果不一致，整个过程将非常痛苦。
（前者其实也需要修改虚拟环境下的环境变量）


最后介绍一下最终的方案，同时拷贝path/to/anaconda/env/下的环境和path/to/anaconda/pkgs/，把pkgs/文件放到对应位置，并且删除pkgs/内的urls和urls.txt。
```shell
conda create -n [new_envs_name] --clone[path to envs_names] --offline
```

如果原先安装虚拟环境的时候，存在离线安装的包。这样的做法也会报错，提示找不到对应包的路径。
简单粗暴的方法是把原先使用的的包迁移到对应位置的文件夹内，或者创建一个指定对应包所在文件夹的软链。
```shell
ln -s [pre/conda/pkg/path] [target/conda/pkg/path]
```
