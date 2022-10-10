# [Break a Palindrome](https://leetcode.com/problems/break-a-palindrome/)

修改回文中的一个字符使其无法构成回文，要求结果的字典顺序最小。

**备注**

- 给定字符串是回文，只由小写字母组成

## 举个栗子🌰
```java
"a" -> ""
"aba" -> "abb"
"bbbb" -> "abbb"
```

## 思路

因为给定字符串是回文，要使结果字典顺序最小，只需要把第一个不是`a`的字母改成`a`即可。

如果全是`a`，把最后一个字母改成`b`就能满足最小字典顺序。

## 题解

```java
//java,0ms,100%/36.8MB,90%
public String breakPalindrome(String s) {
    int N = s.length();
    if (N < 2) return "";
    char[] res = s.toCharArray();
    for (int i = 0; i < N / 2; i++) { //因为回文是对称的，破坏前半部分的某个字母就可以破坏整个回文了，所以只遍历前半部分
        if (s.charAt(i) != 'a') { //遇到不是字母a的字母就改成a
            res[i] = 'a';
            return String.valueOf(res);
        }
    }
    res[N - 1] = 'b'; //字符串全是a，最后一个字母改成b
    return String.valueOf(res);
}
```

```js
//js,69ms,87%/42.2MB,22%
var breakPalindrome = function(s) {
    const N = s.length
    if (N < 2) return ""
    let res = s.split('')
    for (let i = 0; i < N >> 1; i++) {
        if (res[i] != 'a') {
            res[i] = 'a'
            return res.join('')
        }
    }
    res[N - 1] = 'b'
    return res.join('')
};
```

```py
#py,28ms,80%/14.1MB,72%
def breakPalindrome(self, s):
    if len(s) < 2: return ""
    res = list(s)
    for i in range(len(res) // 2):
        if res[i] != 'a':
            res[i] = 'a'
            return "".join(res)
    res[-1] = 'b'
    return "".join(res)
```

```cpp
//cpp,0ms,100%/6.1MB,95%
string breakPalindrome(string s) {
    int N = s.size();
    if (N < 2) return "";
    for (int i = 0; i < N / 2; i++) {
        if (s[i] != 'a') {
            s[i] = 'a';
            return s;
        }
    }
    s[N - 1] = 'b';
    return s;
}
```

2021.6.22 by Yong