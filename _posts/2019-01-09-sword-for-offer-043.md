---
title: 【剑指Offer】043 数组中只出现一次的数字
date: 2019-01-09 10:15:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

一个整型数组里除了两个数字之外，其他的数字都出现了偶数次。请写程序找出这两个只出现一次的数字。


```java
//num1,num2分别为长度为1的数组。传出参数
//将num1[0],num2[0]设置为返回结果
public class Solution {
    public void FindNumsAppearOnce(int [] array, int num1[], int num2[]) {
      if(array.length < 2) return;
      int xor = 0;
      int flag = 1;
      for(int i=0; i<array.length; ++i)
          xor ^= array[i];
      while((xor & flag) == 0) flag <<= 1;
      for(int i=0; i<array.length; ++i) {
          if((flag & array[i]) == 0) num2[0] ^= array[i];
          else num1[0]^= array[i];
      }
    }
}
```
