---
title: 【剑指Offer】029 二叉搜索树与双向链表
date: 2019-01-06 10:17:00
---

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
    public TreeNode Convert(TreeNode root) {
        if(root==null)
            return null;
        if(root.left==null && root.right==null)
            return root;

        TreeNode left = Convert(root.left);
        TreeNode p = left;
        while(p!=null && p.right!=null) {
            p = p.right;
        }
        if(left!=null) {
            p.right = root;
            root.left=p;
        }
        TreeNode right = Convert(root.right);
        if(right!=null) {
            root.right = right;
            right.left = root;
        }

        return  left!=null?left:root;
    }
}
```
