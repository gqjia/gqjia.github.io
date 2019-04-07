---
title: 【剑指Offer】015 调整数组顺序使奇数位于偶数前面
date: 2018-12-03 09:32:31
---

```java
public class S015 {
    public void reOrderArray(int [] array) {
        for(int i= 0;i<array.length-1;i++)
            for(int j=0;j<array.length-1-i;j++)
                if(array[j]%2==0 && array[j+1]%2==1){
                    int t = array[j];
                    array[j]=array[j+1];
                    array[j+1]=t;
                }
    }
}
```


```Java
public class Solution {
    public void reOrderArray(int [] array) {
        for (int i = 0; i < array.length; i++) {
            if (array[i] % 2 == 1) {
                int j = i;
                while(j > 0) {
                    if (array[j - 1] % 2 == 0) {
                        int temp = array[j];
                        array[j] = array[j - 1];
                        array[j - 1] = temp;
                        j--;
                    } else {
                        break;
                    }
                }
            }
        }
    }
}

```
