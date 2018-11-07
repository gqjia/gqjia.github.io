---
title: 【剑指Offer】005 从尾到头打印链表
date: 2018-11-07 08:42:31
---

![005](/images/alg-images/005.png)    

### 最快速的解法  
```python
#!/user/bin/env python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # 返回从尾部到头部的列表值序列，例如[1,2,3]
    def printListFromTailToHead(self, listNode):
        # write code here
        l = []
        head = listNode
        while head:
            l.insert(0, head.val)
            head = head.next
        return l

```

### 第二快的解法  
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
