---
title: 【剑指Offer】016 链表中倒数第k个结点
date: 2018-12-03 09:33:31
---

```java
public class S016 {
    public ListNode FindKthToTail(ListNode list,int k) {
        if (list == null)
            return list;
        ListNode node = list;
        int count = 0;
        while (node != null) {
            count++;
            node = node.next;
        }
        if (count < k)
            return null;
        ListNode p = list;
        for (int i = 0; i < count - k; i++)
            p = p.next;
        return p;
    }
}
```
