---
title: 【剑指Offer】059 删除链表中重复的结点
date: 2019-01-12 15:49:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5



```java
/*
 public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public ListNode deleteDuplication(ListNode pHead) {
        if (pHead == null || pHead.next == null) return pHead;
        if (pHead.val == pHead.next.val) {
            ListNode pNode = pHead.next;
            while (pNode != null && pNode.val == pHead.val) {
                pNode = pNode.next;
            }
            return deleteDuplication(pNode);
        } else {
            pHead.next = deleteDuplication(pHead.next);
            return pHead;
        }
    }
}
```
