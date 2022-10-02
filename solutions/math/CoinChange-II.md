# [Coin Change II](https://leetcode.com/problems/coin-change-ii/)

给定不同面值的硬币，求用这些硬币构成目标数额有多少种组合。

## 举个栗子🌰
```java
4 [1,2,5] -> 3 //1+1+1=1, 2+2, 1+1+2。可以不用5
11 [2,5,10] -> 1 //2+2+2+5
10 [2,5,10] -> 3 //2+2+2+2+2, 5+5, 10
```

## 思路

记录从0到目标数额为止的每个数额的硬币组合数，如下图：

![p518.jpg](/pictures/p518.jpg)

## 题解

Time: `O(mn)`

Space: `O(mn)`

```java
//java,9ms,25%/45.8MB,29%
public int change(int amount, int[] coins) {
    int m = coins.length, n = amount;
    int[][] dp = new int[m + 1][n + 1];
    for (int i = 0; i <= m; i++) dp[i][0] = 1; //第一列全部填1
    for (int i = 1; i <= m; i++) { //第一行除了第一个位置外固定全是0，所以i从1开始
        for (int j = 1; j <= n; j++) { //第一列固定全是1，所以j从1开始。这里j表示当前目标数额
            dp[i][j] = dp[i - 1][j]; //先基于上一行的dp结果，因为求的是所有组合数，再根据情况加上下面if的组合数
            if (j - coins[i - 1] >= 0) dp[i][j] += dp[i][j - coins[i - 1]]; //如果硬币面值比当前目标小就能参与构建当前目标，加上没有这个硬币之前记录的组合数。i-1是因为i从1开始，-1才能取到当前coins里对应的位置
        }
    }
    return dp[m][n];
}
```

```js
//js
var change = function(amount, coins) {
    let m = coins.length, n = amount
    let dp = [...Array(m + 1)].map(_ => Array(n + 1).fill(0))
    for (let i = 0; i <= m; i++) dp[i][0] = 1
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            dp[i][j] = dp[i - 1][j]
            if (j - coins[i - 1] >= 0) dp[i][j] += dp[i][j - coins[i - 1]]
        }
    }
    return dp[m][n]
};
```

```py
#py
def change(self, amount: int, coins: List[int]) -> int:
    m, n = len(coins), amount
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(m + 1): dp[i][0] = 1
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            dp[i][j] = dp[i - 1][j]
            if j - coins[i - 1] >= 0:
                dp[i][j] += dp[i][j - coins[i - 1]]

    return dp[m][n]
```

---

因为dp当前行只基于上一行的结果，所以空间复杂度可以进一步优化，dp数组由二维变成一维。

Time: `O(mn)`

Space: `O(n)`

```java
//java,2ms,100%/36.5MB,71%
public int change(int amount, int[] coins) {
    int[] dp = new int[amount + 1];
    dp[0] = 1; //不用硬币构建数额0，算作1种方法。不填这个1后面无法dp，因为后面的dp也是基于这个1
    for (int coin : coins) //定好一种硬币再遍历1到amount各个数额，即一行一行地dp
        for (int v = coin; v <= amount; v++) //v: 当前目标数额。v从coin开始就不用像二维dp那样用if判断
            dp[v] += dp[v - coin];

    return dp[amount];
}
```

```js
//js
var change = function(amount, coins) {
    let n = amount, dp = Array(n + 1).fill(0)
    dp[0] = 1
    for (let coin of coins)
        for (let v = coin; v <= n; v++)
            dp[v] += dp[v - coin]

    return dp[n]
};
```

```py
#py
def change(self, amount, coins):
    m, n = len(coins), amount
    dp = [0] * (n + 1)
    dp[0] = 1
    for i in range(1, m + 1):
        for j in range(coins[i - 1], n + 1):
            dp[j] += dp[j - coins[i - 1]]
            
    return dp[n]
```

2020.6.7 by Yong