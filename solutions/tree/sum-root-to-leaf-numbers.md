# [Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/)

求所有由根到叶构成的数字加起来的总和。

## 举个栗子🌰
```java
[1,2,3,4,5,6,7,8,9,0] -> 4020
```

## 思路

![p129.jpg](/pictures/p129.jpg)

边做深度优先搜索边构造数字，到达叶节点后就返回当前构造的数字，最后把所有数字加起来即为所求。

## 题解

```java
//java,0ms,100%/37.3MB,58%
public int sumNumbers(TreeNode root) {
    return dfs(root, 0);
}
public int dfs(TreeNode node, int cur) {
    if (node == null) return 0;
    cur = cur * 10 + node.val;
    if (node.left == null && node.right == null) return cur; //到达叶节点
    return dfs(node.left, cur) + dfs(node.right, cur);
}
```

```js
//js,68ms,60%/36MB,16%
var sumNumbers = function (root) {
    let dfs = function (node, cur) {
        if (!node) return 0
        cur = cur * 10 + node.val
        if (!node.left && !node.right) return cur
        return dfs(node.left, cur) + dfs(node.right, cur)
    }
    return dfs(root, 0)
};
```

```swift
//swift,8ms,98%/20.7MB,90%
func sumNumbers(_ root: TreeNode?) -> Int {
    return dfs(root, 0)
}
func dfs(_ node: TreeNode?, _ cur: Int) -> Int {
    guard let node = node else { return 0 }
    let cur = cur * 10 + node.val
    if node.left == nil && node.right == nil { return cur }
    return dfs(node.left, cur) + dfs(node.right, cur)
}
```

```py
#py,28ms,86%/14.1MB,29%
def sumNumbers(self, root: TreeNode) -> int:
    def dfs(node, cur):
        if not node: return 0
        cur = cur * 10 + node.val
        if not node.left and not node.right: return cur
        return dfs(node.left, cur) + dfs(node.right, cur)
    return dfs(root, 0)
```

```cpp
//cpp,4ms,48%
public:
int sumNumbers(TreeNode* root) {
    return dfs(root, 0);
}

private:
int dfs (TreeNode* node, int cur) {
    if (!node) return 0;
    cur = cur * 10 + node->val;
    if (!node->left && !node->right) return cur;
    return dfs(node->left, cur) + dfs(node->right, cur);
}
```

2020.6.26 by Yong