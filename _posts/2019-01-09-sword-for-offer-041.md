---
title: 【剑指Offer】041 二叉树的深度
date: 2019-01-09 09:30:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

### 递归

```java
/**
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
    public int TreeDepth(TreeNode root) {
        if(root==null) return 0;
        int left = TreeDepth(root.left);
        int right = TreeDepth(root.right);

        return left>right?left+1:right+1;
    }
}
```

### 非递归

```java
import java.util.Queue;
import java.util.LinkedList;

public class Solution {
    public int TreeDepth(TreeNode pRoot) {
        if(pRoot == null) return 0;
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.add(pRoot);
        int depth = 0, count = 0, nextCount = 1;
        while(queue.size()!=0) {
            TreeNode top = queue.poll();
            count++;
            if(top.left != null) queue.add(top.left);
            if(top.right != null) queue.add(top.right);
            if(count == nextCount) {
                nextCount = queue.size();
                count = 0;
                depth++;
            }
        }
        return depth;
    }
}
```
