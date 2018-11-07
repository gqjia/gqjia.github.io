---
title: 【DL】1 循环序列模型
date: 2018-11-07 10:15:31
---

1.1  
序列数据(sequence data)  
speech recognition  
music generation  
sentiment classification  
DNA sequence analysis  
Machine translation  
video activity recognition  
name entity recognition  

使用标签(X, Y)作为训练集的监督学习。  

1.2  
one-hot  
语料库    

1.3  
循环神经网络(RNN)  
双向循环神经网络(BRNN)  
神经网络的前向传播  
![Forward Propagation](/images/DL-images/dl-sequeeze-1-1.png)  
![Simplified RNN notation](/images/DL-images/dl-sequeeze-1-2.png)  

1.4  
反向传播网络  
![Forward propagation and backpropagation](/images/DL-images/dl-sequeeze-1-3.png)  

1.5  
不同类型的RNN  
1.many to many  
2.many to one  
3.one to many 音乐生成、序列生成
4.many to many(x,y长度不同)  

1.6 语言模型和序列生成  
