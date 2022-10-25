# [Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)

给定只包含`(`或`)`的字符串，求最长有效子串的长度。

## 举个栗子🌰
```java
"(()" -> 2
"((()))" -> 6
"))(())" -> 4
```

## 思路1

如果`(`和`)`个数相同就是一个有效子串，此时按需更新最长子串长度。

顺序遍历情况下，如果发现`)`个数比`(`多，就不是有效子串，此时需要重置`(`和`)`的计数。

因为单凭顺序遍历不能检测到`(`个数比`)`多的无效子串，所以还要以同样的逻辑逆序遍历。

## 题解

```java
//java
public int longestValidParentheses(String s) {
    int res = 0, l = 0, r = 0;
    for (int i = 0; i < s.length(); i++) { //顺序遍历
        if (s.charAt(i) == '(') l++;
        else r++;
        if (l == r) res = Math.max(res, r * 2); //l * 2也可以
        else if (l < r) { l = 0; r = 0; } //无效子串，重置l和r计数
    }
    l = 0; r = 0; //注意重置l和r
    for (int i = s.length() - 1; i >= 0; i--) { //逆序遍历
        if (s.charAt(i) == '(') l++;
        else r++;
        if (l == r) res = Math.max(res, l * 2);
        else if (l > r) { l = 0; r = 0; }
    }
    return res;
}
```

```py
#py
def longestValidParentheses(self, s):
    res = l = r = 0
    for ch in s:
        if ch == '(': l += 1
        else: r += 1
        if l == r: res = max(res, r * 2)
        elif r > l: l = r = 0
    l = r = 0
    for ch in s[::-1]:
        if ch == '(': l += 1
        else: r += 1
        if l == r: res = max(res, l * 2)
        elif l > r: l = r = 0
    return res
```

## 思路2

有效括号子串通常可以用栈来检查，不过这回栈记录的是位置，因为需要计算有效子串的长度。

## 题解

```java
//java
public int longestValidParentheses(String s) {
    int res = 0;
    Stack<Integer> stk = new Stack<>();
    stk.push(-1); //记录有效子串的前一个位置，作用类似anchor
    for (int i = 0; i < s.length(); i++) {
        if (s.charAt(i) == '(') stk.push(i);
        else { //ch == ')'
            stk.pop(); //把上个'('的位置pop掉，与上上个位置呼应以计算长度。栈至少也有-1保底
            if (stk.isEmpty()) stk.push(i); //如果-1也没了就用当前位置作为anchor
            else res = Math.max(res, i - stk.peek()); //用当前位置到anchor的距离按需更新答案
        }
    }
    return res;
}
```

```go
//go
func longestValidParentheses(s string) int {
    res, stk := 0, []int{-1}
    for i, ch := range s {
        if ch == '(' {
            stk = append(stk, i)
        } else {
            stk = stk[:len(stk) - 1]
            if len(stk) == 0 {
                stk = append(stk, i)
            } else {
                res = max(res, i - stk[len(stk) - 1])
            }
        }
    }
    return res
}
```

2021.4.4 by Yong