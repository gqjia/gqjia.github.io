---
title: 【剑指Offer】036 丑数
date: 2019-01-08 20:06:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。


```java
import java.util.ArrayList;

public class Solution {
    public int GetUglyNumber_Solution(int n) {
        if(n==0) return 0;
        ArrayList<Integer> res = new ArrayList<Integer>();
        res.add(1);
        int i2=0, i3=0, i5=0;
        while(res.size()<n) {
            int m2 = res.get(i2) * 2;
            int m3 = res.get(i3) * 3;
            int m5 = res.get(i5) * 5;
            int min = Math.min(m2, Math.min(m3, m5));
            res.add(min);
            if(min==m2) i2++;
            if(min==m3) i3++;
            if(min==m5) i5++;
        }

        return res.get(res.size() - 1);
    }
}
```
