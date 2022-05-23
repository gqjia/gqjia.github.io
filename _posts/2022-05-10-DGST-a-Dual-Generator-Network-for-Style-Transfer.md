---
title: DGST a Dual-Generator Network for Text Style Transfer [EMNLP 2020]
date: 2022-05-10 09:54:00

---



论文提出了一个 DGST 神经网络。该模型抛弃了鉴别器和平行语料，只使用两个生成器就可以完成文本风格迁移的任务。论文设计了一种句子去噪的方法，称为领域采样（neighbourhood sampling）。先给每个句子引入噪声，再使用模型完成去噪操作。

#### 模型结构

![](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205101556784.png)

以非平行语料X和Y为例，这两条数据的风格为$S_x$和$S_y$。模型的目的是将其中一种风格转换为另一种风格，同时保留与风格无关的语境。

![image-20220511110807186](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205111108364.png)

$D(x\|y)$是根据最小编辑距离测量句子之间抽象距离的函数，其中编辑操作包括词级替换、插入和删除（汉明距离）。等式1要求迁移的文本应该在目标风格空间内，等式2要求迁移的文本不能更改太多，保留与风格无关的信息。

作者受到CycleGan的启发，同样采用循环的形式进行训练，设计两个转换器：一个转换器将一个风格的数据转换到另一个风格的数据，另一个转换器将另一个风格的数据转换回源风格的数据。训练时设计了两个训练目标，一个确保生成文本的信息尽可能被保存；另一个将输入文本风格转换为目标风格。

##### **CycleGan模型**

CycleGAN主要用于域迁移（Domain Adaption）领域，如图片风格迁移（image style transfer）。

![img](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205131622887.jpeg)

像这样，CycleGAN能将真实照片转换成不同风格，但转换后的图片又能保留原始照片中的各种内容。在此之前如pix2pix等其他网络能够实现Domain Adaption，但需要的训练数据必须是成对的，即在训练时，将一张样本照片输入给模型，就必须输入其对应风格的图片，这对训练数据的要求是较高的，因为往往现实中不存在这么多成对的数据，而CycleGan可以做到“unpaired image-to-image translation”，具体来说，我们有一堆照片（但之前并没有这些照片的梵高风格表示），一堆梵高的画作，我们同时喂给机器，机器就能将照片转换成梵高的风格。关于成对数据和不成对数据，原论文给出下图。

![img](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205131624462.jpeg)









为了平衡模型保存内容的能力和风格迁移的能力，模型首先按照DAE（denoising autoencoders）的方式，这样的方式有助于保存除风格外的内容。模型将带噪声的句子进行还原。

**DAE**



损失函数设计这部分没看懂，暂时跳过，之后补充。

![image-20220512195506215](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205121955601.png)

![image-20220512195550338](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205121955381.png)

![image-20220512195603838](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205121956887.png)

![image-20220512195617287](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205121956322.png)



#### 模型实验

模型在Yelp数据集和IMDb数据集上验证效果。作者从迁移强度（Transfer Intensity）和保存程度（Content Preservation）两个方面对模型进行评估。作者用fasttext训练了一个分类器来评估迁移强度；用BLEU分数来评估保存内容的程度。

![image-20220513104456220](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205131044369.png)

在不借助对抗训练、强化学习、外部离线情感分类器的情况下，DGST在Yelp数据集上超越了除StyleTransformer以外的基线模型。而且相比StyleTransformer来说，DGST使用的是BiLSTM。在IMDb数据集上，DGST的迁移能力处于中等水平，而BLEU评分只低于StyleTransformer。模型实验的样例如下：

![image-20220513105552430](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205131055492.png)

作者设计了消融实验，分别为（1）no-res 去除了重建的训练目标；（2）rec-no-noise 在重建时不加入噪声；（3）no-tran 去除了迁移的训练目标；（4）tran-no-noise 在迁移时不加入噪声。

![image-20220513111603569](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205131116613.png)

根据实验，取消重建和迁移的训练目标会分别降低保存能力和迁移能力。在重建的训练目标使用噪声可以平衡模型保存和迁移的能力；对于迁移的训练目标，不使用噪声或者在错误的位置放置噪声将会降低模型的迁移能力。







## 参考资料

[^1]:[文本风格迁移研究综述](http://www.jos.org.cn/jos/article/abstract/6544)
[^2]:[**DGST: a Dual-Generator Network for Text Style Transfer**](https://readpaper.com/paper/3097466938)
[^3]:[**Semi-supervised Text Style Transfer: Cross Projection in Latent Space.**](https://readpaper.com/paper/2971232986)
[^4]:[CycleGAN简介](https://zhuanlan.zhihu.com/p/507840466)
[^5]:[**Formality Style Transfer with Shared Latent Space**](https://readpaper.com/paper/3115113481)
[^6]:[**Automatically Neutralizing Subjective Bias in Text**](https://readpaper.com/paper/2990530823)
