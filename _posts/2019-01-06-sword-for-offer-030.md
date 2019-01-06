---
title: 【剑指Offer】030 字符串的排列
date: 2019-01-06 10:33:00
---

### 递归算法

```java
import java.util.ArrayList;
import java.util.Collections;
public class Solution {
    public ArrayList<String> Permutation(String str) {
        ArrayList<String> res = new ArrayList<String>();
        if(str.length()==0 || str==null)return res;
        int n = str.length();
        helper(res, 0, str.toCharArray());
        Collections.sort(res);
        return res;
    }

    public void helper(ArrayList<String> res, int index, char[] s) {
        if(index==s.length-1)
            res.add(new String(s));
        for(int i=index; i<s.length; i++) {
            if(i==index || s[index]!=s[i]) {
                swap(s, index, i);
                helper(res, index+1, s);
                swap(s, index, i);
            }
        }
    }

    public void swap(char[] t, int i, int j) {
        char c = t[i];
        t[i] = t[j];
        t[j] = c;
    }
}
```
