---
title: 【剑指Offer】007 用两个栈实现队列
date: 2018-12-01 09:11:31
---

```java
import java.util.Stack;

public class S007 {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();

    public void push(int node) {
        stack1.push(node);
    }

    public int pop() {
        while(!stack1.isEmpty())
            stack2.push(stack1.pop());
        int first = stack2.pop();
        while (!stack2.isEmpty())
            stack1.push(stack2.pop());
        return first;
    }
}
```
