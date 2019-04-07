---
title: 【剑指Offer】010 跳台阶
date: 2018-12-02 17:49:31
---

```java
public class S010 {
    public int JumpFloor(int target) {
        if (target <= 0) return -1;
        else if (target == 1) return 1;
        else if (target ==2) return 2;
        else return  JumpFloor(target-1)+JumpFloor(target-2);
    }
}
```


```Java
public class Solution {
    public int JumpFloor(int n) {
        int a = 2, b = 1, c = 0;
        if (n<0)
            return 0;
        else if (n==1)
            return 1;
        else  if (n==2)
            return 2;
        else
            for (int i=3; i <= n; i++){
                c = a + b;  //F(n - 1)
                b = a;
                a = c;
            }
        return c;
    }
}
```
