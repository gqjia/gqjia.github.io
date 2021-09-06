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
Example 1:
candidate 1: It is a guide to action which ensures that the military alwarys obeys the commands of the party.
candidate 2: It is to insure the troops forever hearing the activity fuidebook that party direct.
reference 1: It is a guide to action that ensures that the military will forever heed Party commands.
reference 2: It is the guiding principle which guarantees the military forces always being under the command of the Party.
reference 3: It is the practical guide for the army always to heed the directions of the party.

Example 2:
candidate: the the the the the the the.
reference 1: The cat is on the mat.
reference 2: There is a cat on the mat.
```

好的生成文本通常跟参考文本有很多相同的词（word）和短语（phrase）。
比如例子1中，备选句子1相比备选句子2而言包括更多相同的部分。
因此，可以对相同部分的程度进行量化，从而对生成文本进行评价。  
  
BLEU 的主要任务是比较候选文本（candidate）和参考文本(reference)的 n-gram 匹配次数。
匹配次数越多，说明候选翻译越好。
BLEU 计算的基础是计算精确度（precision），
用候选文本和参考文本中共同出现 unigram 的数量除以候选文本中 unigram 的数量。
  
但是这样也会产生一个问题，可能存在高精度但是实际效果很差的文本，如例子2所示。 
 BLEU 解决这个问题的方案（modified unigram precision）就是将每个参考文本中匹配过的词去掉，不进行下一次匹配。
计算时，首先统计每个 n-gram 在单个参考文本的最大出现次数。
然后在计算候选文本和参考文本 n-gram 匹配次数时，将最大值设置为之前统计的最大次数，即
$Count_{clip} = min(Count, Max_Ref_Count)$。
最后除以所有参考文本文本 n-gram 的总数量。
  
$$p_n = \frac{\sum_{C \in \{Candidates\}}\sum_{n-gram \in C} Count_{clip}(n\mbox{-}gram)}{\sum_{C' \in {\{Candidates\}}} \sum_{n\mbox{-}gram' \in C'}{Count(n\mbox{-}gram')} }$$
  
论文对两个效果差异很大的两个翻译系统在相同条件（127条源文本、每条4个参考翻译）下进行比较，统计平均得分。
如下图所示，modified n-gram precision 可以很好的区分翻译系统的优劣。
  
![Figure 1](../images/posts/2021-08-11-metrics-for-text-generation/Figure-1.png)
  
论文还对 n-gram 的取值进行了实验，在相同的实验条件（两个人工翻译、三个机器翻译系统）下，
依旧保持了不错的区分性，并且在 n-gram 的取值上都包含足够的信息。
为此论文选择对其进行加权平均，即兼顾了随着 n-gram 增加修正精确度区分度的衰减
（modified unigram precision 远大于 modified bigram precision，
modified bigram precision 又远大于 modified trigram precision），
又考虑到了各个取值上包含的信息。
  
![Figure 2](../images/posts/2021-08-11-metrics-for-text-generation/Figure-2.png)
  
虽然 modified n-gram precision 可以惩罚没有出现在参考文本中的 n-gram，
并且限制一个 n-gram 的统计次数（最大值限制为单个参考文本最大出现次数），
但是依然没有办法强制限制翻译文本的长度。
  
通常可以靠精确率（precision）和召回率（recall）相结合来实现对文本长度的限制。
但是 BLEU 考虑的是多条参考翻译，
每个参考翻译都有可能使用不同的词来对源文本中对应的词进行翻译。
因此，一个好的参考翻译只会贴近其中一个可能性，而不是全部。
在所有的参考翻译上进行召回率计算不能保证分数可以评价参考文本质量的好坏。
  
过长的文本会被 modified n-gram precision 惩罚，
因此没必要对过长的文本进行进一步处理。
因此需要引入一个简洁惩罚（brevity penalty）来防止产生过短的文本。
这种简洁惩罚和 modified n-gram precision 都没有直接考虑源文本长度，
而是考虑参考翻译文本长的范围。
当翻译的长度与任务参考翻译的长度相同的时候将简洁惩罚设置为1.0。
具体计算如下：
  
$$\mathrm{BP}=\left\{\begin{array}{ll}
1 & \text { if } c>r \\
e^{(1-r / c)} & \text { if } c \leq r
\end{array}\right$$
  
$$\mathrm{BLEU}=\mathrm{BP} \cdot \exp \left(\sum_{n=1}^{N} w_{n} \log p_{n}\right)$$
  
$$\log \mathrm{BLEU}=\min \left(1-\frac{r}{c}, 0\right)+\sum_{n=1}^{N} w_{n} \log p_{n}$$
  
通常情况下N取值为4，权重值设置为 $w_n = 1/N$


## ROUGE


