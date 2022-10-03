# [Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements/)
求数组中与x差值最小的k个数字（包括x本身）。

**备注**

- 数组已排序

## 举个栗子🌰
```java
[1,2,3,4,5] k=2 x=3 -> [2,3] //相同差值保留较小数字，不取4
[-4,-3,-2,-1] k=3 x=1 -> [-3,-2,-1]
[2,3,7,8,9] k=2 x=6 -> [7,8]
```

## 思路

因为数组已排序，考虑用二分法搜索包含目标数字的窗口的起始位置。只有3种情况，参考下图:

![p658](/pictures/p658.jpg)

## 题解

```java
//java,3ms,97%/40.7MB,59%
public List<Integer> findClosestElements(int[] A, int k, int x) {
    int L = 0, R = A.length - k; //包含k个目标数字的窗口最左不会大于arr.length-k，否则无法构成k个数字
    while (L < R) { //不是<=。如果有=，M+k会越界
        int M = L + R >> 1;
        if (x - A[M] > A[M + k] - x) L = M + 1; //情况1
        else R = M; //<=，即情况2或3。此时R调整为M而不是M-1，因为M仍有可能是起始位置。=的情况下移动R可以满足相同差值下保留较小数字的要求
    }
    List<Integer> res = new ArrayList<>();
    for (int i = L; i < L + k; i++) { //取从起始位置L开始的k个数字
        res.add(A[i]);
    }
    return res;
}
```

```cpp
//cpp,32ms,91%/31.6MB,69%
vector<int> findClosestElements(vector<int>& A, int k, int x) {
    int L = 0, R = A.size() - k;
    while (L < R) {
        int M = L + R >> 1;
        if (A[M + k] - x < x - A[M]) L = M + 1;
        else R = M;
    }
    vector<int> res;
    for (int i = L; i < L + k; i++) {
        res.push_back(A[i]);
    }
    return res;
}
```

```go
//go,36ms,98%/6.9MB,94%
func findClosestElements(A []int, k int, x int) []int {
    L, R := 0, len(A) - k
    for L < R {
        M := (L + R) >> 1
        if x - A[M] > A[M + k] - x {
            L = M + 1
        } else { R = M }
    }
    return A[L : L + k]
}
```

2021.7.2