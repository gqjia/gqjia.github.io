---
title: 不要停止预训练啊！
date: 2021-09-08 17:55:21
---

本来想总结一下自己对预训练模型的一些认识，但是我明显低估了这个任务的难度。
正好这段时间清华大学唐杰老师在微博上推了一个综述论文
[Pre-Trained Models: Past,Present and Future](http://keg.cs.tsinghua.edu.cn/jietang/publications/AIOPEN21-Han-et-al-Pre-Trained%20Models-%20Past,%20Present%20and%20Future.pdf) 。
就以这篇论文的笔记作为开始吧。

![Pre-Trained Models: Past,Present and Future](../images/posts/2021-09-08-pretrain-models-past-present-and-future/pratrain-models.png)
  
这是清华大学悟道团队出的一篇讲预训练的论文。
不得不说，这密密麻麻的作者和机构看起来就很唬人。

让我们跳过前面讲预训练发展历史的部分，直接看预训练语言模型。

# GPT 和 BERT

基于 Transformer 的预训练语言模型主要分为两个分支， GPT 和 BERT 。
前者使用自回归语言模型（autoregressive language modeling）作为预训练目标，
后者则使用了自编码语言模型（autoencoding language modeling）作为预训练目标。
一般而言， GPT 更擅长生成（NLG）任务， BERT 更擅长理解（NLU）任务。
  
![GPT 和 BERT 的不同](../images/posts/2021-09-08-pretrain-models-past-present-and-future/difference-between-gpt-and-bert.png)
  
GPT 的预训练也是跟自回归语言模型一致，根据先前的文本最大化当前词的条件概率。
在预训练之后通过微调（fine-tuning）适应下游任务。
输入文本通过 GPT 获取最后一层的表示（representation），将其经过额外的输出层得到下游任务目标。
  
![BERT 的预训练和微调](../images/posts/2021-09-08-pretrain-models-past-present-and-future/bert.png)
  
BERT 的预训练则采用了另一个思路，通过当前字两侧的文本预测当前字的概率。
为此 BERT 采用了一个预训练任务 MLM （masked language modeling）。
通过使用 \[MASK\] 随机遮蔽文本中的字（token），然后在预训练时对其进行预测。
除此之外， BERT 还采用了 NSP （next sentence prediction）预训练任务，
在预训练时预测两个句子是否构成上下句关系。
在微调阶段，输入如果存在两个句子则使用 \[SEP\] 连接，输入文本开始需要加上 \[CLS\] ，
BERT 会给每个输入 token 输出一个表示，这个表示可以用于序列标注任务和问答任务。
\[CLS\] token 的表示可以看作整个句子的表示，将其通过一个额外的分类层就可以用于分类任务。
  
在此之后，RoBERTa 和 ALBERT 都对 BERT 做了一定的改进，其中 RoBERTa 对 BERT 的改进主要包括4点：
1. 去掉了 NSP 预训练任务（RoBEARTa 指出 BERT 不需要 NSP 这个预训练任务，不过最新的研究 NSP 任务还是很重要的）；
2. 使用了更大的 batch size ，更多的训练数据和更多的训练步数；
3. 使用了更长的训练文本；
4. 动态修改 \[MASK\] ；
  
而 ALBERT 则从减少参数量的角度改进了BERT：
1. 将输入 embedding 矩阵拆分为两个较小的矩阵；
2. Transformer 各层之间共享参数；
3. 使用句子顺序预测预训练任务（SOP）代替 NSP ；
  
因为牺牲了空间效率， ALBERT 微调和推理的速度很慢。
  
对于预训练语言模型的改进主要集中在以下几个方面：
1. 改进模型结构或者设计新的预训练任务（XLNet、UniLM、MASS、SpanBERT、ELECTRA）；
2. 尝试多语言版本、结合知识图谱或者多模态的预训练语言模型；
3. 扩大预训练语言模型的尺寸（GPT、Switch Transformer）的同时提高计算效率；
  
后面详细讲一下各个方面的改进。
  
![PTMs](../images/posts/2021-09-08-pretrain-models-past-present-and-future/PTMs.png)
  
# 模型结构上的改进
  
预训练语言模型在结构上的改进主要有两个目的，
一是希望能建立一个统一的预训练语言模型（uniﬁed sequence modeling），
另一个是 cognitive-inspired architectures。
除此之外，有些改进主要用于提高自然语言理解的任务。
  
## 统一语言建模方向的改进
  
XLNet 使用了置换语言建模（permutated language modeling）统一了 GPT 风格的单项语言生成和 BERT 风格的双向语言理解。
论文指出 BERT 在预训练时使用 \[MASK\] 遮蔽的策略，而下游任务却没有出现 \[MASK\] 。
这使得预训练任务和下游任务存在矛盾。
为此， XLNet 在做预训练时对字的顺序进行重新排序，然后采用自回归的范式进行预训练，这使得 XLNet 同时具有了理解和生成的能力。
  
MPNet 采用了跟 XLNet 一样的置换语言建模，同时弥补了 XLNet 在预训练时不知道句子长度而下游任务知道句子长度这一矛盾。
  
UniLM 则是从多种预训练任务的角度建立一个统一的预训练语言模型。
UniLM 联合训练了单向（unidirectional）、双向（bidirectional）和序列到序列（sequence-to-sequence）三种预训练任务。
之所以能实现这一操作，是因为 UniLM 在注意力计算时采用了 MASK 的策略。
UniLM 在生成问答和生成式摘要上取得了很好的效果。
  
GLM 采用了一个更加优雅的方式统一自回归和自编码。
GLM 选择文本中的一个片段进行 MASK ，并不像 BERT 和 SpanBERT 一样告诉模型 \[MASK\] 代表了多少个字（token），
并采用了二维位置编码的策略保存 \[MASK\] 的长度信息。
GLM是第一个在自然语言理解、条件生成和无条件生成等所有类型任务中同时实现最佳性能的模型。
  
