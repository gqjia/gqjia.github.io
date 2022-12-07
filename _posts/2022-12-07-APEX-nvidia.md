---
title: 如何安装 apex
date: 2022-12-07 14:53:01
---


最近要搞 CPT 和 BART 的预训练，然后就发现了 Megatron-LM 这个仓库，本以为一切会非常顺利，结果发现这个库的使用需要安装 apex ！

瞬间痛苦的回忆涌上心头。

简单记录一下这次是怎么避开那些坑的。

其实关键是下载 22.04-dev 这个分支。

```shell
cd apex
python setup.py install --cpp_ext --cuda_ext
```


直接按照官方 github 上的操作会有各种问题。
后续看看 Megatron-LM 可不可以避免使用 apex 吧。毕竟现在 pytorch 都已经 2.0 了嘛。
