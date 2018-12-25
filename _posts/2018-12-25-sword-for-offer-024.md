---
title: 【剑指Offer】024 栈的压入、弹出序列
date: 2018-12-25 10:38:31
---

```java
import java.util.ArrayList;
import java.util.Stack;

public class Solution {
    public boolean IsPopOrder(int [] pushA,int [] popA) {
        if(pushA.length == 0 || popA.length == 0)
            return false;
        Stack<Integer> s = new Stack<Integer>();
        int popIndex = 0;
        for(int i = 0; i< pushA.length;i++){
            s.push(pushA[i]);
            while(!s.empty() &&s.peek() == popA[popIndex]){
                s.pop();
                popIndex++;
            }
        }
        return s.empty();
    }
}
```
