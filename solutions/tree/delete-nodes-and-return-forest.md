# [Delete Nodes And Return Forest](https://leetcode.com/problems/delete-nodes-and-return-forest/description/)

按删除清单`to_delete`里的值删除节点，并返回剩下的节点“森林”（相连的节点）。

## 举个栗子🌰
```java
[1,2,3,4,5,6,7] [3,5] -> [[1,2,null,4],[6],[7]]
```

## 思路

![p1110](/pictures/p1110.jpg)

因为答案要按根节点分组，所以只在当前节点是根节点且不需要删除时才把节点添加进答案。

如果要删当前节点，它的子节点都会各自成为根节点。

## 题解

```java
//java
Set<Integer> toDel = new HashSet<>();
List<TreeNode> res = new ArrayList<>();

public List<TreeNode> delNodes(TreeNode root, int[] to_delete) {
    for (int x : to_delete) toDel.add(x); //放进集合里以便O(1)时间查询
    dfs(root, true);
    return res;
}

private TreeNode dfs(TreeNode node, boolean isRoot) {
    if (node == null) return null;
    boolean delCur = toDel.contains(node.val);
    if (!delCur && isRoot) res.add(node); //不在删除清单内，且是根节点才添加，因为答案是按根节点分组
    node.left = dfs(node.left, delCur); //如果删了当前节点，左子节点就变成root
    node.right = dfs(node.right, delCur); //右子节点同理
    return delCur ? null : node;
}
```

```js
//js
var delNodes = function(root, to_delete) {
    let res = [], toDel = new Set(to_delete)
    const dfs = (node, isRoot) => {
        if (!node) return null
        const delCur = toDel.has(node.val)
        if (!delCur && isRoot) res.push(node)
        node.left = dfs(node.left, delCur)
        node.right = dfs(node.right, delCur)
        return delCur ? null : node
    }
    dfs(root, true)
    return res
};
```

2022.12.6 by Yong