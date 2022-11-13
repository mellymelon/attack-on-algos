# [Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

反转字符串中的单词顺序。

**备注**

- 字符串中至少有一个单词

## 举个栗子🌰
```java
"  aa628 " -> "aa628"
"012   345  67" -> "67 345 012"
"  ab   cd  " -> "cd ab"
```

## 思路

![p151](/pictures/p151.jpg)

## 题解

```java
//java
public String reverseWords(String s) {
    int N = s.length();
    char[] str = s.toCharArray();
    reverse(str, 0, N - 1); //先反转整个字符串
    int L = 0, R = 0;
    while (L < N) { //再逐个单词反转回来
        while (R < N && str[R] != ' ') R++; //遇到字母才移动R
        reverse(str, L, R - 1); //反转当前单词
        L = R + 1; //跳到R指着的空格后面
        R++; //现在R指着空格，要移动
    }
    return removeSpaces(str);
}

private void reverse(char[] str, int L, int R) {
    while (L < R) {
        char tmp = str[L];
        str[L++] = str[R];
        str[R--] = tmp;
    }
}

private String removeSpaces(char[] str) { //去掉前后缀和单词之间的空格
    int N = str.length, L = 0, R = 0;
    while (R < N) {
        while (str[R] == ' ') R++; //跳过前缀空格。因为至少有一个单词，所以不用R<N检查越界
        while (R < N && str[R] != ' ') str[L++] = str[R++]; //把字符填到数组前面
        while (R < N && str[R] == ' ') R++; //跳过单词后的空格
        if (R < N) str[L++] = ' '; //每个单词后加一个空格
    }
    return String.valueOf(str).substring(0, L); //只需要保留L前面的部分
}
```

```js
//js
var reverseWords = function(s) {
    return s.trim().replace(/\s+/g, ' ').split(' ').reverse().join(' ') //把多个空格换成单个空格以便按空格切分反转
};
```

```py
#py
def reverseWords(self, s):
    return ' '.join(s.split()[::-1])
```

```cpp
//cpp
string reverseWords(string s) {
    reverse(s.begin(), s.end()); //先反转整个字符串
    int N = s.size(), L = 0, R = 0;
    while (R < N) { //再逐个单词反转回来
        while (R < N && s[R] != ' ') R++;
        reverse(s.begin() + L, s.begin() + R);
        L = ++R;
    }
    L = 0, R = 0;
    while (R < N) { //清掉前后缀和单词之间的空格
        while (s[R] == ' ') R++;
        while (R < N && s[R] != ' ') s[L++] = s[R++];
        while (R < N && s[R] == ' ') R++;
        if (R < N) s[L++] = ' ';
    }
    return s.substr(0, L);
}
```

```go
//go
func reverseWords(s string) string {
    anchor, res := 0, ""
    for i := 0; i < len(s); {
        for i < len(s) && s[i] == ' ' { i++ } //跳过前缀空格
        if i == len(s) { break }
        anchor = i
        for i < len(s) && s[i] != ' ' { i++ } //扫过字母
        res = s[anchor:i] + " " + res //拼到答案前面起到反转效果
    }
    return res[:len(res)-1] //排除多加的空格即可
}
```

2020.7.15 by Yong