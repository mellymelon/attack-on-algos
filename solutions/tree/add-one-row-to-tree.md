# [Add One Row to Tree](https://leetcode.com/problems/add-one-row-to-tree/)

在二叉树第`depth`层（1-based）插入一层值都是`val`的节点。

**备注**

- 如果插在第一层（根节点那层），就让根节点变成新插入节点的左子节点。
- 插入一个节点时，如果原来的节点是左子节点，就让这个左子节点变成新插入节点的左子节点。右子节点同理。

## 举个栗子🌰
```java
[1,2,3] val=4 depth=1 -> [4,1,null,2,3] //如果插在根节点那层，根节点变成左子树
[1,2,3,4,5,6,7,8,9] -> [1,2,3,1,1,1,1,4,null,null,5,6,null,null,7,8,9]
```

## 思路

![p623.jpg](/pictures/p623.jpg)

dfs或者bfs都可以。

如果dfs，深度搜索的同时记录当前深度，到达指定的那层时就把当前节点变成新插入节点的子节点并返回。

如果bfs，当子节点位于指定的那层时，把当前子节点变成新插入节点的子节点，新插入节点再变成当前子节点。

## 题解

### DFS

```go
//go
func addOneRow(root *TreeNode, val int, depth int) *TreeNode {
    var dfs func(node *TreeNode, curDepth int, isLeft bool) *TreeNode
    dfs = func(node *TreeNode, curDepth int, isLeft bool) *TreeNode {
        if curDepth == 1 {
            if isLeft { return &TreeNode{val, node, nil} } //如果当前节点是左子树，它会变成新插入节点的左子树
            return &TreeNode{val, nil, node} //否则当前节点变成新插入节点的右子树
        }
        if node == nil { return nil }
        node.Left = dfs(node.Left, curDepth - 1, true)
        node.Right = dfs(node.Right, curDepth - 1, false)
        return node
    }
    return dfs(root, depth, true)
}
```

### BFS

```go
//go
func addOneRow(root *TreeNode, val int, depth int) *TreeNode {
    if depth == 1 { return &TreeNode{val, root, nil} }
    q := []*TreeNode{root}
    for len(q) > 0 {
        depth--
        for sz := len(q); sz > 0; sz-- {
            cur := q[0]
            q = q[1:]
            if depth == 1 { //此时下一层就是要插入新节点的一层
                cur.Left = &TreeNode{val, cur.Left, nil} //当前左子树变成新节点的左子树，新节点变成当前左子树
                cur.Right = &TreeNode{val, nil, cur.Right}
            }
            if cur.Left != nil { q = append(q, cur.Left) }
            if cur.Right != nil { q = append(q, cur.Right) }
        }
    }
    return root
}
```

2022.10.5 by Yong