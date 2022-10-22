# [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

求`s`里由所有`t`字母组成的最短连续子串。

**备注**

- 测试用例的答案是唯一的

## 举个栗子🌰
```java
"aacFadaa" "aaa" -> "adaa" //t的一个字母只能用一次
"dooor" "dog" -> ""
"dooog" "dog" -> "dooog"
"aZdbgoAhGd" "dog" -> "dbgo"
"dddcoodg" "dog" -> "odg"
```

## 思路

有空再补 ╮(╯▽╰)╭

## 题解
```cpp
//cpp,11ms,92%/7.6MB,84%
string minWindow(string s, string t) {
    int count[58]{};
    for (char ch : t) count[ch - 65]++; //统计t各字母个数

    int matches = 0, anchor = 0, resBeg = 0, resLen = s.size() + 1;
    for (int i = 0; i < s.size(); i++) {
        if (count[s[i] - 65]-- > 0) matches++; //遇到匹配t字母的s字母。即使遇到无关字母也减计数，反正计数>0才加matches
        while (matches == t.size()) {
            if (i - anchor + 1 < resLen) {
                resBeg = anchor;
                resLen = i - anchor + 1;
            }
            if (count[s[anchor++] - 65]++ == 0) matches--; //收缩窗口，排除窗口左边的字母并恢复它的统计个数。这里只有t字母会使matches--，因为无关字母在++前计数不会为0
        }
    }
    if (resLen > s.size()) return ""; //相当于resLen==s.size()+1
    return s.substr(resBeg, resLen);
}
```

2022.10.22 by Yong