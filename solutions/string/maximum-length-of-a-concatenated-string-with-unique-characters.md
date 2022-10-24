# [Maximum Length of a Concatenated String with Unique Characters](https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/)

用给定的一些字符串构造最长的无重复字母的序列，并求其长度。

## 举个栗子🌰
```java
["aa","ac","bd"] -> 4 //acbd
["abcde","de","bc","af"] -> 6 //debcaf
```

## 思路

遍历所有单词组合，跳过和当前组合里面的字母有重复冲突的单词，在此过程中记录最长序列长度。

## 题解

```py
#py
def maxLength(self, arr):
    def hasDups(s):
        return len(set(s)) < len(s)

    def dfs(beg, prev):
        for i in range(beg, len(arr)):
            if hasDups(arr[i]): continue
            cand = prev + arr[i]
            if not hasDups(cand):
                self.res = max(self.res, len(cand))
                dfs(i + 1, cand)
    
    self.res = 0
    dfs(0, '')
    return self.res
```

```cpp
//cpp
public:
int maxLength(vector<string>& arr) {
    dfs(0, bitset<26>(), arr);
    return res;
}

private:
int res = 0;

void dfs(int beg, bitset<26> prev, vector<string>& arr) {
    for (int i = beg; i < arr.size(); i++) {
        bitset<26> cur;
        for (char ch : arr[i]) cur.set(ch - 'a'); //用26位bit记录这个词有哪些字母
        if (cur.count() < arr[i].size()) continue; //这个词本身有重复字母，跳过
        if ((cur & prev).any()) continue; //和之前拼好的序列有冲突，跳过
        cur |= prev; //和之前拼好的序列合并
        res = max(res, (int) cur.count());
        dfs(i + 1, cur, arr);
    }
}
```

```go
//go
func maxLength(arr []string) int {
    res := 0
    var dfs func(beg int, prev string)
    dfs = func(beg int, prev string) {
        for i := beg; i < len(arr); i++ {
            if hasDups(arr[i]) { continue }
            cand := prev + arr[i]
            if !hasDups(cand) {
                if len(cand) > res { res = len(cand) }
                dfs(i + 1, cand)
            }
        }
    }

    dfs(0, "")
    return res
}

func hasDups(s string) bool {
    mask := 0
    for i := 0; i < len(s); i++ {
        curBit := 1 << (s[i] - 97)
        if mask & curBit > 0 { return true }
        mask |= curBit
    }
    return false
}
```


2021.7.21 by Yong