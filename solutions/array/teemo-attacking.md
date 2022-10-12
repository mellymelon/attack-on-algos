# [Teemo Attacking](https://leetcode.com/problems/teemo-attacking/)

给定`timeSeries`表示施毒时间点，`duration`表示中毒持续时间，求总中毒时长。

**备注**

- 如果中毒时再次施毒，则中毒时间重新计算。

## 举个栗子🌰
```java
[1,4] 2 -> 4
[1,2,6] 3 -> 7
```

## 思路

因为中毒时间是相同的（可视化为长度相同的长方形），如果有重叠，那么有效的中毒时间不能是重叠的部分，求两者的位置差即可。

![p495.jpg](/pictures/p495.jpg)

## 题解

```cpp
//cpp
int findPoisonedDuration(vector<int>& timeSeries, int duration) {
    int res = 0, prev = -duration;
    for (int t : timeSeries) {
        res += min(t - prev, duration); //新增有效中毒时间为两个中毒时间段的差，最长不会超过一次中毒的时间
        prev = t;
    }
    return res;
}
```

```cpp
//cpp
int findPoisonedDuration(vector<int>& A, int duration) {
    int res = 0;
    for (int i = 0; i + 1 < A.size(); i++)
        res += min(A[i + 1] - A[i], duration);

    return res + duration; //最后一次施毒没有遍历到，这里补上
}
```

2021.11.17 by Yong