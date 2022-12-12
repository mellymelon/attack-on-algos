# [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)

爬楼梯，一次可以爬一层或两层，求到第`n`层有多少种爬法。

**备注**

层数`n`范围：`[1, 45]`

## 举个栗子🌰
```java
1 -> 1
2 -> 2
4 -> 5
```

## 思路

动态规划，第`n`层爬法等于前两层爬法之和。

![p70](/pictures/p70.jpg)

## 题解

```java
//java
public int climbStairs(int n) {
    if (n <= 2) return n;
    int pv2 = 1, pv1 = 2;
    while (n-- > 2) {
        int cur = pv1 + pv2;
        pv2 = pv1;
        pv1 = cur;
    }
    return pv1;
}
```

```py
#py
def climbStairs(self, n):
    if n <= 2: return n
    pv1, pv2 = 2, 1
    for _ in range(2, n):
        pv1, pv2 = pv1 + pv2, pv1
    return pv1
```

```cpp
//cpp
int climbStairs(int n) {
    if (n <= 2) return n;
    int pv1 = 2, pv2 = 1;
    while (n-- > 2) {
        int cur = pv1 + pv2;
        pv2 = pv1;
        pv1 = cur;
    }
    return pv1;
}
```

```go
//go
func climbStairs(n int) int {
    if n <= 2 { return n }
    pv2, pv1 := 1, 2
    for i := 2; i < n; i++ {
        pv2, pv1 = pv1, pv2 + pv1
    }
    return pv1
}
```

2020.5.7 by Yong