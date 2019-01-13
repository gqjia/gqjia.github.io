---
title: 【剑指Offer】064 序列化二叉树
date: 2019-01-13 17:33:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

请实现两个函数，分别用来序列化和反序列化二叉树


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
    public int index = -1;

    String Serialize(TreeNode root) {
        StringBuffer sb = new StringBuffer();
        if(root == null){
            sb.append("#,");
            return sb.toString();
        }
        sb.append(root.val + ",");
        sb.append(Serialize(root.left));
        sb.append(Serialize(root.right));
        return sb.toString();
    }

    TreeNode Deserialize(String str) {
        index++;
        int len = str.length();
        if(index >= len) return null;
        String[] strr = str.split(",");
        TreeNode node = null;
        if(!strr[index].equals("#")){
            node = new TreeNode(Integer.valueOf(strr[index]));
            node.left = Deserialize(str);
            node.right = Deserialize(str);
        }
        return node;
  }
}
```
