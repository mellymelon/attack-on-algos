# [Nearest Exit from Entrance in Maze](https://leetcode.com/problems/nearest-exit-from-entrance-in-maze/)

求从给定初始位置到矩阵任意一边的最短距离，如果不能到达边界就返回-1。

**备注**

- 矩阵中的`+`表示不能通过的墙壁，`.`表示可通过。

- 如果一开始就在边界上，不能返回0，要找另外的边界作为出口。

## 举个栗子🌰
```java
[[".",".",".",".",".","."]] [0,2] -> 1 //只有一行，哪一格都可以是出口
[
  ["+","+","+","+","+"],
  [".",".",".",".","."],
  ["+","+","+","+","+"]
] [1,1] -> 1 //往左走1步或者往右走3步，要求最短，因此是1步
```

## 思路

DFS不能保证距离最短，因此用BFS，最先到达边界的路线就是最短路线。

从给定初始位置出发，广度优先搜索直到边界或者碰壁返回-1。

![p1926](/pictures/p1926.jpg)

## 题解

```py
#py
def nearestExit(self, maze, ent):
    m, n = len(maze), len(maze[0])
    DIRS = [0, 1, 0, -1, 0]
    steps = 0
    maze[ent[0]][ent[1]] = '+'
    q = deque([(ent[0], ent[1])])
    while q:
        sz = len(q)
        for _ in range(sz):
            r, c = q.popleft()
            for i in range(4):
                nr, nc = r + DIRS[i], c + DIRS[i + 1]
                if nr < 0 or nr >= m or nc < 0 or nc >= n or maze[nr][nc] == '+':
                    continue #如果越界或者碰壁，跳过
                if nr == 0 or nr == m - 1 or nc == 0 or nc == n - 1: #到达任一边界
                    return steps + 1
                maze[nr][nc] = '+' #标记为墙以避免重复搜索
                q.append((nr, nc))
        steps += 1
    return -1
```

```cpp
//cpp
int nearestExit(vector<vector<char>>& maze, vector<int>& ent) {
    int m = maze.size(), n = maze[0].size(), steps = 0;
    int DIRS[5]{0, 1, 0, -1, 0};

    queue<pair<int, int>> q;
    q.push({ent[0], ent[1]});
    maze[ent[0]][ent[1]] = '+';
    while (!q.empty()) {
        int sz = q.size();
        while (sz--) {
            auto [r, c] = q.front(); q.pop();
            for (int i = 0; i < 4; i++) {
                int nr = r + DIRS[i], nc = c + DIRS[i + 1];
                if (nr < 0 || nr >= m || nc < 0 || nc >= n || maze[nr][nc] == '+')
                    continue;
                if (nr == 0 || nr == m - 1 || nc == 0 || nc == n - 1)
                    return steps + 1;
                maze[nr][nc] = '+';
                q.push({nr, nc});
            }
        }
        steps++;
    }
    return -1;
}
```

2022.11.21 by Yong