---
title: AE 、 VAE 和 dVAE
date: 2022-05-17 19:33:01

---

## AutoEncoder

自编码器（AE）是一个输入输出一样的神经网络结构。自编码器以无监督的方式进行训练，首先得到输入数据的低维表示，然后将这些表示投影到实际数据[^1]。

![img](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205181602989.png)

![img](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205171935484.jpeg)

自编码器可以理解为一个试图去还原其原始输入的系统。自编码器模型主要由编码器（Encoder）和解码器（Decoder）组成，其主要目的是将输入$x$转换成中间变量$y$，然后再将y转换成 $\hat{x}$，然后对比输入$x$和输出$\hat{x}$使得他们两个无限接近[^2]。

在深度学习中，自动编码器是一种无监督模型，它可以学习到输入数据的隐含特征，这称为编码 (coding)，同时用学习到的新特征可以重构出原始输入数据，称之为解码 (decoding)。从直观上来看，自动编码器可以用于特征降维，类似主成分分析 PCA，但是其相比 PCA 其性能更强，这是由于神经网络模型可以提取更有效的新特征。除了进行特征降维，自动编码器学习到的新特征可以送入有监督学习模型中，所以自动编码器可以起到特征提取器的作用。

神经网络自编码器三大特点：

1. 自编码器是数据相关的，这意味着自动编码器只能压缩哪些与训练数据类似的数据。
2. 自编码器是有损的，意思是解压缩的输出与原来的数据相比是退化的。
3. 自编码器是从数据样本中自动学习的。这就意味着很容易对指定类的输入训练出一种特定的编码器（不需要新的工作）。



构建一个自编码器需要三部分：编码器、解码器和损失函数。

```python
# 建模与训练
def getModel():
    input_layer = Input(shape=(x.shape[1],))
    encoded = Dense(8, activation='relu', activity_regularizer=regularizers.l2(10e-5))(input_layer)  # l2正则化约束
    decoded = Dense(x.shape[1], activation='relu')(encoded)
    autoencoder = Model(input_layer, decoded)
    autoencoder.compile(optimizer='adam', loss='mean_squared_error')
    return autoencoder
```

```python
# calculate_losses是一个辅助函数，计算每个数据样本的重建损失
def calculate_losses(x, preds):
    losses = np.zeros(len(x))
    for i in range(len(x)):
        losses[i] = ((preds[i] - x[i]) ** 2).mean(axis=None)
    return losses
```

自编码器和前馈神经网络的区别和联系如下：

1. 自编码器是前馈神经网络的一种，最开始主要用于数据的降维以及特征的抽取，随着技术的不断发展，现在也被用于生成模型中，可用来生成图片等。
2. 前馈神经网络是有监督学习，其需要大量的标注数据。自编码器是无监督学习（自对比），数据不需要标注因此较容易收集。
3. 前馈神经网络在训练时主要关注的是输出层的数据以及错误率，而自编码的应用可能更多的关注中间隐层的结果。



![image-20220518160438809](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205181604865.png)

整个自编码器结构（编码+解码器）为数据创造了一个“瓶颈”，确保是由主要信息通过“瓶颈”参与重建[^6]。

假设编码器和解码器是单层线性的结构。这样情况下的编码器和解码器可以表示为矩阵的简单线性变换。这时，就可以看出自编码器和 PCA 的关系。同 PCA 一样，自编码器实在讯在最佳的线性子空间来投影数据，同时尽可能的减少信息的损失[^6]。通过梯度下降可以得到 PCA 的编码和解码矩阵，但是这并不是唯一的解决方案。

![线性自编码器和 PCA 之间的关联](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205181617203.png)





**AE的变体：**

**堆栈自编码器**（SAE）：前面讲的自编码器只是简答的含有一层，其实可以采用更深层的架构，这就是堆栈自动编码器或者深度自动编码器，本质上就是增加中间特征层数。

在深度学习中，一般网络都有很多层，因为网络层数一多，训练网络采用的梯度下降，在低层网络会出现梯度弥散的现象，导致了深度网络一直不招人待见[^3]。直到 2006 年的 1 篇论文改变了这种状况[^4]。由 Hinton 提出了一种深层网络的训练方法，改变了人们对深度学习的态度。Hinton 所提出的训练思想，整体过程如下：

