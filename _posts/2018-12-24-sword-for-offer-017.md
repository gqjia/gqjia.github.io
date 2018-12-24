---
title: 【剑指Offer】017 反转链表
date: 2018-12-24 09:25:31
---

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode ReverseList(ListNode head) {
        if(head==null) return null;
        ListNode newHead = null;
        ListNode pNode = head;
        ListNode pPrev = null;
        while(pNode!=null){
            ListNode pNext = pNode.next;
            if(pNext==null)
                newHead = pNode;
            pNode.next = pPrev;
            pPrev = pNode;
            pNode = pNext;
        }
        return newHead;
    }
}
```
