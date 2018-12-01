---
title: 【剑指Offer】009 斐波那契数列
date: 2018-12-01 14:30:31
---

```java
public class S009 {
    public int Fibonacci(int n) {
        int a=1, b=1, c=0;
        if (n<0)
            return 0;
        else if (n==1 || n==2)
            return 1;
        else
            for (int i=3; i <= n; i++){
                c = a + b;
                b = a;
                a = c;
            }
        return c;
    }
}
```
