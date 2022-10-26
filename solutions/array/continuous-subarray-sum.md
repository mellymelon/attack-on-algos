# [Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

判断数组是否存在长度至少为2的连续子数组，其数字总和是`k`的倍数。

## 举个栗子🌰
```java
[1,2,0,8] 5 -> true
[5,0,0,0] 3 -> true
```

## 思路1

![p523_1](/pictures/p523_1.jpg)

遍历数组的同时累加数字并记录位置，如果之前出现过当前累加和%`k`，说明两个位置之间存在`k`的倍数。

因为要求子数组长度至少为2，所以要用哈希表记录位置并以此判断长度够不够。

## 题解

```java
//java
public boolean checkSubarraySum(int[] nums, int k) {
    Map<Integer, Integer> mp = new HashMap<>(); //记录累加和及位置
    mp.put(0, -1); //sum为0，位置在-1
    int curSum = 0;
    for (int i = 0; i < nums.length; i++) {
        curSum += nums[i];
        curSum %= k; //先%下面就少写些%
        if (mp.containsKey(curSum)) {
            if (i - mp.get(curSum) > 1) return true; //之前出现过curSum%k，且距离不小于2
        } else mp.put(curSum, i); //key不存在才put，否则子数组长度不小于2这个条件可能因为连续出现相同的curSum而满足不了
    }
    return false;
}
```

```go
//go
func checkSubarraySum(nums []int, k int) bool {
    mp := map[int]int{0: -1}
    curSum := 0
    for i, x := range nums {
        curSum = (curSum + x) % k
        prev, seen := mp[curSum]
        if seen {
            if i - prev > 1 { return true }
        } else { mp[curSum] = i }
    }
    return false
}
```

## 思路2

![p523_2](/pictures/p523_2.jpg)

用数组`accSum`记录每个位置的累加和，如果是`k`的倍数直接返回`true`，否则在累加和大于`k`且数组长度至少为2的情况下扩大窗口，

逐步减掉之前的累加和直到`k`的倍数出现或者最后返回`false`。

## 题解

```java
//java
public boolean checkSubarraySum(int[] nums, int k) {
    int N = nums.length;
    int accSum[] = new int[N];
    accSum[0] = nums[0];
    for (int i = 1; i < N; i++) {
        accSum[i] = accSum[i - 1] + nums[i];
        if (accSum[i] % k == 0 || nums[i] == 0 && nums[i - 1] == 0) return true; //当前累加和是k的倍数或者等于0，或者连续两个0
        if (accSum[i] > k) { //当前累加和大于k才往前逐个排查
            int j = i - 2; //从前2个开始往前逐个检查，满足长度至少为2的要求
            while (j >= 0) { //下标要在数组范围内
                if ((accSum[i] - accSum[j]) % k == 0) return true; //减掉前面的累加和就是当前子数组累加和，若是k的倍数就返回true
                j--;
            }
        }
    }
    return false;
}
```

```go
//go
func checkSubarraySum(nums []int, k int) bool {
    accSum := make([]int, len(nums))
    accSum[0] = nums[0]
    for i := 1; i < len(nums); i++ {
        accSum[i] = nums[i] + accSum[i - 1]
        if accSum[i] % k == 0 || nums[i] == 0 && nums[i - 1] == 0 {
            return true
        }
        if accSum[i] > k {
            j := i - 2
            for j >= 0 {
                if (accSum[i] - accSum[j]) % k == 0 { return true }
                j--
            }
        }
    }
    return false
}
```

2021.7.8 by Yong