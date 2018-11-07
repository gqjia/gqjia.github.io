---
title: 【剑指Offer】004 替换空格
date: 2018-11-06 19:39:31
---
![004](/images/alg-images/004.png)  

### 调用方法
调用replace方法。  
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
    # s 源字符串
    def replaceSpace(self, s):
        # write code here
        return s.replace(" ", "%20")
```  

### 最快的算法
将s转换成列表。  
遍历s，将空格替换成%20。  
利用join函数输出。  

```python
#!/user/bin/env python
# -*- coding:utf-8 -*-

class Solution:
    # s 源字符串
    def replaceSpace(self, s):
        # write code here
        s = list(s)
        count=len(s)
        for i in range(0,count):
            if s[i]==' ':
                s[i]='%20'
        return ''.join(s)
```
