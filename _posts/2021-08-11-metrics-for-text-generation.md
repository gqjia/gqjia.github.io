---
title: 写在GEM之前：文本生成的那些评价指标
date: 2021-08-11 10:56:01
---

上半年的时候就已经看到 [The GEM Benchmark](https://arxiv.org/abs/2102.01672) 这篇论文。当时就想着写点什么记录一下，但是一直找不到时间来写。总拖着也不好，现在就把之前零零散散记录的内容整理出来吧。

这篇先简单介绍一下文本生成的评价指标，下一篇再详细介绍 [The GEM Benchmark](https://arxiv.org/abs/2102.01672) 的做法。


衡量生成文本的质量是文本生成评价指标设计的初衷，但是对生成文本的质量进行评价是一个非常主观的过程。而且目前还没有一个完美的客观评价指标，这就导致文本生成的论文很多都需要使用人工评价来证明自己取得了很好的效果。  
文本生成的评价指标主要分为客观评价指标和人工评价。这里主要聊下客观评价指标。


## BLEU
[BLEU: a Method for Automatic Evaluation of Machine Translation](https://www.researchgate.net/publication/2588204_BLEU_a_Method_for_Automatic_Evaluation_of_Machine_Translation) 是2002年 IBM 提出的论文。而这篇论文设计的初衷是设计一种简单、快速、低成本的方法替代人工评价。  
BLEU最开始是用在机器翻译的任务中，主要是用来计算参考译文和候选译文中 n-gram 的重叠程度（n-gram precision）。这一设计的灵感来自于语音识别（ASR）任务的字错误率(word  error rate)，这样导致 BLEU 允许生成文本在单词选择和词序上存在一定的差异（论文的说法是合理性差异 legitimate difference）。  

以论文中的例子来说：
```txt
candidate 1: It is a guide to action which ensures that the military alwarys obeys the commands of the party.
candidate 2: It is to insure the troops forever hearing the activity fuidebook that party direct.

reference 1: It is a guide to action that ensures that the military will forever heed Party commands.
reference 2: It is the guiding principle which guarantees the military forces always being under the command of the Party.
reference 3: It is the practical guide for the army always to heed the directions of the party.
```
好的生成文本通常跟参考文本有很多相同的词（word）和短语（phrase），


## ROUGE
