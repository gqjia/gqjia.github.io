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