1. 网络各层参数预训练。在以前的神经网络中，参数的初始化都是用随机初始化方法，然而这种方法会导致深层网络中的参数很难被训练，于是 Hinton 提出了参数预训练，这个主要就是采用 RBM、以及自编码器对网络的每一层进行参数初始化。也就是说稀疏自编码就是为了对网络的每一层进行参数初始化，仅仅是为了获得初始的参数值而已（这就是所谓的无监督参数初始化，或者称之为 “无监督 pre-training”）。
2. 采用自编码器（或者 RBM ）把网络从第一层开始进行自编码训练，在每一层学习到的隐藏特征表示后作为下一层的输入，然后下一层再进行自编码训练，对每层网络的进行逐层无监督训练。
3. 当无监督训练完毕后，针对下游指定的任务（比如分类）可以用有标签的数据对整个网络的参数继续进行梯度下降调整。

这就是深层网络的训练思想，总体归结为：无监督预训练、有监督微调。

稀疏自编码仅仅只是为了获得参数的初始值而已。栈式自编码神经网络是一个由多层稀疏自编码器组成的神经网络，其前一层自编码器的输出作为其后一层自编码器的输入。栈式自编码就是利用上面所说的：无监督 pre-training、有监督微调进行训练训练的深度网络模型。

```python
def train(x_train):

    # input placeholder
    input_image = Input(shape=(ENCODING_DIM_INPUT, ))

    # encoding layer
    encode_layer1 = Dense(ENCODING_DIM_LAYER1, activation='relu')(input_image)
    encode_layer2 = Dense(ENCODING_DIM_LAYER2, activation='relu')(encode_layer1)
    encode_layer3 = Dense(ENCODING_DIM_LAYER3, activation='relu')(encode_layer2)
    encode_output = Dense(ENCODING_DIM_OUTPUT)(encode_layer3)

    # decoding layer
    decode_layer1 = Dense(ENCODING_DIM_LAYER3, activation='relu')(encode_output)
    decode_layer2 = Dense(ENCODING_DIM_LAYER2, activation='relu')(decode_layer1)
    decode_layer3 = Dense(ENCODING_DIM_LAYER1, activation='relu')(decode_layer2)
    decode_output = Dense(ENCODING_DIM_INPUT, activation='tanh')(decode_layer3)

    # build autoencoder, encoder
    autoencoder = Model(inputs=input_image, outputs=decode_output)

    # compile autoencoder
    autoencoder.compile(optimizer='adam', loss='mse')

    # training
    autoencoder.fit(x_train, x_train, epochs=EPOCHS, batch_size=BATCH_SIZE, shuffle=True)

    return autoencoder
```

**欠完备自编码器**从自编码器获得有用特征的一种方法是限制 h 的维度比 x 小，这种编码维度小于输入维度的自编码器称为欠完备（undercomplete）自编码器。学习欠完备的表示将强制自编码器捕捉训练数据中最显著的特征。

**正则自编码器**使用的损失函数可以鼓励模型学习其他特性（除了将输入复制到输出），而不必限制使用浅层的编码器和解码器以及小的编码维数来限制模型的容量。这些特性包括稀疏表示、表示的小导数、以及对噪声或输入缺失的鲁棒性。即使模型容量大到足以学习一个无意义的恒等函数，非线性且过完备的正则自编码器仍然能够从数据中学到一些关于数据分布的有用信息。

**去噪自编码器**（denoising autoencoder, DAE）是一类接受损坏数据作为输入，并训练来预测原始未被损坏数据作为输出的自编码器。





![image-20220519095408740](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205190954854.png)

上面的人脸图片可以描述成多种不同属性，比如微笑、皮肤黑、男性、有胡须、不戴眼镜和头发黑等等。如果想要通过神经网络架构重构出这张人脸图片，可以先将图片编码成一个一个不同的属性，然后通过综合这些属性解码得到重构图片，这就是自编码器的常规思路[^8]。

![image-20220519095439092](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205190954144.png)

然而这里还有一个需要解决的问题是，如何对一张图片的属性进行编码，因为属性既可以描述成离散值的形式也可以描述成概率分布的形式(从概率分布中随机采样出离散值)。比如第一张小男孩图片的smile属性，离散值可以表示为-0.8，概率分布可以表示成-1到0之间的正态分布(然后从中随机采样出离散值，大概率在正态分布最高点的横坐标-0.5附近)[^8]。



## VAE[^5]

![image-20220519101015759](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205191010826.png)

最基础的AutoEncoder由一个Encoder和一个Decoder组成。Encoder对输入图片进行编码得到latent code，然后通过Decoder进行重建，就算输入图片和生成图片的重构误差，训练的时候最小化重构误差。

