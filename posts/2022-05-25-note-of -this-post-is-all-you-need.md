---
title: This post is all you need
date: 2022-05-25 14:48:23
---





**月来客栈**大佬分享的博客，需要认真学习一下。



## 模型结构

传统的 Encoder-Decoder 架构在建模过程中，下一个时刻的计算过程会 依赖于上一个时刻的输出，而这种固有的属性就限制了传统的 Encoder-Decoder 模型就不能以并行的方式进行计算。因此，谷歌提出了 Transformer 架构来解决 这一问题。当然，Transformer 架构的优点在于它完全摈弃了传统 的循环结构，取而代之的是只通过注意力机制来计算模型输入与输出的隐含表示， 而这种注意力的名字就是自注意力机制（self-attention）。所谓自注意力机制就是通过某种运算来直接计算得到句子在编码 过程中每个位置上的注意力权重；然后再以权重和的形式来计算得到整个句子的 隐含向量表示。最终，Transformer 架构就是基于这种的自注意力机制而构建的 Encoder-Decoder 模型。

![image-20220525145750760](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205251457980.png)



### 自注意力机制

所谓的自注意力机制其实就是论文中所指代的 “Scaled Dot-Product Attention”。注意力机制可以描述为将 query 和一系列的 key-value 对映射到某个输出的过程，而这个输出的向量就是根 据 query 和 key 计算得到的权重作用于 value 上的权重和。

![image-20220525150127201](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205251501394.png)

输出向量的计算公式为：

![image-20220525151057619](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205251510676.png)

之所以要进行缩放这一步是因为通过实验作者发现，对于较大的 k d 来说在完 成 T QK 后将会得到很大的值，而这将导致在经过 sofrmax 操作后产生非常小的梯 度，不利于网络的训练。

模型在对当前位置的信息进行 编码时，会过度的将注意力集中于自身的位置（虽然这符合常识）而可能忽略了 其它位置。因此，作者采取的一种解决方案就是采用多头注意力机制 （MultiHeadAttention）。

#### softmax 函数？

#### 多头注意力机制？





### 位置编码

因为模型不包括递归和卷积，为了使得模型能够利用到序列的顺序必须加入位置编码信息。论文设计了一个与输入相同维度的位置编码，在编码器和解码器堆栈底部添加到输入中。位置编码包含很多种，有固定的、有可学习的。论文使用不同频率的正弦和预先函数作为位置编码：

![image-20220526095536230](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205260955385.png)

其中 $pos$ 是位置， $i$ 是维度。也就是说位置编码的每个维度对应一个正弦。

![image-20220526103153451](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205261031552.png)

$x$ 轴是输入 token ，上图设置输入token 序列长度为 70。$y$ 轴是 位置编码的值，取值范围为 $[-1, 1]$ 。左图每个 token 在相同维度上是相同的值，如果替换任意两个 token 位置编码的值不变。而右图对原本的位置编码加上 位置编码 函数，token 位置改变就会被模型捕捉到。

模型通过 $x/10000^{\frac{2*i}{d_{model}}}$ 控制函数波长，波长范围为 $[2\pi, 10000*2\pi]$ 。最短波长函数为 $sin(x/10000^{\frac{2*0}{d_{model}}})$ ，最长波长函数为 $sin(x/10000^{\frac{2*(\frac{d_{mode}}{2})}{d_{model}}})$ 。 $i$ 的取值范围为 $[0, d_{model}/2)$ 。 $pos$ 的取值范围为 $[0, max\_len)$ 。

#### 为什么 $i$ 的大小要限制为 $[0, d_{model}/2)$ ？





### 编码器结构

编码器是由 6 个相同模型堆叠组成，每个 编码器主要由两部分网络所构成：多 头注意力机制和两层前馈神经网络。对于这两部分网络来说都加入了残差连接，并且在残差连接后还进行 了层归一化操作。这样，对于每个部分来说其输出均为 LayerNorm(x+Sublayer(x))， 并且在都加入了 Dropout 操作。为了便于在这些地方使用残差连接，这两部分网络输出向量的维度 均为 $d_{model} = 512$ 。

#### 为什么使用 Relu 激活函数？

#### 为什么维度限制为 512 ？







### 解码器结构











### 解码阶段为什么还需要 位置编码









































## 参考资料

[^1]:[This post is all you need（上卷）—— 层层剥开 Transformer](https://zhuanlan.zhihu.com/p/420820453)



