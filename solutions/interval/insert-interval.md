# [Insert Interval](https://leetcode.com/problems/insert-interval/submissions/)

在给定的一堆区间里插入一个新的区间。

**备注**

- 给定的区间已按左端排序，且没有重叠

## 举个栗子🌰
```java
[] [7,8] -> [[7,8]]
[[1,5]] [2,7] -> [[1,7]]
[[1,2],[5,6],[7,8]] [2,3] -> [[1,3],[5,6],[7,8]]
```

## 思路

![p57.jpg](/pictures/p57.jpg)

只有三种情况：

① 新区间在原有区间右边

② 新区间在原有区间左边

③ 新区间和原有区间重叠（需要融合）

## 题解

```js
//js
var insert = function(A, newI) {
    if (!A.length) return [newI]
    let former = []
    for (var [i, [L, R]] of A.entries()) {
        if (newI[0] > R) former.push([L, R])
        else if (newI[1] < L) {
            i--
            break
        }
        else {
            newI[0] = Math.min(newI[0], L)
            newI[1] = Math.max(newI[1], R)
        }
    }
    return former.concat([newI], A.slice(i + 1))
};
```

```py
#py
def insert(self, A, newI):
    if not A: return [newI]
    former = [] #在新区间之前的区间
    for i, (L, R) in enumerate(A):
        if newI[0] > R: #新区间在原有区间右边且没有交集
            former.append([L, R])
        elif newI[1] < L: #新区间在原有区间左边，剩下的可以不遍历了，按i的位置直接取剩下的区间
            i -= 1 #后半部分答案从i开始。-1是为了修正返回时的i+1，比如[2,7]插入[[1,5]]
            break
        else: #有交集，融合操作通过修改新区间的左右边界实现
            newI[0] = min(newI[0], L)
            newI[1] = max(newI[1], R)
    return former + [newI] + A[i+1:] #i+1才有可能构造空数组。如果可以遍历到最后一个区间，A[i+1:]就是一个空数组
```

```cpp
//cpp
vector<vector<int>> insert(vector<vector<int>>& A, vector<int>& newI) {
    if (A.empty()) return {newI};
    vector<vector<int>> former;
    int i = 0;
    while (i < A.size()) {
        if (newI[0] > A[i][1]) former.push_back(A[i]);
        else if (newI[1] < A[i][0]) break;
        else {
            newI[0] = min(newI[0], A[i][0]);
            newI[1] = max(newI[1], A[i][1]);
        }
        i++;
    }
    vector<vector<int>> res;
    res.insert(res.end(), former.begin(), former.end());
    res.push_back(newI);
    res.insert(res.end(), A.begin() + i, A.end());
    return res;
}
```

2021.5.18 by Yong