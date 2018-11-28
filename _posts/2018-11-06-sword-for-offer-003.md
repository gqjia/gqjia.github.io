---
title: 【剑指Offer】003 二维数组的查找
date: 2018-11-06 10:27:31
---
![003](/images/alg-images/003.jpg)  

### 暴力解法

两层循环来进行查找。  

```python
#!/user/bin/env python
# -*- coding:utf-8 -*-
'''
Created on 2018-11-06
Upgrate on 2018-11-06
Anthor: Moriarty12138
Github: https://github.com/Moriarty12138/leetcode-pratice
'''
class Solution:
    # array 二维列表
    def Find(self, target, array):
        # write code here
        for i in array:
            for j in i:
                if j == target:
                    return True
        return False
```

```java
public class S003 {
    public boolean Find(int target, int [][] array) {
        int row = 0, col = 0, t = 0;
        boolean isFind = false;
        for(int i = 0; i < array.length; i++)
            for(int j = 0; j < array[0].length; j++ )
                if(false == isFind && target == array[i][j])
                    isFind = true;
        return isFind;
    }
}
```


---

### 分治解法  
![003](/images/alg-images/003-1.jpg)  

从右上角侧开始查找  
如果这个数大于要查找的数，则开始查找这个数左侧的数，进入下一个循环。  
如果这个数小于要查找的数，则开始查找这个数下方的数，若找到则输出，若未找到则输出未找到该数。  
如果这个数等于要查找的数，输出结果。  

```python
#!/user/bin/env python
# -*- coding:utf-8 -*-
'''
Created on 2018-11-06
Upgrate on 2018-11-06
Anthor: Moriarty12138
Github: https://github.com/Moriarty12138/leetcode-pratice
'''
class Solution:
    # array 二维列表
    def Find(self, target, array):
        # write code here
        if array == [[]]:
            return False
        for i in array[::-1]:
            if i[0] == target:
                return True
            elif i[0] < target:
                for j in i:
                    if j == target:
                        return True
        return False
```

### 时间最快的算法

```python
#!/user/bin/env python
# -*- coding:utf-8 -*-
class Solution:
    # array 二维列表
    def Find(self, target, array):
        # write code here
        n=len(array)
        flag='false'
        for i in range(n):
            if target in array[i]:
                flag='true';
                break
        return flag
while True:
    try:
        S=Solution()
        # 字符串转为list
        L=list(eval(raw_input()))
        array=L[1]
        target=L[0]
        print(S.Find(target, array))
    except:
        break
```
