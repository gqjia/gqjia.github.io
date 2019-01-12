---
title: 【剑指Offer】052 把字符串转换成整数
date: 2019-01-11 10:18:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

将一个字符串转换成一个整数(实现Integer.valueOf(string)的功能，但是string不符合数字要求时返回0)，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0。

#### 输入描述:

输入一个字符串,包括数字字母符号,可以为空

#### 输出描述:

如果是合法的数值表达则返回该数字，否则返回0

#### 示例1
输入
>+2147483647
    1a33

输出
>2147483647
    0



```java
public class Solution {
    public int StrToInt(String str) {
        if(str.length() == 0) return 0;
        char[] a = str.toCharArray();
        boolean flag = true;
        int sum = 0;
        if(a[0] == '-') flag = false;
        else if(a[0] == '+') ;
        else sum = a[0] - 48;
        for(int i = 1; i < a.length; i++) {
            if(a[i] < 48 || a[i] > 57) return 0;
            sum = sum * 10 + a[i] - 48;
        }
        return flag?sum:sum*-1;
    }
}
```
