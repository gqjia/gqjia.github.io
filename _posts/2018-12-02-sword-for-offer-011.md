---
title: 【剑指Offer】011 变态跳台阶
date: 2018-12-02 17:50:31
---

```java
public class s011 {
    public int JumpFloorII(int target) {
        if (target <= 0) return 0;
        else if (target == 1) return 1;
        else return 2 * JumpFloorII(target - 1);
    }
}
```
