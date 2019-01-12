---
title: 【剑指Offer】058 链表中环的入口结点
date: 2019-01-12 15:08:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。


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

    public ListNode EntryNodeOfLoop(ListNode pHead) {
         if(pHead==null || pHead.next==null) return null;
         ListNode p1 = pHead;
         ListNode p2 = pHead;
        while(p2!=null && p2.next!=null) {
            p1 = p1.next;
            p2 = p2.next.next;
            if(p1==p2) {
                p1 = pHead;
                while(p1!=p2) {
                    p1 = p1.next;
                    p2 = p2.next;
                }
                if(p1==p2) return p1;
            }
        }
        return null;
    }
}
```
