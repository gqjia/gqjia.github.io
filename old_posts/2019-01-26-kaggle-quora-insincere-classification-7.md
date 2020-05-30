---
title: 【kaggle】【quora insincere questions classification】7 特征工程
date: 2019-01-26 14:09:00
---

#### StandardScalers数据预处理

我们知道，在训练模型的时候，要输入features,即因子，也叫特征。对于同一个特征，不同的样本中的取值可能会相差非常大，一些异常小或异常大的数据会误导模型的正确训练；另外，如果数据的分布很分散也会影响训练结果。以上两种方式都体现在方差会非常大。此时，我们可以将特征中的值进行标准差标准化，即转换为均值为0，方差为1的正态分布。所以在训练模型之前，一定要对特征的数据分布进行探索，并考虑是否有必要将数据进行标准化。
　　
##### 标准化的方式一

使用preprocessing.scale()

```python
from sklearn import preprocessing   
import numpy as np    
X = np.array([[1., -1., 2.], [2., 0., 0.], [0., 1., -1.]])    
X_scaled = preprocessing.scale(X)   
#output :X_scaled = [[ 0.         -1.22474487  1.33630621]  
                 [ 1.22474487  0.         -0.26726124]  
                 [-1.22474487  1.22474487 -1.06904497]]  
＃scaled之后的数据零均值，单位方差  
X_scaled.mean(axis=0)  # column mean: array([ 0.,  0.,  0.])    
X_scaled.std(axis=0)  #column standard deviation: array([ 1.,  1.,  1.])  
```


##### 标准化的方式二

使用StandardScaler，fit()，transform()；或者fit_transform()


```python
from sklearn.preprocessing import StandardScaler
import numpy as np

X = np.array([[1., -1., 2.],
              [2., 0., 0.],
              [0., 1., -1.]])
ss = StandardScaler()
ss2 = StandardScaler()
print(X)
scaler = ss.fit(X) # <class 'sklearn.preprocessing.data.StandardScaler'>
# print(ss is scaler) # True
print(scaler)
print(scaler.mean_)

transform = scaler.transform(X)
print(transform)
# ss_transform = ss.transform(X) # 同上，完全一样
# print(ss_transform)

# fit_transform = ss.fit_transform(X) # 重新学习了一遍，当然结果是一样的
# print(fit_transform)

# ss2_transform = ss2.transform(X) # 没有通过fit得到元数据的均值和方差，无法进行0-1标准化。直接使用是错误的
# print(ss2_transform)
```
