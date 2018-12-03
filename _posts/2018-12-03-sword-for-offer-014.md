---
title: 【剑指Offer】014 数值的整数次方
date: 2018-12-03 09:31:31
---

```java
public class S014 {
    public double doPower(double base, int exponent) {
        double temp = 1;
        for (int i=1; i <= exponent; i++) {
            temp *= base;
            if (temp > 1.7976931348623157E308)
                return -1;
        }
        return temp;
    }
    public double Power(double base, int exponent){
        if (exponent > 0)
            return doPower(base, exponent);
        else if (exponent < 0)
            return 1.0 / doPower(base, -exponent);
        return 1;
    }
}
```
