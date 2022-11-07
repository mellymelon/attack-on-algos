# [Maximum 69 Number](https://leetcode.com/problems/maximum-69-number/)

给定只由6或9组成的数字，最多把一个6改成9，或者9改成6，求最大的结果。

**备注**

数字范围：`[1, 10000]`

## 举个栗子🌰
```java
99 -> 99
996 -> 999
9669 -> 9969
```

## 思路

把数字转成字符串或者数组再把第一个6改成9不是不可以，但要O(n)空间，没什么技术含量。

因为数字只由6或9组成，为了使结果最大化只能是把6改成9，如有6则差必为3。

可以边除边模找出6的最高位再乘以3加到原数字上以达到6改成9的效果。

又因为数字不超过10000，按照6的最高位，差可能是3、30、300、3000。

## 题解 1

```js
//js,55ms,99%
var maximum69Number  = function(num) {
    let x = num, i = 0, i6 = -1 //i6记录6的最高位
    while (x > 0) {
        if (x % 10 == 6) i6 = i
        x = ~~(x / 10)
        i++
    }
    if (i6 == -1) return num //没有6，不用改
    return num + 3 * 10 ** i6 //比如9969 = 9669 + 3 * 10^2
};
```

## 题解 2

```go
//go,0ms,100%
func maximum69Number (num int) int {
    if num / 1000 == 6 { return num + 3000 } //第一个数字是6
    if num / 100 % 10 == 6 { return num + 300 } //第二个数字是6
    if num / 10 % 10 == 6 { return num + 30 } //第三个数字是6
    if num % 10 == 6 { return num + 3 } //最后一个数字是6
    return num //没有6，本来就全是9
}
```

2022.11.7 by Yong