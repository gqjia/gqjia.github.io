---
title: 【剑指Offer】012 矩形覆盖
date: 2018-12-02 17:51:31
---

```java
public class S012 {
    public int RectCover(int target) {
        if (target == 0) return 0;
        else if(target  == 1) return 1;
        else if(target == 2) return 2;
        else return RectCover(target-1)+RectCover(target-2);
    }
}
```
