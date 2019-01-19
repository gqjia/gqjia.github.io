---
title: 【kaggle】【quora insincere questions classification】2 Capsule Net
date: 2019-01-19 19:59:00
---


#### Capsule网络想解决什么问题？

Capsule网络主要想解决卷积神经网络（Convolutional Neural Networks）存在的一些缺陷，比如说信息丢失，视角变化等。

卷积神经网络中的池化（Pooling）操作会导致局部内容的准确位置和相对空间关系等信息丢失，对此Hinton大神还做过一些很意思的评价。

>“ The pooling operation used in convolutional neural networks is a big mistake and the fact that it works so well is a disaster.”-- Geoffrey Hinton

卷积神经网络对于视角变化的学习效率是很低的，需要大量不同视角的图片作为训练数据或者使用更多的卷积过滤器（Filter），模型才能够处理视角变化。


#### Capsule是什么？

一个Capsule指的是一组具备一定特性的神经元（Neurons），它会根据从下层Capsules接收到的活跃度向量（Activity Vector）来计算自身的活跃度向量，并将其发送给上层的Capsules。活跃度向量可以用来表征实体（Entity），向量长度表示实体存在的概率大小，向量在空间中的方向表示实体的实例化参数（Instantiation Parameters）。

在计算机图形学（Computer Graphics）中，我们可以根据物体的层级化几何参数，生成物体对应的视觉图片，这个过程被称为渲染（Rendering）。从反向图形学（Inverse Graphics）的角度，我们可以把激活一个Capsule想象成是对渲染过程的反转，从视觉图片中重构物体的实例化参数。

那活跃度向量的每个维度都代表什么实例化参数呢？我们可以看一个数字Capsule的例子，在这个例子中每个数字Capsule会输出一个16维活跃度向量，如果我们对向量的一个指定维度进行微小的调整，然后再根据调整后的活跃度向量将数字Capsule重构成数字图片，我们会发现向量的某些维度跟数字的特定变换有关系。


### 参考文献：
1. [Dynamic Routing Between Capsules](https://arxiv.org/pdf/1710.09829.pdf)