AutoEncoder可以理解为通过网络学习出任意概率分布，然后取概率分布中的最高点的横坐标作为编码的离散值。这会导致AutoEncoder的生成过程是不可控的，对输入噪声敏感，因为学习得到的概率分布是不能提前预知的。

![image-20220519101056482](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205191010529.png)

VAE通过Encoder学习出mean和std两个编码，同时随机采样一个正态分布的编码 $\varepsilon$ ，然后通过$\varepsilon   * std + mean$公式重采样得到 latent code，然后通过 Decoder 进行重建。

另外，由于正态分布的连续性，不存在不可导问题，可以通过重参数方法，对 latent code 进行恢复，并且通过链式求导法则进行梯度更新。

![image-20220519101805730](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205191018800.png)

VAE可以理解为通过网络学习出每个属性正态分布的 mean 和 std 编码，然后通过 mean 和 std 和 N ( 0,1 ) 正态分布恢复每个属性的正态分布，最后随机采样得到每个属性的离散值。

![image-20220519101924527](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205191019578.png)

如图所示，VAE 相对于 AutoEncoder 的好处是，当采样输入不同时，VAE 对于任意采样都能重构出鲁棒的图片。VAE 的生成过程是可控的，对输入噪声不敏感，我们可以预先知道每个属性都是服从正态分布的。

VAE 就是在 AntoEncoder 的基础上给 lantent vector 添加限制条件，让其服从高斯分布，这样我们通过训练得到的 decoder 就可以直接使用，将随机生成的一个高斯分布喂给 decoder 就能生成图片。

![image-20220519102317159](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205191023213.png)

VQVAE 通过 Encoder 学习出中间编码，然后通过最邻近搜索将中间编码映射为 codebook 中 K 个向量之一，然后通过 Decoder 对 latent code 进行重建。

另外由于最邻近搜索使用 argmax 来找 codebook 中的索引位置，导致不可导问题， VQVAE 通过 stop gradient 操作来避免最邻近搜索的不可导问题，也就是 latent code 的梯度跳过最近邻搜索直接复制到中间编码上。

VQVAE 相比于 VAE 最大的不同是，直接找每个属性的离散值，通过类似于查表的方式，计算 codebook 和中间编码的最近邻作为 latent code 。由于维护了一个 codebook ，编码范围更加可控， VQVAE 相对于 VAE ，可以生成更大更高清的图片(这也为后续 DALLE 和 VQGAN 的出现做了铺垫)。

因此AutoEncoder、VAE和VQVAE可以统一为latent code的概率分布设计不一样，AutoEncoder通过网络学习得到任意概率分布，VAE设计为正态分布，VQVAE设计为codebook的离散分布。**总之，AutoEncoder的重构思想就是用低纬度的latent code分布来表达高纬度的数据分布，VAE和VQVAE的重构思想是通过设计latent code的分布形式，进而控制图片生成的过程。**



## dVAE

相比普通的 VAE ，dVAE 有两点区别：

1. 和 VQVAE 方法相似，dVAE 的 encoder 是将图像的 path 映射到 8192 出的词表中，论文将其分布设为在词表上的均匀分布，这是一个离散分布，由于不可导的问题









## 参考资料

[^1]: https://www.kaggle.com/code/shivamb/how-autoencoders-work-intro-and-usecases/notebook
[^2]:[【全】一文带你了解自编码器（AutoEncoder）](https://zhuanlan.zhihu.com/p/80377698)
[^3]:[【深度学习】 自编码器（AutoEncoder）](https://zhuanlan.zhihu.com/p/133207206)
[^4]:http://www.cs.toronto.edu/~fritz/absps/ncfast.pdf
[^5]:[Auto-Encoding Variational Bayes](https://arxiv.org/pdf/1312.6114.pdf)
[^6]:[Understanding Variational Autoencoders (VAEs)](https://towardsdatascience.com/understanding-variational-autoencoders-vaes-f70510919f73)
[^7]:[Understanding Generative Adversarial Networks (GANs)](https://towardsdatascience.com/understanding-generative-adversarial-networks-gans-cd6e4651a29)
[^8]:[漫谈VAE和VQVAE，从连续分布到离散分布](https://mp.weixin.qq.com/s?__biz=Mzg4MjQ1NzI0NA==&mid=2247495721&idx=1&sn=8242be9e662f96896c13c36067fa8ab0&chksm=cf54dfdaf82356cc41bd2bb4924937a46de335e7da56e4e3128ee01f1b6994b0018edc762982&scene=21#wechat_redirect)