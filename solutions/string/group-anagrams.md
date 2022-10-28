# [Group Anagrams](https://leetcode.com/problems/group-anagrams/)

给变位词分组。

**备注**

- 只有小写字母

## 举个栗子🌰
```java
["aab","aba","abc","cba","abb"] -> [["abb"],["abc","cba"],["aab","aba"]]
```

## 思路 1

给变位词的字母排序，把排序后字母相同的变位词分到一组。

## 题解

```java
//java
public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> mp = new HashMap<>();
    for (String s : strs) {
        char[] chars = s.toCharArray(); //转成数组的目的是给字母排序
        Arrays.sort(chars); //排序的目的是使变位词字母的顺序固定，方便作为key
        String key = String.valueOf(chars); //把chars转回String作为key保存
        if (!mp.containsKey(key)) mp.put(key, new ArrayList<>());
        mp.get(key).add(s); //key相同的变位词分到一组
    }
    return new ArrayList<>(mp.values());
}
```

```js
//js
var groupAnagrams = function (strs) {
    let mp = {}
    for (s of strs) {
        let key = [...s].sort()
        mp[key] = mp[key] ? mp[key] = [...mp[key], s] : [s] //把整个char数组作为key
    }
    return Object.values(mp)
};
```

```swift
//swift
func groupAnagrams(_ strs: [String]) -> [[String]] {
    var mp = [String: [String]]()
    for s in strs {
        let key = String(s.sorted())
        if let arr = mp[key] { mp[key]!.append(s) }
        else { mp[key] = [s] }
    }
    return Array(mp.values)
}
```

```py
#py
def groupAnagrams(self, strs):
    mp = defaultdict(list)
    for s in strs:
        mp[''.join(sorted(s))].append(s)
    return mp.values()
```

```cpp
//cpp
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string, vector<string>> mp;
    for (string& s : strs) {
        string key = s;
        sort(key.begin(), key.end());
        mp[key].push_back(s);
    }
    vector<vector<string>> res;
    for (auto it = mp.begin(); it != mp.end(); it++) {
        res.push_back(it->second);
    }
    return res;
}
```

## 思路 2

用数组单独统计每个变位词的字母个数，把统计数组相同的变位词分到一组。

## 题解

```java
//java
public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> mp = new HashMap<>();
    for (String s : strs) {
        int[] count = new int[26];
        for (int i = 0; i < s.length(); i++) count[s.charAt(i) - 'a']++;
        String key = Arrays.toString(count);
        if (!mp.containsKey(key)) mp.put(key, new ArrayList<>());
        mp.get(key).add(s);
    }
    return new ArrayList<>(mp.values());
}
```

```go
//go
func groupAnagrams(strs []string) [][]string {
    mp := make(map[[26]int][]string)
    for _, s := range strs {
        var count [26]int
        for _, ch := range s {
            count[ch - 'a']++
        }
        mp[count] = append(mp[count], s)
    }
    var res [][]string
    for _, strs := range mp {
        res = append(res, strs)
    }
    return res
}
```

2020.4.6 by Yong