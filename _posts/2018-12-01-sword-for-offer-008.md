---
title: 【剑指Offer】008 旋转数组的最小数字
date: 2018-12-01 14:30:31
---

```java
import java.util.ArrayList;
public class S008 {
    public int minNumberInRotateArray(int [] array) {
        if(array.length == 0) return 0;
        if(array.length == 1) return array[0];
        for (int i = 0; i < array.length-1; i++)
            if (array[i] > array[i+1])
                return array[i+1];
            else
                if (i == array.length-2) //查找至最后一个元素
                    return array[0];
        return 0;
    }
}

```
