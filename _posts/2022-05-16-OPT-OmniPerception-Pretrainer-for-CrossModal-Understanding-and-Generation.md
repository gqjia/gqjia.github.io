---
title: 另一个OPT
date: 2022-05-16 19:54:00
---









中科院的紫初太东模型。

如果不是工作需要，谁又会看这篇论文呢？

## 论文概述

OPT是一个包含三个模态的预训练语言模型。模型由一个 Seq2Seq 的框架构建，包含三个单模态编码器，用于生成每个模态的嵌入；一个跨模态编码器，用于捕获三个模态之间的关联性；两个跨模态解码器，用于生成文本和图像。预训练数据集采用 Open Images 数据集，预训练任务从三个不同粒度学习多个模态的对齐和转换。实验表明，OPT 不仅可以学得较强的多模态表征还可以在跨模态理解和生成任务上取得良好得结果。



## 模型结构

![image-20220517150712330](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205171507747.png)

OPT模型首先使用三种单模态编码器对文本、图片、语音进行编码。然后将三种表征使用 Transformer 进行跨模态交互。最后使用两种解码器**重建**输入文本和图像。

文本编码器按照 BERT 的思路，分词部分使用 WordPiece ，将 token embedding 和 position embedding 相加之后经过一层 LayerNorm 层得到文本的表征。

图像编码器使用在 Visual Genome 数据集上训练好的 Faster R-CNN 提取每个区域的表征。为了捕获空间位置信息，论文引入了一个7维特征 $[x_1, y_1, x_2, y_2, w, h, w * h]$ 。其中 $(x_1, y_1) $、$(x_2, y_2)$ 代表左上角和右下角坐标； $w/h$ 代表区域的宽度/高度。首先使用两个全连接层将图像特征和位置特征投影到相同的映射空间，再将两者相加通过LN层得到最终的表征。

语音编码器使用训练好的 wav2vec 2.0 模型获取语音的 token 以及每个 token 的表示。最后通过LN层得到音频的表征。

![image-20220517160434875](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205171604939.png)

在得到各个模态的表征后，将三者进行拼接送入到基于 Transformer 的跨模态编码器中。

![image-20220517160750652](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205171607721.png)

文本解码器采用 Transformer 的解码器。在图像解码器上，论文设计了一个两阶段的框架



