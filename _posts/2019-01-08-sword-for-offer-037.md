---
title: 【剑指Offer】037 第一个只出现一次的字符
date: 2019-01-08 20:26:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.

```java
import java.util.LinkedHashMap;

public class Solution {
    public int FirstNotRepeatingChar(String str) {
        LinkedHashMap <Character, Integer> map = new LinkedHashMap<Character, Integer>();
        for(int i=0; i<str.length(); i++){
            if(map.containsKey(str.charAt(i))){
                int time = map.get(str.charAt(i));
                map.put(str.charAt(i), ++time);
            }
            else {
                map.put(str.charAt(i), 1);
            }
        }
        int pos = -1;  
        int i=0;
        for(; i<str.length(); i++){
            char c = str.charAt(i);
            if (map.get(c) == 1) {
                return i;
            }
        }
        return pos;
    }
}
```
