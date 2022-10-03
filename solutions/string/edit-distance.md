# [Edit Distance](https://leetcode.com/problems/edit-distance/)

求至少需要多少个改动才能把word1改成word2。改动有三种，增加、删除、替换。

## 举个栗子🌰
```java
"horse" "ros" -> 3 //h换成r，再删掉第三个r和最后一个e
```

## 思路

一下子把word1改成word2很有难度，但逐个字母比对着改就容易得多。

每次改动的同时记录怎么改才能使次数最少，下次改动也基于当前记录的最少次数。

这个过程可以用动态规划实现，如下图：

![p72.jpg](/pictures/p72.jpg)

## 题解

```java
//java,4ms,89%/38.8MB,74%
public int minDistance(String word1, String word2) {
    int m = word1.length(), n = word2.length();
    if (m * n == 0) return m + n; //如果其中一个为空字符串，则答案为另一个字符串的长度
    int[][] dp = new int[m + 1][n + 1];
    for (int i = 0; i <= m; i++) dp[i][0] = i; //第一列填0到i。注意取到m
    for (int i = 0; i <= n; i++) dp[0][i] = i; //第一行填0到i。注意取到n
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (word1.charAt(i) == word2.charAt(j)) { //如果字母相等，不用+1，直接等于左上dp结果。因为word1和word2都有这个字母，不需要任何改动
                dp[i + 1][j + 1] = dp[i][j];
            }
            else {
                dp[i + 1][j + 1] = Math.min(dp[i][j], Math.min(dp[i + 1][j], dp[i][j + 1])) + 1; //左上、左、上之中的最小者+1。+1是在它们记录的改动数的基础上再加一个改动
            }
        }
    }
    return dp[m][n];
}
```

```py
#py,160ms,75%/18MB,21%
def minDistance(self, word1: str, word2: str) -> int:
    m, n = len(word1), len(word2)
    if m * n == 0: return m + n
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, n + 1): dp[0][i] = i
    for i in range(1, m + 1): dp[i][0] = i
    for i, j in product(range(m), range(n)):
        if word1[i] == word2[j]: dp[i + 1][j + 1] = dp[i][j]
        else: dp[i + 1][j + 1] = min(dp[i + 1][j], dp[i][j], dp[i][j + 1]) + 1
    return dp[-1][-1]
```

2021.5.23 by Yong