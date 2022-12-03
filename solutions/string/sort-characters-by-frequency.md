# [Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

按字符个数从多到少构造字符串。

**备注**

只有大小写字母和数字。

## 举个栗子🌰
```java
"122333Abb" -> "33322bb1A"
"xxxooo" -> "xxxooo" //也可以是"oooxxx"
```

## 思路

先统计各字符个数再按个数构造字符串。

## 题解

```java
//java
public String frequencySort(String s) {
    int N = s.length(), occ[] = new int[75]; //最多到z对应的122，而122-48=74，即最多用到75个位置(0~74)
    for (int i = 0; i < N; i++)
        occ[s.charAt(i) - '0']++; //统计各字符个数
    char[] chars = new char[N];
    int i = 0;
    while (i < N) { //往chars填答案
        char curCh = '0'; int curMax = 0; //这里curCh初始化成什么字符都可以
        for (char ch = '0'; ch <= 'z'; ch++) { //找出现最多的字符及其个数
            if (occ[ch - '0'] > curMax) {
                curCh = ch; curMax = occ[ch - '0'];
            }
        }
        while (occ[curCh - '0']-- > 0) chars[i++] = curCh; //同一字符有几个就填几个，边填边更新occ和当前位置
    }
    return String.valueOf(chars);
}
```

```js
//js
var frequencySort = function(s) {
    let occ = Array(75).fill(0)
    for (ch of s) occ[ch.charCodeAt(0) - 48]++
    let N = s.length, res = "", i = 0
    while (i < N) {
        let j = occ.indexOf(Math.max(...occ))
        while (occ[j]-- > 0) {
            res += String.fromCharCode(j + 48)
            i++
        }
    }
    return res
};
```

```go
//go
func frequencySort(s string) string {
    var occ [75]int
    for _, ch := range(s) {
        occ[ch - '0']++
    }
    i, res := 0, strings.Builder{}
    for i < len(s) {
        curCh, curMax := 0, 0
        for ch, cnt := range occ {
            if cnt > curMax {
                curMax = cnt
                curCh = ch + '0'
            }
        }
        for n := curMax; n > 0; n-- {
            res.WriteString(string(curCh))
            i++
            occ[curCh - '0']--
        }
    }
    return res.String()
}
```

```py
#py
def frequencySort(self, s):
    return "".join(ch * cnt for ch, cnt in Counter(s).most_common())
```

2020.5.22 by Yong