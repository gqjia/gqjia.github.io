---
title: 【centos】centOS7 64位安装steam
date: 2018-11-17 08:02:31
---

> wget http://springdale.math.ias.edu/data/puias/unsupported/7/x86_64//dnf-0.6.4-2.sdl7.noarch.rpm

> wget http://springdale.math.ias.edu/data/puias/unsupported/7/x86_64/dnf-conf-0.6.4-2.sdl7.noarch.rpm

> wget http://springdale.math.ias.edu/data/puias/unsupported/7/x86_64/python-dnf-0.6.4-2.sdl7.noarch.rpm

> yum install dnf-0.6.4-2.sdl7.noarch.rpm dnf-conf-0.6.4-2.sdl7.noarch.rpm python-dnf-0.6.4-2.sdl7.noarch.rpm

> dnf install steam

> steam

出现bug

> LDPRELOAD-'/user/$LIB/libstdc++.so.6' LIBGL_DRI3_DISABLE=1 steam

启动steam
>steam  
