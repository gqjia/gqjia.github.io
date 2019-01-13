---
title: 【剑指Offer】063 把二叉树打印成多行
date: 2019-01-13 17:18:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

### 递归

```java
import java.util.ArrayList;


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
    ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        ArrayList<ArrayList<Integer>> list = new ArrayList<>();
        depth(pRoot, 1, list);
        return list;
    }

    private void depth(TreeNode root, int depth, ArrayList<ArrayList<Integer>> list) {
        if(root == null) return;
        if(depth > list.size())
            list.add(new ArrayList<Integer>());
        list.get(depth -1).add(root.val);

        depth(root.left, depth + 1, list);
        depth(root.right, depth + 1, list);
    }
}
```

### 非递归

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;

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
    ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        if(pRoot == null) return result;
        Queue<TreeNode> layer = new LinkedList<TreeNode>();
        ArrayList<Integer> layerList = new ArrayList<Integer>();
        layer.add(pRoot);
        int start = 0, end = 1;
        while(!layer.isEmpty()){
            TreeNode cur = layer.remove();
            layerList.add(cur.val);
            start++;
            if(cur.left!=null) layer.add(cur.left);           
            if(cur.right!=null) layer.add(cur.right);
            if(start == end){
                end = layer.size();
                start = 0;
                result.add(layerList);
                layerList = new ArrayList<Integer>();
            }
        }
        return result;
    }
}
```
