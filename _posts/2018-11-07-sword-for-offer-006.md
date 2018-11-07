---
title: 【剑指Offer】006 重建二叉树
date: 2018-11-07 09:11:31
---

![006](/images/alg-images/006.png)   

### 一般解法
遍历前序序列，第一个数作为根节点。  
再遍历中序序列，该数左侧为左子树，右侧为右子树。  
递归。  
```python
#!/user/bin/env python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

from collections import deque
class Solution:
    # 返回从尾部到头部的列表值序列，例如[1,2,3]
    def printListFromTailToHead(self, listNode):
        # write code here
        if not listNode:
            return []
        temp = deque()
        while listNode:
            temp.appendleft(listNode.val)
            listNode = listNode.next
        return temp
```
