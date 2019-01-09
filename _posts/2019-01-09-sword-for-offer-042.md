---
title: 【剑指Offer】042 平衡二叉树
date: 2019-01-09 09:50:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

输入一棵二叉树，判断该二叉树是否是平衡二叉树。

### 遍历

```java
public class Solution {
    public boolean IsBalanced_Solution(TreeNode root) {
        if(root == null) return true;
        return Math.abs(maxDepth(root.left) - maxDepth(root.right)) <= 1 &&
                        IsBalanced_Solution(root.left) &&
                        IsBalanced_Solution(root.right);
    }

    private int maxDepth(TreeNode root) {
        if(root == null) return 0;
        return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
    }
}
```

### 从下往上遍历

```java
public class Solution {
    public boolean IsBalanced_Solution(TreeNode root) {
        return getDepth(root) != -1;
    }
    
    private int getDepth(TreeNode root) {
        if (root == null) return 0;
        int left = getDepth(root.left);
        if (left == -1) return -1;
        int right = getDepth(root.right);
        if (right == -1) return -1;
        return Math.abs(left - right) > 1 ? -1 : 1 + Math.max(left, right);
    }
}
```
