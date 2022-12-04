# [Minimum Average Difference](https://leetcode.com/problems/minimum-average-difference/)

求数组里最小均值差的位置，如果有多个，返回最靠前的位置。

第`i`个位置的均值差指前`i+1`个元素的平均值与剩余元素平均值的差的绝对值。

**备注**

数字范围：`[0, 100000]`

## 举个栗子🌰
```java
[2,5,3,9,5,3] -> 3
```

## 思路 1

用数组记录到每个位置为止的累加和，以便取前`i+1`个元素以及剩余元素的总和及均值，然后按需更新最小均值及位置。

## 题解 1

Time: `O(N)`

Space: `O(N)`

```js
//js
var minimumAverageDifference = function(nums) {
    let N = nums.length, curSum = 0
    let prefixSum = Array(N).fill(0) //记录到每个位置为止的累加和
    for (let i = 0; i < N; i++) {
        curSum += nums[i]
        prefixSum[i] = curSum
    }
    let res = 0, minAvg = 100000
    for (let i = 0; i < N; i++) {
        let avg1 = ~~(prefixSum[i] / (i + 1)) //前i+1个元素的均值
        let avg2 = ~~((prefixSum[N - 1] - prefixSum[i]) / (N - i - 1)) //剩余元素的均值
        let curAvg = Math.abs(avg1 - avg2)
        if (curAvg < minAvg) {
            minAvg = curAvg
            res = i
        }
    }
    return res
};
```

```go
//go
func minimumAverageDifference(nums []int) int {
    N, curSum := len(nums), 0
    prefixSum := make([]int, N)
    for i, x := range nums {
        curSum += x
        prefixSum[i] = curSum
    }
    res, minAvg := 0, 100000
    for i := range nums {
        avg1 := prefixSum[i] / (i + 1)
        avg2 := (prefixSum[N - 1] - prefixSum[i]) / max(N - i - 1, 1) //max取1避免分母为0
        curAvg := abs(avg1 - avg2)
        if curAvg < minAvg {
            minAvg, res = curAvg, i
        }
    }
    return res
}

func max(a, b int) int {
    if a > b { return a }
    return b
}

func abs(x int) int {
    if x < 0 { return -x }
    return x
}
```

## 思路 2

反正都要跑两趟，第一趟求全部元素的总和`postSum`，第二趟遍历的同时从0开始累加就能得到前`i+1`个元素的总和及均值，同理累减`postSum`就能得到剩余元素的总和及均值。

## 题解 2

Time: `O(N)`

Space: `O(1)`

```js
//js
var minimumAverageDifference = function(nums) {
    let postSum = nums.reduce((a, b) => a + b)
    let N = nums.length, curSum = 0, minAvg = 100000, res = 0
    nums.forEach((x, i) => {
        curSum += x; postSum -= x
        let avg1 = ~~(curSum / (i + 1))
        let avg2 = ~~(postSum / (N - i - 1))
        let curAvg = Math.abs(avg1 - avg2)
        if (curAvg < minAvg) {
            minAvg = curAvg
            res = i
        }
    })
    return res
};
```

```go
//go
func minimumAverageDifference(nums []int) int {
    N, postSum := len(nums), 0
    for _, x := range nums { postSum += x }
    curSum, minAvg, res := 0, 100000, 0
    for i, x := range nums {
        curSum += x; postSum -= x
        avg1 := curSum / (i + 1)
        avg2 := postSum / max(N - i - 1, 1)
        curAvg := abs(avg1 - avg2)
        if curAvg < minAvg {
            minAvg, res = curAvg, i
        }
    }
    return res
}

func max(a, b int) int {
    if a > b { return a }
    return b
}

func abs(x int) int {
    if x < 0 { return -x }
    return x
}
```

2022.12.4 by Yong