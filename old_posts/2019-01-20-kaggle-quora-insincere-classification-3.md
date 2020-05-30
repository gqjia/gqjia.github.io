---
title: 【kaggle】【quora insincere questions classification】3 StratifiedKFold
date: 2019-01-20 17:24:00
---


### sklearn.model_selection.StratifiedKFold

class sklearn.model_selection.StratifiedKFold(n_splits=’warn’, shuffle=False, random_state=None)[source]

#### Parameters:

##### n_splits : int, default=3

Number of folds. Must be at least 2.

Changed in version 0.20: n_splits default value will change from 3 to 5 in v0.22.

##### shuffle : boolean, optional

Whether to shuffle each stratification of the data before splitting into batches.

##### random_state : int, RandomState instance or None, optional, default=None

If int, random_state is the seed used by the random number generator; If RandomState instance, random_state is the random number generator; If None, the random number generator is the RandomState instance used by np.random. Used when shuffle == True.


#### Examples

```python
>>> from sklearn.model_selection import StratifiedKFold
>>> X = np.array([[1, 2], [3, 4], [1, 2], [3, 4]])
>>> y = np.array([0, 0, 1, 1])
>>> skf = StratifiedKFold(n_splits=2)
>>> skf.get_n_splits(X, y)
>>> print(skf)  
StratifiedKFold(n_splits=2, random_state=None, shuffle=False)
>>> for train_index, test_index in skf.split(X, y):
...    print("TRAIN:", train_index, "TEST:", test_index)
...    X_train, X_test = X[train_index], X[test_index]
...    y_train, y_test = y[train_index], y[test_index]
TRAIN: [1 3] TEST: [0 2]
TRAIN: [0 2] TEST: [1 3]
```

#### StratifiedKFold和Kfold的区别

tratifiedKFold用法类似Kfold，但是他是分层采样，确保训练集，测试集中各类别样本的比例与原始数据集中相同。

### 参考资料
1. [sklearn.model_selection.StratifiedKFold](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.StratifiedKFold.html)
2. [StratifiedKFold和Kfold的区别](https://blog.csdn.net/zhangbaoanhadoop/article/details/79559011)
