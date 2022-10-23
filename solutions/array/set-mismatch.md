# [Set Mismatch](https://leetcode.com/problems/set-mismatch/)

由1到n组成的数组中刚好有一个数字重复、一个数字缺失，求重复和缺失的数字。

## 举个栗子🌰
```java
[1,1,2] -> [1,3]
[1,4,3,3] -> [3,2]
[4,2,1,5,2] -> [2,3]
```

## 思路

用哈希表做，O(n)空间，没什么技术含量。

利用数字只有1到n这个特点，可以把空间复杂度优化到O(1)。

用数字本身作为下标，把它指向的数字标记为负数，以此找出重复和缺失的数字。

![p645](/pictures/p645.jpg)

## 题解

```cpp
//cpp
vector<int> findErrorNums(vector<int>& nums) {
    int rep, los;
    for (int x : nums) {
        if (nums[abs(x) - 1] < 0) rep = abs(x); //位置被标记过，指向这个位置的数字就是重复数字
        else nums[abs(x) - 1] *= -1; //把当前位置的数字标记为负数以便检测
    }
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] > 0) { los = i + 1; break; } //位置没被标记过，说明指向这个位置的数字缺失了
    }
    return {rep, los};
}
```

2022.10.23 by Yong