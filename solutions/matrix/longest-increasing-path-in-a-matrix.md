# [Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

求矩阵中最长严格递增序列的长度。

## 举个栗子🌰
```java
[[3,6,7],[4,5,8],[7,6,9]] -> 7
```

## 思路

![p329.jpg](/pictures/p329.jpg)

边深入矩阵边记录从某一格开始最长可以构造多长的序列。

## 题解

```java
//java
int[][] memo;
int[][] DIRS = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

public int longestIncreasingPath(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length, res = 0;
    memo = new int[m][n];
    for (int r = 0; r < m; r++) {
        for (int c = 0; c < n; c++) {
            res = Math.max(res, dfs(matrix, r, c));
        }
    }
    return res;
}

private int dfs(int[][] matrix, int r, int c) {
    if (memo[r][c] != 0) return memo[r][c];
    int curMax = 1;
    for (int[] dir : DIRS) {
        int y = r + dir[0], x = c + dir[1];
        if (0 <= y && y < matrix.length && 0 <= x && x < matrix[0].length && matrix[y][x] > matrix[r][c]) {
            curMax = Math.max(curMax, 1 + dfs(matrix, y, x));
        }
    }
    memo[r][c] = curMax;
    return curMax;
}
```

```cpp
//cpp
int[][] memo; //记录访问过的格子的最长递增长度，优化搜索时间
int[][] DIRS = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}}; //dfs的四个方向

public int longestIncreasingPath(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length, res = 0;
    memo = new int[m][n];
    for (int r = 0; r < m; r++) {
        for (int c = 0; c < n; c++) {
            res = Math.max(res, dfs(matrix, r, c));
        }
    }
    return res;
}

private int dfs(int[][] matrix, int r, int c) { //调用dfs前会确保在矩阵范围内
    if (memo[r][c] != 0) return memo[r][c];
    int curMax = 1; //当前序列长度为1，即当前数字
    for (int[] dir : DIRS) {
        int y = r + dir[0], x = c + dir[1];
        if (0 <= y && y < matrix.length && 0 <= x && x < matrix[0].length && matrix[y][x] > matrix[r][c]) { //在矩阵范围内且比当前格大才搜索
            curMax = Math.max(curMax, 1 + dfs(matrix, y, x)); //注意+1，包括当前格
        }
    }
    memo[r][c] = curMax;
    return curMax;
}
```

```go
//go
func longestIncreasingPath(matrix [][]int) int {
    m, n := len(matrix), len(matrix[0])
    memo := make([][]int, m)
    for i := 0; i < m; i++ { memo[i] = make([]int, n) }
    DIRS := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}

    var dfs func(r, c int) int
    dfs = func(r, c int) int {
        if memo[r][c] != 0 { return memo[r][c] }
        curMax := 1
        for _, d := range DIRS {
            y, x := r + d[0], c + d[1]
            if 0 <= y && y < m && 0 <= x && x < n && matrix[y][x] > matrix[r][c] {
                curMax = max(curMax, 1 + dfs(y, x))
            }
        }
        memo[r][c] = curMax
        return curMax
    }
    
    res := 0
    for r := 0; r < m; r++ {
        for c := 0; c < n; c++ {
            res = max(res, dfs(r, c))
        }
    }
    return res
}

func max(a, b int) int {
    if a > b { return a }
    return b
}
```

2021.8.6 by Yong