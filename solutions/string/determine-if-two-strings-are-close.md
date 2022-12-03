# [Determine if Two Strings Are Close](https://leetcode.com/problems/determine-if-two-strings-are-close/)

对于两个字符串，允许以下两个操作无限次；如果能通过操作使两个字符串相同，就认为它们接近。

操作1： 交换同一字符串中任意两个字母的位置。

操作2： 把所有同一字母变成字符串中存在的另一个字母。

## 举个栗子🌰
```java
"aaccc" "aaacc" -> true
"aaabbbbccddeeeeefffff" "aaaaabbcccdddeeeeffff" -> false
```

## 思路

因为允许无限次操作，满足“接近”的条件只有两个：

1. 两个字符串的字母集合相同
2. 两个字符串的字母个数统计组合相同

![p1657](/pictures/p1657.jpg)

## 题解

```java
//java
public boolean closeStrings(String w1, String w2) {
    if (w1.length() != w2.length()) return false;
    int[] cnt1 = new int[26], cnt2 = new int[26]; //记录每个字母出现多少次
    int bmp1 = 0, bmp2 = 0; //记录有多少不同的字母
    for (int i = 0; i < w1.length(); i++) {
        char ch1 = w1.charAt(i), ch2 = w2.charAt(i);
        cnt1[ch1 - 'a']++;
        cnt2[ch2 - 'a']++;
        bmp1 |= 1 << ch1;
        bmp2 |= 1 << ch2;
    }
    Arrays.sort(cnt1); //因为只有26个字母，可以认为这里的排序是O(1)时间
    Arrays.sort(cnt2);
    return Arrays.equals(cnt1, cnt2) && bmp1 == bmp2;
}
```

```cpp
//cpp
bool closeStrings(string w1, string w2) {
    if (w1.size() != w2.size()) return false;
    vector<int> cnt1(26), cnt2(26);
    bitset<26> bset1, bset2;
    for (int i = 0; i < w1.size(); i++) {
        cnt1[w1[i] - 'a']++, cnt2[w2[i] - 'a']++;
        bset1.set(w1[i] - 'a'), bset2.set(w2[i] - 'a');
    }
    sort(cnt1.begin(), cnt1.end());
    sort(cnt2.begin(), cnt2.end());
    return cnt1 == cnt2 && bset1 == bset2;
}
```

```py
#py
def closeStrings(self, w1, w2):
    return set(w1) == set(w2) and sorted(Counter(w1).values()) == sorted(Counter(w2).values())
```

2022.12.3 by Yong