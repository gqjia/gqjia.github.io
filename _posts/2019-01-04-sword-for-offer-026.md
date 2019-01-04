---
title: 【剑指Offer】026 二叉搜索树的后序遍历序列
date: 2019-01-04 13:13:00
---

```java
public class Solution {
    public boolean isTreeBST(int [] sequence, int start, int end) {
        if(end <= start) return true;
        int i = start;
        for(; i < end; i++) {
            if(sequence[i] > sequence[end]) break;
        }
        for(int j = i; j < end; j++) {
            if(sequence[j] < sequence[end]) return false;
        }
        return isTreeBST(sequence, start, i-1) && isTreeBST(sequence, i, end-1);
    }
    public boolean VerifySquenceOfBST(int [] sequence) {
        if(sequence.length == 0) return false;
        return isTreeBST(sequence, 0, sequence.length-1);
    }
}
```
