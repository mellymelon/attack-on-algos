# [Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

给定完全二叉树，求其节点个数。

**备注**

- 要求少于O(n)时间

## 举个栗子🌰
```java
[1,2,3,4,5,6] -> 6
```

## 思路

因为完全二叉树的节点都往左靠，所以节点的高度可以通过不断往左走计算。

对于左右子树的高度: 

如果相等说明左子树是满二叉树, 需要进一步统计右子树的节点数(最后一层最后出现的节点必然在右子树中)。

如果不等说明右子树是高度小于左子树的满二叉树, 需要进一步统计左子树的节点数(最后一层最后出现的节点必然在左子树中)。

满二叉树的节点数为`1 << height`，或者说2的`height`次方。

![p222](/pictures/p222.jpg)

## 题解

```java
//java
public int countNodes(TreeNode root) {
    if (root == null) return 0;
    int heightL = getHeight(root.left);
    int heightR = getHeight(root.right);
    if (heightL == heightR)
        return (1 << heightL) + countNodes(root.right); //高度相等说明左边满了，先加左子树的节点数再继续统计右边
    return (1 << heightR) + countNodes(root.left); //高度不相等说明右边满了，先加右子树的节点数再继续统计左边
}

private int getHeight(TreeNode node) { //计算当前节点到叶节点的距离
    int res = 0;
    while (node != null) {
        node = node.left;
        res++;
    }
    return res;
}
```

```cpp
//cpp
int countNodes(TreeNode* root) {
    if (!root) return 0;
    int heightL = getHeight(root->left);
    int heightR = getHeight(root->right);
    if (heightL == heightR) return (1 << heightR) + countNodes(root->right);
    return (1 << heightR) + countNodes(root->left);
}

int getHeight(TreeNode* node) {
    int res = 0;
    while (node) res++, node = node->left;
    return res;
}
```

```go
//go
func countNodes(root *TreeNode) int {
    if root == nil { return 0 }
    getHeight := func(node *TreeNode) int {
        res := 0
        for ; node != nil; node = node.Left { res++ }
        return res
    }
    heightL, heightR := getHeight(root.Left), getHeight(root.Right)
    if heightL == heightR {
        return 1 << heightL + countNodes(root.Right)
    }
    return 1 << heightR + countNodes(root.Left)
}
```

2020.6.23 by Yong