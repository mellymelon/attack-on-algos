# [3Sum Closest](https://leetcode.com/problems/3sum-closest/submissions/)

求最接近`target`的三个不同位置的数字的总和，可以近到差为0。

**备注**

- 数组长度至少为3
- nums[i] 范围 [-1000, 1000]
- target 范围 [-10000, 10000]

## 举个栗子🌰
```java
[1,2,3,-1,-2,-3] 0 -> 0 //离目标0最近的三个数的和就是0，即1+2+(-3)=0
[-2,2,1,5,-3,3] 4 -> 4
```

## 思路

![p16.jpg](/pictures/p16.jpg)

先排序，方便定好一个数字后根据当前总和大小调整另外两个数字，在这个过程中动态更新与`target`的最小差值`minDiff`。

由`target - sum = minDiff`移项可得`sum = target - minDiff`，只要求出`minDiff`就能算出所求`sum`，注意记录`minDiff`时要保留符号（正负）。

## 题解

```java
//java
public int threeSumClosest(int[] nums, int target) {
    Arrays.sort(nums);
    int N = nums.length, minDiff = 13000; //依题意，diff最大是10000-(-3000)=13000
    for (int i = 0; i < N - 2; i++) { //N-2，因为要有3个数字
        if (i == 0 || nums[i] != nums[i - 1]) { //去掉这行也可以，只是重复数字比较多时有优化效果
            int j = i + 1, k = N - 1;
            while (j < k) {
                int curSum = nums[i] + nums[j] + nums[k];
                if (Math.abs(target - curSum) < Math.abs(minDiff)) {
                    minDiff = target - curSum; //注意minDiff不能有abs，最后计算3sum原值需要minDiff的正负符号。注意是target-curSum，不是curSum-target，要对应return的表达式
                    if (minDiff == 0) return curSum; //返回target也可以
                }
                if (curSum < target) j++;
                else k--;
            }
        }
    }
    return target - minDiff;
}
```

```py
#py
def threeSumClosest(self, nums: List[int], target: int) -> int:
    nums.sort()
    N, minDiff = len(nums), 13000
    for i in range(N - 2):
        if i == 0 or nums[i] != nums[i - 1]:
            j, k = i + 1, N - 1
            while j < k:
                curSum = nums[i] + nums[j] + nums[k]
                if abs(target - curSum) < abs(minDiff):
                    minDiff = target - curSum
                    if minDiff == 0: return target
                if curSum < target: j += 1
                else: k -= 1
    return target - minDiff
```

```cpp
//cpp
int threeSumClosest(vector<int>& nums, int target) {
    sort(nums.begin(), nums.end());
    int N = nums.size(), minDiff = 13000;
    for (int i = 0; i < N - 2; i++) {
        if (i == 0 || nums[i] != nums[i - 1]) {
            int j = i + 1, k = N - 1;
            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];
                if (abs(target - sum) < abs(minDiff)) {
                    minDiff = target - sum;
                    if (minDiff == 0) return target;
                }
                if (sum < target) j++;
                else k--;
            }
        }
    }
    return target - minDiff;
}
```

```go
//go
func threeSumClosest(nums []int, target int) int {
    sort.Ints(nums)
    N, minDiff := len(nums), 13000
    for i := 0; i < N - 2; i++ {
        if i == 0 || nums[i] != nums[i - 1] {
            j, k := i + 1, N - 1
            for j < k {
                curSum := nums[i] + nums[j] + nums[k]
                diff := target - curSum
                if abs(diff) < abs(minDiff) {
                    minDiff = diff
                    if minDiff == 0 { return curSum }
                }
                if curSum < target {
                    j++
                } else { k-- }
            }
        }
    }
    return target - minDiff
}

func abs(x int) int {
    if x < 0 { return -x }
    return x
}
```

2021.6.3 by Yong