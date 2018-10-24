---
title: 【转】算法工程师手册
date: 2018-10-23 19:11:31
---


## 数学基础
1. [线性代数基础](http://www.huaxiaozhuan.com/数学基础/chapters/1_algebra.html)
*  基本知识
*  向量操作
*  矩阵运算
2. 概率论基础
*  概率与分布
*  期望
*  大数定律及中心极限定理
*  不确定性来源
*  常见概率分布
*  先验分布与后验分布
*  测度论
*  信息论
3. 数值计算基础
*  数值稳定性
*  Conditioning
*  梯度下降法
*  海森矩阵
*  牛顿法
*  拟牛顿法
*  约束优化
4. 常用函数
*  sigmoid
*  softplus
*  Gamma 函数和贝塔函数

## 统计学习
1. 机器学习简介
*  基本概念
*  机器学习三要素
2. 线性代数基础
*  线性回归
*  广义线性模型
*  对数几率回归
*  线性判别分析
*  感知机
3. 支持向量机
*  线性可分支持向量机
*  线性支持向量机
*  非线性支持向量机
*  支持向量回归
*  SVDD
*  序列最小最优化方法
*  其它讨论
4. 朴素贝叶斯
*  贝叶斯定理
*  朴素贝叶斯法
*  半朴素贝叶斯分类器
*  其它讨论
5. 决策树
*  原理
*  特征选择
*  生成算法
*  剪枝算法
*  CART 树
*  连续值、缺失值处理
*  多变量决策树
6. knn
*  k 近邻算法
*  kd树
7. 集成学习
*  集成学习误差
*  Boosting
*  Bagging
*  集成策略
*  多样性分析
8. 梯度提升树
*  提升树
*  gboost
*  LightGBM
9. 特征工程
*  缺失值处理
*  特征编码
*  数据标准化、正则化
*  特征选择
*  稀疏表示和字典学习
*  多类分类问题
*  类别不平衡问题
10. 模型评估
*  泛化能力
*  过拟合、欠拟合
*  偏差方差分解
*  参数估计准则
*  泛化能力评估
*  训练集、验证集、测试集
*  性能度量
*  超参数调节
*  传统机器学习的挑战
11. 降维
*  维度灾难
*  主成分分析 PCA
*  核化线性降维 KPCA
*  流形学习
*  度量学习
*  概率PCA
*  独立成分分析
*  t-SNE
*  LargeVis
12. 聚类
*  性能度量
*  原型聚类
*  密度聚类
*  层次聚类
*  谱聚类
13. 半监督学习
*  生成式半监督学习方法
*  半监督 SVM
*  图半监督学习
*  基于分歧的方法
*  半监督聚类
*  总结
14. EM算法
*  示例
*  EM算法原理
*  EM算法与高斯混合模型
*  EM 算法与 kmeans 模型
*  EM 算法的推广
14. 最大熵算法
*  最大熵模型MEM
*  分类任务最大熵模型
*  最大熵的学习

## 深度学习
1. 深度学习简介
*  介绍
*  历史
2. 机器学习基础
*  基本概念
*  点估计、偏差方差
*  最大似然估计
*  贝叶斯估计
*  随机梯度下降
*  传统机器学习的挑战
*  低维流形
3. 深度前馈神经网络
*  基础
*  损失函数
*  输出单元
*  隐单元
*  结构设计
*  历史小记
4. 反向传播算法
*  反向传播
*  深度前馈神经网络  
*  实现
*  应用
*  自动微分
5. 正则化
*  基本概念
*  参数范数正则化
*  约束正则化
*  数据集增强
*  噪声鲁棒性
*  早停
*  参数共享
*  dropout
*  稀疏表达
*  半监督学习与多任务学习
*  对抗训练
*  正切传播算法
*  正则化和欠定问题
6. 最优化础
*  代价函数
*  神经网络最优化挑战
*  mini-batch
*  基本优化算法
*  自适应学习率算法
*  二阶近似方法
*  共轭梯度
*  优化策略和元算法
*  参数初始化策略
7. 卷积神经网络
*  卷积运算
*  卷积层、池化层
*  基本卷积的变体
*  算法细节
*  历史和现状
8. 循环神经网络
*  RNN计算图
*  循环神经网络
*  长期依赖
*  序列到序列架构
*  递归神经网络
*  回声状态网络
*  LSTM 和其他门控RNN
*  外显记忆
9. 工程实践指导原则
*  性能度量
*  默认的基准模型
*  决定是否收集更多数据
*  选择超参数
*  调试策略
*  示例：数字识别系统
*  数据预处理
*  变量初始化
*  结构设计

## 自然语言处理

### 主题模型
*  Unigram Model
*  pLSA Model
*  LDA Model
*  模型讨论

###词向量
*  向量空间模型 VSM
*  LSA
*  Word2Vec
*  GloVe

##计算机视觉

###图片分类网络
*  LeNet
*  AlexNet
*  VGG-Net
*  Inception
*  ResNet
*  SENet
*  DenseNet
*  小型网络
*  趋势

##工具

###CRF
*  CRF++
*    安装
*    使用
*    Python接口
*    常见错误

###lightgbm
*  lightgbm使用指南
*    安装
*    调参
*    进阶
*    API
*    Docker

###xgboost
*  xgboost使用指南
*    安装
*    调参
*    外存计算
*    GPU计算
*    单调约束
*    DART booster
*    Python API

### [scikit-learn](http://sklearn.apachecn.org/cn/0.19.0/index.html)
1. [预处理](http://www.huaxiaozhuan.com/工具/scikit-learn/chapters/1.preprocess.html)
*  特征处理
*  特征选择
*  字典学习
*  PipeLine
2. 降维
*  PCA
*  MDS
*  Isomap
*  LocallyLinearEmbedding
*  FA
*  FastICA
*  t-SNE
3. 监督学习模型
*  线性模型
*  支持向量机
*  贝叶斯模型
*  决策树
*  KNN
*  AdaBoost
*  梯度提升树
*  Random Forest
4. 模型评估
*  数据集切分
*  性能度量
*  验证曲线 && 学习曲线
*  超参数优化
5. 聚类模型
*  KMeans
*  DBSCAN
*  MeanShift
*  AgglomerativeClustering
*  BIRCH
*  GaussianMixture
*  SpectralClustering
6. 半监督学习模型
*  标签传播算法

## spark
1. 基础概念
*  核心概念
*  安装和使用
*  pyspark shell
*  独立应用
2. rdd使用
*  概述
*  创建 RDD
*  转换操作
*  行动操作
*  其他方法和属性
*  持久化
*  分区
*  混洗
3. dataframe使用
*  概述
*  SparkSession
*  DataFrame 创建
*  DataFrame 保存
*  DataFrame
*  Row
*  Column
*  GroupedData
*  functions
4. 累加器和广播变量
*  累加器
*  广播变量

## numpy
*  [numpy 使用指南](http://www.huaxiaozhuan.com/工具/numpy/chapters/numpy.html)
*    ndarray
*    ufunc 函数
*    函数库
*    数组的存储和加载

## scipy
*  scipy 使用指南
*    常数和特殊函数
*    拟合与优化
*    线性代数
*    统计
*    数值积分
*    稀疏矩阵

## matplotlib
*  matplotlib 使用指南
*    matplotlib配置
*    matplotlib Artist
*    基本概念
*    布局
*    Path
*    path effect
*    坐标变换
*    3D 绘图
*    技巧

## pandas
*  pandas 使用指南
*    基本数据结构  
*    内部数据结构
*    下标存取
*    运算
*    变换
*    数据清洗
*    字符串操作
*    聚合与分组
*    时间序列
*    DataFrame 绘图
*    移动窗口函数
*    数据加载和保存

## [keras](https://keras.io/zh/)

参考文章：《算法工程师手册(数学基础/统计学习/深度学习/自然语言处理/计算机视觉/工具)》by 华校专 <br/>
作者：[华校专]<br/>
链接：http://www.huaxiaozhuan.com/<br/>
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。<br/>
