# [Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

设计一个TimeMap支持按键根据某个时刻获取对应的值。

## 举个栗子🌰
```java
TimeMap timeMap = new TimeMap();
timeMap.set("y", "yong", 1);
timeMap.get("y", 2); //->"yong"
timeMap.set("y", "yogurt", 3);
timeMap.get("y", 3); //->"yogurt"
timeMap.get("y", 1); //->"yong"
```

## 思路

![p981.jpg](/pictures/p981.jpg)

为了随时查询到某个键对应的值，可以用数组记录一个键对应的所有出现过的值及时刻，数组的每个元素表示为`(value,timestamp)`。

因为`(value,time)`是顺着时间添加的，所以同一个键对应的元素天然是按时间排序的，因此可以用二分法减少查询时间。

## 题解

```py
#py
class TimeMap:

    def __init__(self):
        self.mp = defaultdict(list) #记录同一个key出现过的所有的value及时刻

    def set(self, key, value, timestamp):
        self.mp[key].append((value, timestamp))

    def get(self, key, timestamp):
        A = self.mp[key]
        if not A or timestamp < A[0][1]: return "" #不写or后面的条件也可以，只是为了提早返回以优化速度
        if timestamp >= A[-1][1]: return A[-1][0] #这一行也可以省略，理由同上
        L, R = 0, len(A) - 1
        while L <= R: #二分法找mp[key]里时间最接近查询时刻的元素的值
            M = L + R >> 1
            if A[M][1] == timestamp: return A[M][0] #刚好有在这个时刻设的值，直接返回
            if A[M][1] > timestamp: R = M - 1 #当前时刻晚于查询的时刻，肯定不在右边范围
            else: #当前时刻不晚于查询的时刻，可能后面还有某个时刻更新了值
                res = A[L][0] #L可能会越界退出循环，先保存一下对应的值保底
                L = M + 1
        
        return res
```

2022.10.6 by Yong