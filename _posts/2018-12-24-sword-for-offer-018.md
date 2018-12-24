---
title: 【剑指Offer】018 合并两个排序的链表
date: 2018-12-24 09:35:31
---

### 非递归
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
    public ListNode Merge(ListNode list1,ListNode list2) {
        if(list1==null) return list2;
        if(list2==null) return list1;
        ListNode mergeList = null;
        ListNode mergeHead = null;
        while(list1!=null && list2!=null){
            if(list1.val<=list2.val){
                if(mergeHead==null) {
                    mergeList = list1;
                    mergeHead = list1;
                }
                else{
                    mergeList.next = list1;
                    mergeList = mergeList.next;
                }
                list1 = list1.next;
            }else{
                if(mergeHead==null) {
                    mergeList = list2;
                    mergeHead = list2;
                }
                else{
                    mergeList.next = list2;
                    mergeList = mergeList.next;
                }
                list2 = list2.next;
            }
        }
        if(list1==null) mergeList.next=list2;
        if(list2==null) mergeList.next=list1;
        return mergeHead;
    }
}
```
### 递归

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
    public ListNode Merge(ListNode list1,ListNode list2) {
        if(list1 == null) return list2;
        if(list2 == null) return list1;
        if(list1.val <= list2.val){
            list1.next = Merge(list1.next, list2);
            return list1;
        }else{
           list2.next = Merge(list1, list2.next);
           return list2;
        }   
    }
}
```
