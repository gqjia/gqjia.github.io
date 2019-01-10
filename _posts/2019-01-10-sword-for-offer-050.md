---
title: 【剑指Offer】050 求1+2+3+...+n
date: 2019-01-10 16:02:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。


```java
public class Solution {
    public int Sum_Solution(int n) {
        int sum = (int) (Math.pow(n,2) + n);
        return sum>>1;
    }
}
```
