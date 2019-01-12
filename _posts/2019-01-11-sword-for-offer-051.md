---
title: 【剑指Offer】051 不用加减乘除做加法
date: 2019-01-11 10:02:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

写一个函数，求两个整数之和，要求在函数体内不得使用+、-、\*、/四则运算符号。



```java
public class Solution {
    public int Add(int num1,int num2) {
      while (num2!=0) {
          int temp = num1^num2;
          num2 = (num1&num2)<<1;
          num1 = temp;
      }
      return num1;
    }
}
```
