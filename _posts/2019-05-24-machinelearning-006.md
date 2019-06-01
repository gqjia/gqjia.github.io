---
title: 【西瓜书】 006 支持向量机
date: 2019-05-24 13:42:00
---


训练数据集

$$D = {(x_1, y_1), (x_2, y_2), ..., (x_m, y_m)}, y_i\in{-1, +1}$$

划分超平面可以用线性方程来表示

$$w^T x + b = 0$$

其中$w = (w_1; w_2; ...; w_d)$为法向量，决定了超平面的方向；$b$为位移项，决定了超平面与原点之间的距离。记作超平面$(w, b)$。

样本空间中任意点$x$到超平面$(w, b)$的距离可以写为

$$r = \frac{|w^T x + b|}{\|w\|}$$


超平面$(w,b)$能够将训练样本正确分类，即对于$(x_i, y_i) \in D$
1. $w^T x + b >= +1 , y_i = +1$
2. $w^T x + b <= -1 , y_i = -1$

![SVM 6.3](/images/machine-learning/ml6.3.jpg)

两个异类支持向量(support vector)到超平面的距离之和为间隔(margin)

$$\gamma = \frac{2}{\|w\|}$$

最大间隔(maximum margin)的划分超平面

$$\max{w, b} \frac{1}{2} \|w\|^2    s.t. y_i (w^T x_i + b) >= 1, i = 1, 2, ..., m$$

![SVM 6.8](/images/machine-learning/ml6.8.jpg)

解出$\alpha$后求出$w$和$b$即可得到模型

$$f(x) = w^T x + b = \sum_{i = 1}^{m}{w_i^T x + b}$$

从对偶问题解出的$\alpha_i$是公式6.8的拉格朗日乘子,他恰对应着训练样本$(x_i, y_i)$

上述过程需满足KKT条件
