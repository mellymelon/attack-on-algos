# [Arithmetic Slices II - Subsequence](https://leetcode.com/problems/arithmetic-slices-ii-subsequence/)

求数组里有几个算术子序列。

**备注**

- 算术子序列至少有3个元素，等差，不一定连续。

## 举个栗子🌰
```java
[2,4,6,8,12] -> 4 //[2,4,6], [4,6,8], [2,4,6,8], [4,8,12]
```

## 思路

![p446](/pictures/p446.jpg)

## 题解

```py
#py
def numberOfArithmeticSlices(self, A):
    res, dp = 0, Counter() #dp记录到每个位置为止不同的差对应的个数
    for i in range(len(A)):
        for j in range(i): #到i之前
            d = A[i] - A[j]
            res += dp[(j, d)] #到当前位置为止满足这个差的序列有几个就加几个
            dp[(i, d)] += dp[(j, d)] + 1
    return res
```

```go
//go
func numberOfArithmeticSlices(A []int) int {
    res, dp := 0, make([]map[int]int, len(A))
    for i, x := range A {
        dp[i] = make(map[int]int)
        for j, y := range A[:i] {
            d := x - y
            res += dp[j][d]
            dp[i][d] += dp[j][d] + 1
        }
    }
    return res
}
```

2022.11.27 by Yong