# [Leaf-Similar Trees](https://leetcode.com/problems/leaf-similar-trees/description/)

判断两棵二叉树所有叶节点的值是否相同。

## 举个栗子🌰
```java
root1=[6,5,2,4] root2=[1,3,2,4] -> true //[4,2] == [4,2]
```

## 思路

用数组分别记录两棵树叶节点的值再比较即可，叶节点可以通过递归或者迭代找到。

## 题解 1

递归/深度优先搜索

```java
//java
public boolean leafSimilar(TreeNode root1, TreeNode root2) {
    List<Integer> seq1 = new ArrayList<>(), seq2 = new ArrayList<>();
    dfs(root1, seq1);
    dfs(root2, seq2);
    return seq1.equals(seq2);
}

private void dfs(TreeNode node, List<Integer> seq) {
    if (node == null) return;
    if (node.left == null && node.right == null) seq.add(node.val);
    dfs(node.left, seq);
    dfs(node.right, seq);
}
```

```js
//js
var leafSimilar = function (root1, root2) {
    const dfs = (node, seq) => {
        if (!node) return
        if (!node.left && !node.right) seq.push(node.val)
        dfs(node.left, seq)
        dfs(node.right, seq)
    }
    let seq1 = [], seq2 = []
    dfs(root1, seq1)
    dfs(root2, seq2)
    return seq1.join(",") == seq2.join(",")
};
```

```swift
//swift
func leafSimilar(_ root1: TreeNode?, _ root2: TreeNode?) -> Bool {
    var seq1 = [Int](), seq2 = [Int]()
    dfs(root1, &seq1)
    dfs(root2, &seq2)
    return seq1 == seq2
}
func dfs(_ node: TreeNode?, _ seq: inout[Int]) {
    guard let node = node else { return }
    if node.left == nil && node.right == nil { seq.append(node.val) }
    dfs(node.left, &seq)
    dfs(node.right, &seq)
}
```

```py
#py
def leafSimilar(self, root1: TreeNode, root2: TreeNode) -> bool:
    seq1, seq2 = [], []
    def dfs(node, seq):
        if not node: return
        if not node.left and not node.right: seq.append(node.val)
        dfs(node.left,seq)
        dfs(node.right,seq)
    dfs(root1, seq1)
    dfs(root2, seq2)
    return seq1 == seq2
```

```kt
//kt
fun leafSimilar(root1: TreeNode?, root2: TreeNode?): Boolean {
    val seq1 = mutableListOf<Int>()
    val seq2 = mutableListOf<Int>()
    dfs(root1, seq1)
    dfs(root2, seq2)
    return seq1 == seq2
}

fun dfs(node: TreeNode?, seq: MutableList<Int>) {
    node ?: return
    if (node.left == null && node.right == null) seq.add(node.`val`)
    dfs(node.left, seq)
    dfs(node.right, seq)
}
```

```go
//go
func leafSimilar(root1 *TreeNode, root2 *TreeNode) bool {
    var seq1, seq2 []int
    dfs(root1, &seq1)
    dfs(root2, &seq2)
    if len(seq1) != len(seq2) { return false }
    for i, x := range seq1 {
        if x != seq2[i] { return false }
    }
    return true
}

func dfs(node *TreeNode, seq *[]int) {
    if node == nil { return }
    if node.Left == nil && node.Right == nil {
        *seq = append(*seq, node.Val)
        return
    }
    dfs(node.Left, seq)
    dfs(node.Right, seq)
}
```

## 题解 2

用栈迭代，一发现叶节点不同可提前返回。

```java
//java
public boolean leafSimilar(TreeNode root1, TreeNode root2) {
    Stack<TreeNode> stk1 = new Stack<>(), stk2 = new Stack<>();
    stk1.push(root1);
    stk2.push(root2);
    while (!stk1.isEmpty() && !stk2.isEmpty()) {
        if (getLeaf(stk1) != getLeaf(stk2)) return false; //按序列顺序逐个对比叶节点
    }
    return stk1.isEmpty() && stk2.isEmpty(); //如果叶节点数相同两个栈都会为空，否则考虑其中一棵树的叶节点可能多一些
}

private int getLeaf(Stack<TreeNode> stk) {
    while (true) { //直到找到叶节点为止
        TreeNode node = stk.pop();
        if (node.right != null) stk.push(node.right); //先push右节点可以让左节点在pop时先被处理
        if (node.left != null) stk.push(node.left);
        if (node.left == null && node.right == null) return node.val;
    }
}
```

2020.5.5 by Yong