---
title: 【剑指Offer】033 连续子数组的最大和
date: 2019-01-07 10:45:00
---

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Solution {
    public int FindGreatestSumOfSubArray(int[] array) {
        List<Integer> list = new ArrayList<>();
        for(int i=0;i<array.length;i++){
            int sum = 0;
            for(int j=i;j<array.length;j++) {
                sum += array[j];
                list.add(sum);
            }
        }
        if(list.size() <=0) return 0;
        Collections.sort(list);
        return list.get(list.size()-1);
    }
}
```
