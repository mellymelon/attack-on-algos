# [Range Sum of BST](https://leetcode.com/problems/range-sum-of-bst/description/)

求二叉搜索树中`[low,high]`范围内节点值的总和。

## 举个栗子🌰
```java
[5,3,6,2,4] low=5 high=7 -> 11 //5+6
```

## 思路

遍历二叉搜索树，如果节点值在`[low,high]`范围内就加到结果里。

## 题解 1

递归

Time: `O(n)`

Space: `O(h)`

```java
//java
public int rangeSumBST(TreeNode root, int low, int high) {
    if (root == null) return 0;
    if (root.val < low) return rangeSumBST(root.right, low, high); //节点值太小，找更大的右边
    if (root.val > high) return rangeSumBST(root.left, low, high); //节点值太大，找更小的左边
    return root.val + rangeSumBST(root.left, low, high) + rangeSumBST(root.right, low, high); //此时节点值在要求范围内，加到结果里
}
```

## 题解 2

Morris中序遍历

Time: `O(n)`

Space: `O(1)`

```py
#py
def rangeSumBST(self, root, low, high):
    res, cur = 0, root
    while cur:
        if cur.left:
            pre = cur.left
            while pre.right and pre.right != cur: #找左子树最右子节点
                pre = pre.right
            if not pre.right:
                pre.right = cur
                cur = cur.left
            else: #此时左子树已遍历完
                if low <= cur.val <= high: #如果节点值在要求范围内就加到结果里
                    res += cur.val
                pre.right = None
                cur = cur.right #继续遍历右子树
        else: #此时左子树已遍历完
            if low <= cur.val <= high: #如果节点值在要求范围内就加到结果里
                res += cur.val
            cur = cur.right #继续遍历右子树
    return res
```

2020.11.16 by Yong