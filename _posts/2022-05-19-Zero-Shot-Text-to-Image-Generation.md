---
title: DALL-E Zero-Shot Text-to-Image Genration
date: 2022-05-19 14:41:00

---







传统的文本到图像的生成主要通过在固定的数据集上进行训练，找到更好的建模假设。这些假设可能设计复杂 的体系结构、辅助损失和辅助信息（side information），例如目标的部分标签或者在训练时进行部分分段 MASK （segmentation mask）。论文设计了一个基于 Transformer 的简单方法，模型将图像和文本使用自回归（autoregressively）的方式建模为单流数据。在有了足够的规模和数据的情况下，模型在 zero shot 的情况下与以往的模型相当。





## Discrete VAE (dVAE)

dVAE 的编码器和解码器采用带有 bottleneck-style resblocks 的卷积 ResNets 。这个模型主要使用 1 × 1 卷积连接 3 × 3 卷积。编码器的第一个卷积是 7 × 7 ，最后一层卷积是 1 × 1 。编码器使用最大池对特征进行采样。解码器使用最近邻上采样（nearest-neighbor upsampleing）。















## 参考资料

[^1]:[DALL·E: Creating Images from Text](https://openai.com/blog/dall-e/)