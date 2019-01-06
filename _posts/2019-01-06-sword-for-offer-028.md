---
title: 【剑指Offer】028 复杂链表的复制
date: 2019-01-06 09:52:00
---

```java
/*
public class RandomListNode {
    int label;
    RandomListNode next = null;
    RandomListNode random = null;

    RandomListNode(int label) {
        this.label = label;
    }
}
*/
public class Solution {
    public RandomListNode Clone(RandomListNode pHead) {
        RandomListNode p = pHead;
        RandomListNode t = pHead;
        while(p!=null) {
            RandomListNode q = new RandomListNode(p.label);
            q.next = p.next;
            p.next = q;
            p = q.next;
        }
        while(t!=null){
            RandomListNode q = t.next;
            if(t.random!=null)
                q.random = t.random.next;
            t = q.next;
        }

        RandomListNode s = new RandomListNode(0);
        RandomListNode s1 = s;
        while(pHead!=null){
            RandomListNode  q = pHead.next;
            pHead.next = q.next;
            q.next = s.next;
            s.next = q;
            s = s.next;
            pHead = pHead.next;    
       }
        return s1.next;
    }
}
```
