---
title: 【剑指Offer】013 二进制中1的个数
date: 2018-12-02 17:53:31
---

### 用函数

```java
public class S013 {
    public int NumberOf1(int n) {
        int t = 0;
        char[] ch = Integer.toBinaryString(n).toCharArray();
        for (int i = 0; i < ch.length; i++)
            if (ch[i] == '1')
                t++;
        return t;
    }
}
```

### 正常算法

```java
public class S013 {
    public int NumberOf1(int n) {
      int count = 0;
      int flag = 1;
      while (flag != 0) {
          if ((n & flag) != 0) {
              count++;
          }
          flag = flag << 1;
      }
      return count;
    }
}
```

### 最优解

```java
public class S013 {
    public int NumberOf1(int n) {
      int count = 0;
        while (n != 0) {
            ++count;
            n = (n - 1) & n;
        }
        return count;
    }
}
```
