# [Where Will the Ball Fall](https://leetcode.com/problems/where-will-the-ball-fall/)

`1`表示往右滚，`-1`表示往左滚，求每个球最终会滚到哪一列，如果不能滚到底就记`-1`。

## 举个栗子🌰
```java
[[-1,-1,-1,1]] -> [-1,0,1,-1] //第0个和第3个球滚出界，记-1；第1个球滚到了第0列，第2个球滚到了第1列
[[1,1,-1,1],[-1,1,1,-1]] -> [2,-1,-1,-1] //第1个球和第2个球滚动方向相反，呈V形阻塞
```

## 思路

对于每个球，模拟它从上往下滚的位置。

![p1706](/pictures/p1706.jpg)

## 题解

```go
//go
func findBall(grid [][]int) []int {
    m, n := len(grid), len(grid[0])
    res := make([]int, n)
    for i := 0; i < n; i++ { //第i个球
        cur := i //第i个球一开始在第i列
        for r := 0; r < m; r++ { //从上往下滚
            c := cur + grid[r][cur] //即将从当前列滚到第c列
            if c < 0 || c >= n || grid[r][cur] != grid[r][c] { //如果越界或者滚到有冲突的列
                res[i] = -1 //无法滚到最底
                break
            }
            res[i] = c //记录第i个球滚到了第c列
            cur = c //目前滚到了第c列
        }
    }
    return res
}
```

2022.11.1 by Yong