---
title: 【剑指Offer】031 数组中出现次数超过一半的数字
date: 2019-01-07 10:13:00
---

```java
import java.util.Arrays;
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        int len=array.length;
        if(len<1)
            return 0;
        int count=0;
        Arrays.sort(array);
        int num=array[len/2];
        for(int i=0;i<len;i++)
            if(num==array[i])
                count++;
        if(count<=(len/2))
            num=0;
        return num;
    }
}
```
