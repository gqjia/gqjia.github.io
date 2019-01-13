---
title: 【剑指Offer】067 滑动窗口的最大值
date: 2019-01-13 18:30:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。


```java
import java.util.ArrayList;
import java.util.ArrayDeque;

/**
用一个双端队列，队列第一个位置保存当前窗口的最大值，当窗口滑动一次
1.判断当前最大值是否过期
2.新增加的值从队尾开始比较，把所有比他小的值丢掉
*/
public class Solution {
   public ArrayList<Integer> maxInWindows(int [] num, int size) {
        ArrayList<Integer> res = new ArrayList<>();
        if(size == 0) return res;
        int begin;
        ArrayDeque<Integer> q = new ArrayDeque<>();
        for(int i = 0; i < num.length; i++){
            begin = i - size + 1;
            if(q.isEmpty())
                q.add(i);
            else if(begin > q.peekFirst())
                q.pollFirst();

            while((!q.isEmpty()) && num[q.peekLast()] <= num[i])
                q.pollLast();
            q.add(i);  
            if(begin >= 0) res.add(num[q.peekFirst()]);
        }
        return res;
    }
}
```
