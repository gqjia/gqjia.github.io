---
title: 【kaggle】【quora insincere questions classification】5 文本分类中样本倾斜
date: 2019-01-21 14:13:00
---

样本的偏斜问题，也叫数据集偏斜（unbalanced），它指的是参与分类的两个类别（也可以指多个类别）样本数量差异很大。


这个问题可以有多种方法去解决：

1. 对训练数据undersampling，即对多数类数据进行抽样，或者将少数类翻倍，使得两类数量相同，这种方法在效果上也还说得过去。但是这种方法会有一些问题：
    * 重采样改变了样本的分布，对于某些依赖于样本概率分布的算法来说这个方法行不通。
    * 对多数类采样会使信息减少，而对少数类翻倍并不会使得信息增多。
2. 使用Cost-sensitive learning方法，该方法是想试试看能否通过训练时候这种惩罚把预测“推”向另外一面。



---
目前看来第一种方法效果不佳。

平衡这个数据集的一个大问题是你最终丢弃了大量数据

除非能找到一种方法来继续使用希望得到的数据




基本上，当我们应用K-fold CV并对每个折叠进行平均时，我们给出的预测是每个折叠的K模型的集合。如果是一个集合模型，即你的LB得分，至少有两个因素影响表现。

1. 每个折叠的CV得分（每个基础分类器的性能）越高越好。
2. 每个折叠的分歧越高越好。

因此，您还应该检查验证框架中的第二个因素（折叠预测之间的相关性）。如果需要，可以在K-fold CV之前分离一些数据作为最终预测的验证数据（LB评分的估计）。

[A validation framework & impact of the random seed](https://www.kaggle.com/bminixhofer/a-validation-framework-impact-of-the-random-seed)

Takeaway

* The much higher scores on the leaderboard compared to CV scores are caused by a low correlation between folds of K-Fold CV.
* When tuning a model, you have to find the best tradeoff between CV score and correlation between folds.
* The seed is a valid hyperparameter to tune when not tuning it to the public LB.
* Because the seed has a huge influence on the score, the LB score of top public kernels is not a good indicator on how good the model architecture is.
* Although this is a kernels-only competition, local compute does matter a lot because you will likely not be able to achieve a good score on the leaderboard without tuning the seed.



---
尝试对损失函数进行加权。
