---
title: 【剑指Offer】044 和为S的连续正数序列
date: 2019-01-09 11:22:00
---

### 题目描述

时间限制：1秒 空间限制：32768K

小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!

#### 输出描述:

>输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<ArrayList<Integer>> FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer>> list = new ArrayList<ArrayList<Integer>> ();
        if(sum==1) return list;
        int n = (int) (Math.ceil((Math.sqrt(8 * sum + 1) - 1) / 2));
        int tmp = 0;
        int num = 0;
        for (int i=n; i>1; i--){
            tmp = (i + 1) * i / 2;
            if((sum-tmp)%i==0) {
                ArrayList<Integer> arrayList = new ArrayList<Integer>();
                num = (sum - tmp)/i;
                for(int a=1;a<=i;a++)
                    arrayList.add(num+a);
                list.add(arrayList);
            }
        }
        return list;
    }
}
```
