---
title: 【西瓜书】 004 决策树
date: 2019-05-20 12:06:00
---

## 第四章 决策树

决策树是一种基本的分类与回归算法。

在分类问题中，表示基于特征对实例进行分类的过程。它可以认为是if-then规则的集合；也可以认为是定义在特征空间与类空间上的条件概率分布，将特征空间划分成了互不相交的单元。

决策树的优点是模型具有可读性，分类速度快。

决策树学习通常包括三个步骤：特征选择、决策树的生成和决策树的修剪。

决策树的算法通常是递归地选择最优特征，并根据该特征对训练数据进行分割，使得各个子数据集有一个最好的分类的过程。这一过程对应着特征空间的划分，即决策树的构建。但是决策树有可能发生过拟合现象，所以需要对已生成的树自下而上进行剪枝，使其有更好的泛化能力。

决策树的生成对应于模型的局部选择，决策树的剪枝对应于模型的全局选择。


#### 特征选择

1. 信息增益
//图片待补充

2. 信息增益比
//图片待补充

3. 平均最小化准则、基尼指数
//图片待补充

#### 决策树的生成

1. ID3算法
```txt
ID3 (Examples, Target_Attribute, Attributes)
    Create a root node for the tree
    If all examples are positive, Return the single-node tree Root, with label = +.
    If all examples are negative, Return the single-node tree Root, with label = -.
    If number of predicting attributes is empty, then Return the single node tree Root,
    with label = most common value of the target attribute in the examples.
    Otherwise Begin
        A ← The Attribute that best classifies examples.
        Decision Tree attribute for Root = A.
        For each possible value, vi, of A,
            Add a new tree branch below Root, corresponding to the test A = vi.
            Let Examples(vi) be the subset of examples that have the value vi for A
            If Examples(vi) is empty
                Then below this new branch add a leaf node with label = most common target value in the examples
            Else below this new branch add the subtree ID3 (Examples(vi), Target_Attribute, Attributes – {A})
    End
    Return Root
```
//图片待补充

2. C4.5算法
//图片待补充

3. CART生成算法
//图片待补充

#### 决策树的剪枝

1. 预剪枝
决策树生成过程中进行，会判断该结点的划分是否能带来决策树泛化性能的提升，如果不能，则该结点停止分裂。

2. 后剪枝
先生成一颗完整的决策树，然后自底向上剪枝。

3. CART剪枝
CART剪枝算法从“完全生长”的决策树的底端剪去一些子树，使决策树变小。


#### 如何处理连续值、缺失值

1. 连续值处理
最简单的策略是采用二分法对连续值进行处理。这正是C4.5决策树算法采用的机制。还有就是对连续数据进行离散化，分为几个值，然后当作离散值进行处理。

2. 缺失值处理
这里存在两个问题：

* 如何在属性值缺失的情况下进行划分属性选择
* 给定划分属性，若样本在该属性上的值缺失，如何对样本进行划分
对于第一个问题，若取值未知，则根据其他样本的取值来计算划分点。

对于第二个问题，若取值未知，则将该样本同时划入所有子结点，且设置一个样本权值用于计算loss。

#### 决策树的优点和缺点

1. 优点：

* 决策树算法中学习简单的决策规则建立决策树模型的过程非常容易理解，
* 决策树模型可以可视化，非常直观
* 应用范围广，可用于分类和回归，而且非常容易做多类别的分类
* 能够处理数值型和连续的样本特征

2. 缺点：

* 很容易在训练数据中生成复杂的树结构，造成过拟合（overfitting）。剪枝可以缓解过拟合的负作用，常用方法是限制树的高度、叶子节点中的最少样本数量。
* 学习一棵最优的决策树被认为是NP-Complete问题。实际中的决策树是基于启发式的贪心算法建立的，这种算法不能保证建立全局最优的决策树。Random Forest 引入随机能缓解这个问题

#### 各种决策树之间比较
1.   ID3由Ross Quinlan在1986年提出。ID3决策树可以有多个分支，但是不能处理特征值为连续的情况。决策树是一种贪心算法，每次选取的分割数据的特征都是当前的最佳选择，并不关心是否达到最优。在ID3中，每次根据“最大信息熵增益”选取当前最佳的特征来分割数据，并按照该特征的所有取值来切分，也就是说如果一个特征有4种取值，数据将被切分4份，一旦按某特征切分后，该特征在之后的算法执行中，将不再起作用，所以有观点认为这种切分方式过于迅速。ID3算法十分简单，核心是根据“最大信息熵增益”原则选择划分当前数据集的最好特征，信息熵是信息论里面的概念，是信息的度量方式，不确定度越大或者说越混乱，熵就越大。在建立决策树的过程中，根据特征属性划分数据，使得原本“混乱”的数据的熵(混乱度)减少，按照不同特征划分数据熵减少的程度会不一样。在ID3中选择熵减少程度最大的特征来划分数据（贪心），也就是“最大信息熵增益”原则。下面是计算公式，建议看链接计算信息上增益的实例。

2. C4.5是Ross Quinlan在1993年在ID3的基础上改进而提出的。.ID3采用的信息增益度量存在一个缺点，它一般会优先选择有较多属性值的Feature,因为属性值多的Feature会有相对较大的信息增益?(信息增益反映的给定一个条件以后不确定性减少的程度,必然是分得越细的数据集确定性更高,也就是条件熵越小,信息增益越大).为了避免这个不足C4.5中是用信息增益比率(gain ratio)来作为选择分支的准则。信息增益比率通过引入一个被称作分裂信息(Split information)的项来惩罚取值较多的Feature。除此之外，C4.5还弥补了ID3中不能处理特征属性值连续的问题。但是，对连续属性值需要扫描排序，会使C4.5性能下降。

3. CART（Classification and Regression tree）分类回归树由L.Breiman,J.Friedman,R.Olshen和C.Stone于1984年提出。ID3中根据属性值分割数据，之后该特征不会再起作用，这种快速切割的方式会影响算法的准确率。CART是一棵二叉树，采用二元切分法，每次把数据切成两份，分别进入左子树、右子树。而且每个非叶子节点都有两个孩子，所以CART的叶子节点比非叶子多1。相比ID3和C4.5，CART应用要多一些，既可以用于分类也可以用于回归。CART分类时，使用基尼指数（Gini）来选择最好的数据分割的特征，gini描述的是纯度，与信息熵的含义相似。CART中每一次迭代都会降低GINI系数。下图显示信息熵增益的一半，Gini指数，分类误差率三种评价指标非常接近。回归时使用均方差作为loss function。



#### 课后习题
//待补充
