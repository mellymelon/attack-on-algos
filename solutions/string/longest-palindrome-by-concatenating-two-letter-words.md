# [Longest Palindrome by Concatenating Two Letter Words](https://leetcode.com/problems/longest-palindrome-by-concatenating-two-letter-words/)

给定一些长度为2的字符串，求用这些字符串组成最长回文的长度。

## 举个栗子🌰
```java
["ab","cd","bd","hh"] -> 2
["aa","bc","cc","cb"] -> 6
["ak","bb","ak","ka","hh","ka"] -> 10
```

## 思路

因为字符串长度都为2，只有两种情况，字符串形如aa或者ab。

aa只能单独或者和同样的aa作为中心，ab只能和它的镜像ba对称放在回文的两侧。

## 题解

```java
//java
public int longestPalindrome(String[] words) {
    int pairs = 0, mid = 0;
    Map<String, Integer> count = new HashMap<>();
    for (String w : words) {
        if (w.charAt(0) == w.charAt(1)) {
            if (count.containsKey(w)) {
                pairs += 2;
                mid--;
                count.remove(w);
            } else {
                mid++;
                count.put(w, 1);
            }
        } else {
            String mirr = new String();
            mirr = mirr + w.charAt(1) + w.charAt(0);
            if (count.containsKey(mirr) && count.get(mirr) > 0) {
                pairs += 2;
                count.put(mirr, count.get(mirr) - 1);
            } else count.put(w, count.getOrDefault(w, 0) + 1);
        }
    }
    if (mid > 0) pairs++;
    return pairs * 2;
}
```

```cpp
//cpp
int longestPalindrome(vector<string>& words) {
    int pairs = 0, mid = 0; //mid： 统计形如aa的可以作为回文中心的字符串的个数
    unordered_map<string, int> count;
    for (string w : words) {
        if (w[0] == w[1]) {
            if (count[w]) {
                pairs += 2; //比如aaaa，有2对，它们组成回文中心
                mid--;
                count[w]--;
            } else mid++, count[w]++;
        } else {
            string mirr = w;
            reverse(w.begin(), w.end());
            if (count[mirr]) {
                pairs += 2; //比如abba，有2对，它们在回文的两侧
                count[mirr]--;
            } else count[w]++;
        }
    }
    return ((mid > 0) + pairs) * 2; //如果有多余的aa可用作中心，只取一个作中心。最后乘以2取结果长度
}
```

2022.11.3 by Yong