# [Earliest Possible Day of Full Bloom](https://leetcode.com/problems/earliest-possible-day-of-full-bloom/)

给定`P`表示每种花的栽培时间，`G`表示花的成长时间，求种完所有花直到全部开花至少需要多少天。

**备注**

- 同一天只能栽培一种花

## 举个栗子🌰
```java
[3,2,2,1] [1,2,1,5] -> 9
```

## 思路

优先种成长时间较长的花，利用它成长期间这段时间再种别的花以减少总时间。

![p2136](/pictures/p2136.jpg)

## 题解

```js
//js,293ms,100%/75.2MB,95%
var earliestFullBloom = function(P, G) {
    const order = [...Array(P.length).keys()] //栽种顺序，表示先种第i种花
    order.sort((a, b) => G[b] - G[a]) //按成长时间从大到小排序
    let cur = 0, res = 0 //cur: 当前已消耗的种花时间
    for (const i of order) {
        res = Math.max(res, cur + P[i] + G[i]) //如果种完当前花并开花后总的时间在前一种花的成长期内，则总的时间不会变多，反之同理
        cur += P[i]
    }
    return res
};
```

```go
//go,228ms,100%/10.4MB,87%
func earliestFullBloom(P []int, G []int) int {
    var order []int
    for i := 0; i < len(P); i++ {
        order = append(order, i)
    }
    sort.Slice(order, func(i, j int) bool {
        return G[order[i]] > G[order[j]]
    })
    cur, res := 0, 0
    for _, i := range order {
        res = max(res, cur + P[i] + G[i])
        cur += P[i]
    }
    return res
}

func max(a, b int) int {
    if a > b { return a }
    return b
}
```

2022.10.29 by Yong