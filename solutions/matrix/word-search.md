# [Word Search](https://leetcode.com/problems/word-search/)

求给定单词是否存在于矩阵中。

## 举个栗子🌰
```java
[["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]] "ABCCED" -> true
```

## 思路

深度优先搜索，匹配到一个字母后再继续往其余三个方向搜索，直到匹配完单词的所有字母。

## 题解

```java
//java
char[][] board;
String word;

public boolean exist(char[][] board, String word) {
    this.board = board;
    this.word = word;
    for (int r = 0; r < board.length; r++) {
        for (int c = 0; c < board[0].length; c++) {
            if (board[r][c] == word.charAt(0) && dfs(r, c, 0)) { //和单词的首字母相同就dfs
                return true;
            }
        }
    }
    return false;
}

private boolean dfs(int r, int c, int i) { //i表示匹配的字母数，要和单词的第i个字母比对
    if (i == word.length()) return true;
    if (r < 0 || r >= board.length || c < 0 || c >= board[0].length) return false;
    if (board[r][c] != word.charAt(i)) return false; //不匹配就不继续搜索
    char tmp = board[r][c];
    board[r][c] = ' '; //做标记，避免同一轮dfs重复使用当前字母
    if (dfs(r - 1, c, i + 1) || dfs(r + 1, c, i + 1) ||
        dfs(r, c - 1, i + 1) || dfs(r, c + 1, i + 1)) return true; //单词存在于任意一条路径即可
    board[r][c] = tmp; //为下一轮dfs恢复当前字母
    return false;
}
```

```js
//js
var exist = function(board, word) {
    const dfs = function(r, c, i) {
        if (i == word.length) return true
        if (r < 0 || r >= board.length || c < 0 || c >= board[0].length) return false
        if (board[r][c] != word[i]) return false
        let tmp = board[r][c]
        board[r][c] = ' '
        if (dfs(r - 1, c, i + 1) || dfs(r + 1, c, i + 1) ||
            dfs(r, c - 1, i + 1) || dfs(r, c + 1, i + 1)) return true
        board[r][c] = tmp
        return false
    }
    for (let r = 0; r < board.length; r++) {
        for (let c = 0; c < board[0].length; c++) {
            if (board[r][c] == word[0] && dfs(r, c, 0)) return true
        }
    }
    return false
}
```

```swift
//swift
func exist(_ board: [[Character]], _ word: String) -> Bool {
    let word = Array(word)
    var board = board
    for r in 0..<board.i {
        for c in 0..<board[0].i {
            if board[r][c] == word[0] && dfs(&board, r, c, word, 0) { return true }
        }
    }
    return false
}
func dfs(_ board: inout [[Character]], _ r: Int, _ c: Int, _ word: [Character], _ i: Int) -> Bool {
    if i == word.i { return true }
    if r < 0 || r >= board.i || c < 0 || c >= board[0].i { return false }
    if board[r][c] != word[i] { return false }
    let tmp = board[r][c]
    board[r][c] = " "
    if dfs(&board, r - 1, c, word, i + 1) || dfs(&board, r + 1, c, word, i + 1) ||
    dfs(&board, r, c - 1, word, i + 1) || dfs(&board, r, c + 1, word, i + 1) { return true }
    board[r][c] = tmp
    return false
}
```

```py
#py
def exist(self, board, word):
    m, n = len(board), len(board[0])

    def dfs(r, c, i):
        if i == len(word): return True
        if r < 0 or r >= m or c < 0 or c >= n: return False
        if board[r][c] != word[i]: return False
        tmp = board[r][c]
        board[r][c] = ' '
        if dfs(r - 1, c, i + 1) or dfs(r + 1, c, i + 1) or \
        dfs(r, c - 1, i + 1) or dfs(r, c + 1, i + 1): return True
        board[r][c] = tmp
        return False

    for r in range(m):
        for c in range(n):
            if board[r][c] == word[0] and dfs(r, c, 0): return True
    return False
```

```go
//go
func exist(board [][]byte, word string) bool {
    m, n := len(board), len(board[0])
    var dfs func (r, c, i int) bool
    dfs = func (r, c, i int) bool {
        if i == len(word) { return true }
        if r < 0 || r >= m || c < 0 || c >= n { return false }
        if board[r][c] != word[i] { return false }
        tmp := board[r][c]
        board[r][c] = ' '
        i++ //这里++下面4个i就不用+1
        if dfs(r - 1, c, i) || dfs(r + 1, c, i) || dfs(r, c - 1, i) || dfs(r, c + 1, i) { return true }
        board[r][c] = tmp
        return false
    }
    for r := 0; r < m; r++ {
        for c := 0; c < n; c++ {
            if board[r][c] == word[0] && dfs(r, c, 0) { return true }
        }
    }
    return false
}
```

```cpp
//cpp
string w;

bool exist(vector<vector<char>>& board, string word) {
    w = word;
    for (int r = 0; r < board.size(); r++)
        for (int c = 0; c < board[0].size(); c++)
            if (board[r][c] == w[0] && dfs(r, c, 0, board))
                return true;
    return false;
}

bool dfs(int r, int c, int i, vector<vector<char>>& board) {
    if (i == w.size()) return true;
    if (r < 0 || r == board.size() || c < 0 || c == board[0].size() || board[r][c] == '.') return false;
    if (board[r][c] == w[i]) { //匹配才继续搜索
        char tmp = board[r][c];
        board[r][c] = '.';
        if (dfs(r - 1, c, ++i, board) || dfs(r + 1, c, i, board) || dfs(r, c - 1, i, board) || dfs(r, c + 1, i, board)) return true;
        board[r][c] = tmp;
    }
    return false;
}
```
2020.7.21 by Yong