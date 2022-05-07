---
title: VideoCLIP
date: 2022-05-05 14:04:00

---

![image-20220505140702073](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205051407184.png)

论文来自于Facebook 和 CMU 。



## 论文概述

论文作者将 CLIP 的思路延续到了视频领域。作者通过使用视频-文本对数据进行训练，设计了基于对比学习的训练目标，对 Transformer 结构的模型进行训练，得到了一个 zero-shot 的视频、文本理解模型。模型在下游任务（序列级视频-文本检索 sequence-level text-video retrieval、视频问答  VideoQA、token 级的动作定位 token-level action localization 和动作分隔 action segmentation ）都取得了 SOTA 。

## 论文背景

作者发现直接的训练目标会导致糟糕的训练结果，并且提出一个假设，视频和文本之间细粒度的关联对于 zero-shot 









## 论文结论

论文提出了一个视频-文本多模态预训练模型。模型采用了对比学习的训练目标，将时间上重叠的数据作为正例与临近的数据作为负例进行对比。（？）该方法在不进行监督训练时，在下游任务上的表现优于先前的无监督方法，并且在某些情况下，模型甚至优于有监督的方法。此外在下游任务进行微调可以带来进一步的效果提升。



