# [Minimum Time to Make Rope Colorful](https://leetcode.com/problems/minimum-time-to-make-rope-colorful/)

`A[i]`表示气球颜色，用小写字母表示。`T[i]`表示去掉第`i`个气球所需的时间。

求至少需要多少时间才能使一排气球两两不同色。

## 举个栗子🌰
```java
"aaa" [4,2,3] -> 5 //去掉第2个和第3个a，花费2+3=5的时间
"aabcc" [3,4,1,5,2] -> 5 //去掉第1个a和最后一个c，花费3+2=5的时间
```

## 思路

如果气球和前一个气球同色，一定要去掉其中一个气球。为了判断要去掉哪一个，遍历的同时记录同色系列的气球哪个所需时间最大，先把所需时间小于最大时间的气球去掉。

如果气球和前一个气球不同色，重置最大时间，因为这个最大时间是相对于同色气球而言的。

## 题解

```js
//js,101ms,91%/51.6MB,99%
var minCost = function(A, T) {
    let res = 0, maxCost = 0 //maxCost: 去掉连续同色气球中的某一个所需的最大时间
    for (let i = 0; i < A.length; i++) {
        if (A[i] != A[i - 1]) maxCost = 0 //如果和前一个气球不同色，重置maxCost
        res += Math.min(maxCost, T[i]) //如果和前一个气球不同色，res+0。如果同色，必须要去掉这个或者前一个气球，选花费时间较少的
        maxCost = Math.max(maxCost, T[i]) //必须在更新res后再更新maxCost，因为maxCost范围不包括当前位置
    }
    return res
};
```

```cpp
//cpp,124ms,99%/95.5MB,19%
int minCost(string A, vector<int>& T) {
    int res = 0, maxCost = T[0];
    for (int i = 1; i < A.length(); i++) {
        if (A[i] == A[i - 1]) res += min(maxCost, T[i]); //同色，res加上去掉花费时间较少的气球所需的时间
        else maxCost = 0; //不同色，重置maxCost
        maxCost = max(maxCost, T[i]);
    }
    return res;
}
```

2022.10.3 by Yong