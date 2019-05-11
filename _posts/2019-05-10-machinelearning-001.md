---
title: 【西瓜书】 001 绪论&线性模型
date: 2019-05-10 19:57:00
---

### 第一章 绪论

### 第三章 线性模型

线性模型(linear model)试图通过学得一个通过属性的线性组合来进行预测的函数,即

$$f(x) = w_1 x_1 + w_2 x_2 + ... + w_d x_d +b$$

向量形式为

$$f(x) = W^T x + b$$

其中$W = (w_1; w_2; ...; w_d)$

#### 线性回归(Linear Regression)

给定数据集$D = {(x_1, y_1), (x_2, y_2), ..., (x_m, y_m)}$
，其中$x_i = (x_{i1}; x_{i2}; ...; x_{id})$,$y_i \in \mathbb{R}$.线性回归试图学的一个线性模型以尽可能准确地预测实值输出标记.

$f(x_i) = w x_i + b$,使得$f(x_i) \backsimeq y_i$

![3.7](/images/machine-learning/ml3.7.jpg)




#### 逻辑回归(Logistic Regression)

#### 线性判别(LDA)

#### 分类学习的拆分策略

#### 如何处理类别不平衡问题
