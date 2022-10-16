# [Minimum Difficulty of a Job Schedule](https://leetcode.com/problems/minimum-difficulty-of-a-job-schedule/)

按顺序完成jobDifficulty（`jd`）里的任务，其中`jd[i]`表示任务难度。

假如要把任务分配到`d`天内完成，每天的任务难度取决于最高的工作难度。

求最低的总工作难度。

**备注**

- 任务数范围 [1, 300]
- 任务难度范围 [0, 1000]
- 必须每天干活，如果不能每天都分配到任务，返回`-1`

## 举个栗子🌰
```java
[6,5,4,3,2,1] 3 -> 9 //第一天max(6,5,4,3)，第二天2，第三天1。6+2+1=9
```

## 思路

![p1335.jpg](/pictures/p1335.jpg)

## 题解

```java
//java
private int[][] memo;

public int minDifficulty(int[] jd, int d) {
    int n = jd.length;
    if (n < d) return -1; //不能每天都分配到任务
    memo = new int[n][d + 1];
    for (int[] row : memo) Arrays.fill(row, -1);
    return dp(jd, 0, d);
}

private int dp(int[] jd, int i, int d) {
    if (memo[i][d] != -1) return memo[i][d];
    if (d == 1) {
        int res = max(jd, i);
        memo[i][d] = res;
        return res;
    }
    int res = 300000, curMax = 0;
    for (int j = i; j < jd.length - d + 1; j++) {
        curMax = Math.max(curMax, jd[j]);
        res = Math.min(res, curMax + dp(jd, j + 1, d - 1));
    }
    memo[i][d] = res;
    return res;
}

private int max(int[] A, int beg) {
    int res = 0;
    for (int i = beg; i < A.length; i++)
        res = Math.max(res, A[i]);
    return res;
}
```

```py
#py
def minDifficulty(self, jd, d):
    @cache
    def dp(i, d): # i：第i个任务；d：还剩d天
        if d == 1: return max(jd[i:]) # 只剩一天，分配剩下的全部任务，取最高难度
        res, curMax = 300000, 0
        for j in range(i, n - d + 1):
            curMax = max(curMax, jd[j]) # 取当前最高难度
            res = min(res, curMax + dp(j + 1, d - 1)) # 当前最高难度加上往后各日子里的最高难度，取最小的总和
        return res
        
    n = len(jd)
    return -1 if n < d else dp(0, d)
```

2022.10.16 by Yong