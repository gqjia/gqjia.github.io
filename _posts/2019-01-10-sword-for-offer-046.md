---
title: 【剑指Offer】046 左旋转字符串
date: 2019-01-10 14:40:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！


```java
public class Solution {
    public String LeftRotateString(String str,int n) {
        if(n>str.length()) return "";
        String s1 = str.substring(0, n);
        String s2 = str.substring(n,str.length());
        return s2 + s1;
    }
}
```