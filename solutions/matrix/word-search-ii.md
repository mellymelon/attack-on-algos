# [Word Search II](https://leetcode.com/problems/word-search-ii/)

在矩阵中找出`words`里的词。

## 举个栗子🌰
```java
[["f","o","r"],["l","o","w"],["y","r","e"]] ["flow","flower","floor","fly"] -> ["floor","flow","flower","fly"]
```

## 思路

为了高效匹配`words`里的词，先建立前缀树再深度遍历矩阵。

![p212](/pictures/p212.jpg)

## 题解

```java
//java
List<String> res = new ArrayList();

public List<String> findWords(char[][] board, String[] words) {
    TrieNode root = createTrie(words);
    for (int i = 0; i < board.length; i++) {
        for (int j = 0; j < board[0].length; j++) {
            dfs(board, i, j, root);
        }
    }
    return res;
}

private TrieNode createTrie(String[] words) {
    TrieNode r = new TrieNode();
    for (String w : words) {
        TrieNode cur = r;
        for (char ch : w.toCharArray()) {
            if (cur.children[ch - 'a'] == null)
                cur.children[ch - 'a'] = new TrieNode();
            cur = cur.children[ch - 'a'];
        }
        cur.word = w;
    }
    return r;
}

private void dfs(char[][] board, int i, int j, TrieNode cur) {
    char ch = board[i][j];
    if (ch == '$' || cur.children[ch - 'a'] == null) return;
    cur = cur.children[ch - 'a'];
    if (cur.word != null) {
        res.add(cur.word);
        cur.word = null; //设为null以防止重复添加同一个词。这里没有马上return，因为可能还有同样前缀的词，比如ab和abc
    }
    board[i][j] = '$'; //标记'$'防止同一轮dfs重复搜索
    if (i > 0) dfs(board, i - 1, j, cur); //上
    if (j > 0) dfs(board, i, j - 1, cur); //左
    if (i < board.length - 1) dfs(board, i + 1, j, cur); //右
    if (j < board[0].length - 1) dfs(board, i, j + 1, cur); //下
    board[i][j] = ch; //恢复以便下一轮dfs搜索
}

class TrieNode {
    TrieNode[] children = new TrieNode[26];
    String word;
}
```

```js
//js
var findWords = function(board, words) {
    let res = [], root = createTrie(words)
    for (let i = 0; i < board.length; i++) {
        for (let j = 0; j < board[0].length; j++) {
            dfs(board, i, j, root, res)
        }
    }
    return res
}

var createTrie = function(words) {
    let r = new TrieNode().root
    for (w of words) {
        let cur = r
        for (ch of w) {
            if (!cur[ch]) cur[ch] = {}
            cur = cur[ch]
        }
        cur.word = w
    }
    return r
}

var TrieNode = function() {
    this.root = {}
}

var dfs = function(board, i, j, cur, res) {
    let ch = board[i][j]
    if (ch == '$' || !cur[ch]) return
    cur = cur[ch]
    if (cur.word) {
        res.push(cur.word)
        cur.word = null
    }
    board[i][j] = '$'
    if (i > 0) dfs(board, i - 1, j, cur, res)
    if (j > 0) dfs(board, i, j - 1, cur, res)
    if (i < board.length - 1) dfs(board, i + 1, j, cur, res)
    if (j < board[0].length - 1) dfs(board, i, j + 1, cur, res)
    board[i][j] = ch
}
```

```go
//go
func findWords(board [][]byte, words []string) []string {
    m, n := len(board), len(board[0])
    var res []string
    
    var dfs func(i, j int, cur *trieNode)
    dfs = func(i, j int, cur *trieNode) {
        ch := board[i][j]
        if ch == '$' || cur.children[ch - 97] == nil { return }
        cur = cur.children[ch - 97];
        if cur.word != "" {
            res = append(res, cur.word)
            cur.word = ""
        }
        board[i][j] = '$'
        if i > 0 { dfs(i - 1, j, cur) }
        if i + 1 < m { dfs(i + 1, j, cur) }
        if j > 0 { dfs(i, j - 1, cur) }
        if j + 1 < n { dfs(i, j + 1, cur) }
        board[i][j] = ch
    }
    
    root := createTrie(words)
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            dfs(i, j, root)
        }
    }
    return res
}

func createTrie(words []string) *trieNode {
    r := &trieNode{}
    for _, w := range words {
        cur := r
        for _, ch := range w {
            if cur.children[ch - 97] == nil { cur.children[ch - 97] = &trieNode{} }
            cur = cur.children[ch - 97]
        }
        cur.word = w
    }
    return r
}

type trieNode struct {
    children [26]*trieNode
    word string
}
```

2020.6.30 by Yong