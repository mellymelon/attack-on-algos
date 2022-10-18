# [Count and Say](https://leetcode.com/problems/count-and-say/)

求第n个count-and-say序列。

## 举个栗子🌰
```java
1 -> "1"
2 -> "11"
4 -> "1211"
```

## 思路

count-and-say序列形如`["1", "11", "21", "1211", "111221", "312211", "13112221", ...]`。

要求第n个序列，必须要知道前一个序列的结构，可以通过递归从第一个序列开始逐步构造到第n个序列。

## 题解

```java
//java
public String countAndSay(int n) {
    if (n == 1) return "1";
    String prev = countAndSay(n - 1);
    StringBuilder res = new StringBuilder();
    int i = 0;
    while (i < prev.length()) {
        int count = 0;
        char say = prev.charAt(i);
        while (i < prev.length() && prev.charAt(i) == say) {
            count++;
            i++;
        }
        res.append(count);
        res.append(say);
    }
    return res.toString();
}
```

```go
//go
func countAndSay(n int) string {
    if n == 1 { return "1" }
    res, i := strings.Builder{}, 0
    prev := countAndSay(n - 1) //递归求序列的前一个序列，再根据前一个序列构造当前序列
    for i < len(prev) {
        count, say := 0, prev[i]
        for i < len(prev) && prev[i] == say { //数有几个连续的重复数字
            count++
            i++
        }
        res.WriteString(strconv.Itoa(count))
        res.WriteString(string(say))
    }
    return res.String()
}
```

2022.10.18 by Yong