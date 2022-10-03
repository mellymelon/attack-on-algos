# [Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/submissions/)

求至少多大载重的船才能在规定天数内运完所有货。货必须按顺序运。

## 举个栗子🌰
```java
[1,1,1,1,1] 2 -> 3 //第一天运1+1+1，第二天运1+1
```

## 思路

船至少要能载最重那件货，至多能一次载全部货。规定时间内运完全部货的合适载重可以在至少和至多范围内用二分法求出。

## 题解

```cpp
//cpp
int shipWithinDays(vector<int>& weights, int days) {
    int L = 0, R = 0;
    for (int w : weights) {
        L = max(L, w); //载重至少要能载最重那件货
        R += w; //载重最大满足一次载完所有货
    }
    while (L < R) {
        int M = L + R >> 1, cur = 0, need = 1; //cur: 当前打包重量
        for (int w : weights) { //模拟按顺序载货
            cur += w;
            if (cur > M) { //载货量超过M
                need++; //需要多一天时间
                cur = w; //下一天的载货量从w开始计起
            }
        }
        if (need > days) L = M + 1; //如果载重为M时需要的天数超过规定天数，就提高最低载重L
        else R = M; //如果时间还够，就不用那么大的载重，但M仍可能是候选，所以R降低到M
    }
    return L;
}
```

```go
//go
func shipWithinDays(weights []int, days int) int {
    L, R := 0, 0
    for _, w := range weights {
        if w > L { L = w }
        R += w
    }
    for L < R {
        cur, need := 0, 1
        M := (L + R) >> 1
        for _, w := range weights {
            cur += w
            if cur > M {
                need++
                cur = w
            }
        }
        if need > days {
            L = M + 1
        } else { R = M }
    }
    return L
}
```

2021.7.28