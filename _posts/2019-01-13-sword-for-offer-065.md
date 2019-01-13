---
title: 【剑指Offer】065 二叉搜索树的第k个结点
date: 2019-01-13 17:59:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）    中，按结点数值大小顺序第三小结点的值为4。


```java
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }

}
*/
public class Solution {
   int index = 0;
    TreeNode KthNode(TreeNode root, int k) {
        if(root != null){
            TreeNode node = KthNode(root.left, k);
            if(node != null) return node;
            index ++;
            if(index == k) return root;
            node = KthNode(root.right, k);
            if(node != null) return node;
        }
        return null;
    }
}
```
