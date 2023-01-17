# [Flip String to Monotone Increasing](https://leetcode.com/problems/flip-string-to-monotone-increasing/description/)

每次可以把0改成1或者1改成0，求至少多少次改动使结果都为0 或者 都为1 或者 所有1都在所有0后面。

## 举个栗子🌰
```java
"100" -> 1 //把第一个1改成0
"1001001" -> 2 //把前面两个1改成0
"10111000" -> 4 //第一个1改成0，最后3个0改成1
```

## 思路

![p926](/pictures/p926.jpg)

## 题解

```cpp
//cpp
int minFlipsMonoIncr(string s) {
    int ones = 0, flips = 0; //ones追踪把1改成0的情况，flips追踪最少的改动
    for (char ch : s) {
        if (ch == '1') ones++;
        else flips++;
        flips = min(flips, ones);
    }
    return flips;
}
```

```go
//go
func minFlipsMonoIncr(s string) int {
    ones, flips := 0, 0
    for _, ch := range s {
        if ch == '1' {
            ones++
        } else {
            flips++
        }
        if ones < flips {
            flips = ones
        }
    }
    return flips
}
```

2021.8.11 by Yong
