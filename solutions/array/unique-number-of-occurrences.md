# [Unique Number of Occurrences](https://leetcode.com/problems/unique-number-of-occurrences/)

判断数组各元素的频数是否唯一。

## 举个栗子🌰
```java
[1,1,1,3,3,3] -> false
[5,5,6,6,6,7] -> true
```

## 思路

先记录各元素频数，再用集合去重。

如果去重后的频数集合长度和元素数相同，说明没有重复频数。

## 题解

```java
//java
public boolean uniqueOccurrences(int[] arr) {
    Map<Integer, Integer> mp = new HashMap<>(); //记录元素频数
    for (int x : arr) mp.put(x, mp.getOrDefault(x, 0) + 1);
    return new HashSet<Integer>(mp.values()).size() == mp.size(); //放进集合去重
}
```

```js
//js
var uniqueOccurrences = function(arr) {
    const mp = new Map()
    arr.forEach(x => mp.set(x, (mp.get(x) || 0) + 1))
    return new Set(mp.values()).size == mp.size
};
```

```go
//go
func uniqueOccurrences(arr []int) bool {
    mp := map[int]int{}
    for _, x := range arr { mp[x]++ }
    uniq := map[int]bool{}
    for _, v := range mp {
        if uniq[v] { return false }
        uniq[v] = true
    }
    return true
}
```

2022.9.25 by Yong