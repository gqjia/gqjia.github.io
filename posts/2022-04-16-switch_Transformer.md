title: Switch Transformer
date: 2022-04-18 11:01:11



## 论文摘要

Switch Transformer在网络结构上最大的改进是Sparse routing的稀疏结构，相比于OpenAI在GPT-3里所使用的Sparse Attention，需要用到稀疏算子而很难发挥GPU、TPU硬件性能的问题。Switch Transformer不需要稀疏算子，可以更好的适应GPU、TPU等硬件。Switch Transformer虽然有1.6万亿参数，但通过Sparse routing的改进，每轮迭代只会触发部分Expert的计算，而每个token也只会路由给一个Expert，所以对算力的需求并没有随着参数量的增加而大幅增长，使得这个模型更加容易训练。数据并行、模型并行、Expert并行的并行策略设计，在MoE网络结构上能够获得更低的通信开销，提高并行的效率[^2]。

在深度学习中，模型通常对所有输入重复使用相同的参数。而MoE模型则是为每个例子选择不同的参数。于是一个稀疏激活的模型（参数数量惊人但计算成本不变）诞生了。然而，尽管取得了一些显著的成功，但由于复杂性、通信成本和训练的不稳定性，模型广泛采用仍需优化。我们用Switch Transformer来解决这些问题。同时，我们简化了MoE路由算法，设计了直观的改进模型，降低了通信和计算成本。我们提出的训练方法减轻了不稳定性，并且我们首次展示了用较低精度（bfloat16）格式训练大型稀疏模型的可能性。同时，基于T5 Base和T5 Large（Raffel et al.，2019）设计模型，以在相同计算资源的情况下获得高达7倍的预训练速度。这些改进扩展到多语言设置中，我们在所有101种语言中测量mT5基本版本的增益。最后，通过在“巨大的干净的爬虫语料库”上预训练多达万亿个参数的模型，提高了当前语言模型的规模，并实现了比T5-XXL模型4倍的加速[^2]。



### Sparse Attention 是什么？

### GPT-3 是如何使用Sparse Attention 的？

### MoE 网络结构是什么？

MoE(Mix of Expert)是一种神经网络，也属于一种combine的模型，上个世纪90年代被提出。适用于数据集中的数据产生方式不同。不同于一般的神经网络的是它根据数据进行分离训练多个模型，各个模型被称为专家，而门控模块用于选择使用哪个专家，模型的实际输出为各个模型的输出与门控模型的权重组合。各个专家模型可采用不同的函数（各种线性或非线性函数）。混合专家系统就是将多个模型整合到一个单独的任务中[^2]。



## 模型结构













## 参考资料

[^1]:[如何评价谷歌推出1.6万亿参数超级语言模型Switch Transformer？](https://www.zhihu.com/question/439162583)
[^2]:[NLP炼丹笔记：Switch Transformers 朴实无华 大招秒杀](https://zhuanlan.zhihu.com/p/344702054)
[^3]:[Switch Transformer: 高效稀疏的万亿参数Transformer](https://zhuanlan.zhihu.com/p/352462938)
[^4]:[MoE: 稀疏门控制的专家混合层](https://mp.weixin.qq.com/s?__biz=MzI4ODg3NDY2NQ==&mid=2247484515&idx=1&sn=eb0960b55ef1d09d23082d302d003edf&chksm=ec368da5db4104b3940658f912df98500a4a3a640913db1518b856b5fe60b88dd997e6ea49b2&token=956863618&lang=zh_CN#rd)
[^5]:[Mesh-Tensorflow: 广义分布式训练大模型](https://mp.weixin.qq.com/s?__biz=MzI4ODg3NDY2NQ==&mid=2247484575&idx=1&sn=d5f13521a54448a9278d6c6af390da6c&chksm=ec368d59db41044f03e37725f543d9ac05aff9fea5471003b5edfd8628c14550957f68a78935&token=956863618&lang=zh_CN#rd)
[^6]:[T5: 文本到文本的Transformer迁移学习](https://mp.weixin.qq.com/s?__biz=MzI4ODg3NDY2NQ==&mid=2247484599&idx=1&sn=2d2e8eb9d4214eb2342f27b71ebc8567&chksm=ec368d71db4104670c9eb32cc7ff9e75589ff176f764510287ef483e5ea99e7f1cb611154eb5&token=956863618&lang=zh_CN#rd)
