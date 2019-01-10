---
title: 【剑指Offer】045 和为S的两个数字
date: 2019-01-10 14:22:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

#### 输出描述:

对应每个测试案例，输出两个数，小的先输出。


```java
import java.util.ArrayList;

public class Solution {
    public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {
        ArrayList<Integer> list = new ArrayList<Integer>();
        if (array == null || array.length < 2) return list;
        int i = 0, j = array.length - 1;
        while(i<j){
            if(array[i]+array[j]==sum) {
                list.add(array[i]);
                list.add(array[j]);
                return list;
            }else if(array[i]+array[j]>sum) j--;
            else i++;
        }
        return list;
    }
}
```
