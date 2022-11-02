# [Minimum Genetic Mutation](https://leetcode.com/problems/minimum-genetic-mutation/)

每次只改变一个字母，求`start`变成`bank`里有的`end`至少需要多少步。

如果无法变成`end`，返回-1。

**备注**

- 所有字符串长度固定为8
- 字符串只由`A`、`C`、`G`、`T`组成

## 举个栗子🌰
```java
"AACCGGTT" "AACGCGAT" ["AACGCGAT","AACGGGTT","AACGGGAT"] -> 3 //AACCGGTT -> AACGGGTT -> AACGGGAT -> AACGCGAT
```

## 思路

每次只改字符串中的一个字母，如果改动后的结果在`bank`里有，继续基于之前的结果作改动直到`end`出现或者返回-1。

![p433](/pictures/p433.jpg)

## 题解

```java
//java,2ms,55%
public int minMutation(String start, String end, String[] bank) {
    Set<String> geneBank = new HashSet<>();
    for (String s : bank) geneBank.add(s); //放进set里方便O(1)时间查询
    int res = 0;
    Queue<String> q = new LinkedList<>();
    q.add(start);
    while (!q.isEmpty()) {
        int sz = q.size();
        while (sz-- > 0) {
            String cur = q.poll();
            if (cur.equals(end)) return res;
            char[] chars = cur.toCharArray(); //转成char数组以便修改字母
            for (int i = 0; i < 8; i++) {
                char tmp = chars[i]; //修改前保存
                for (char ch : "ACGT".toCharArray()) {
                    chars[i] = ch;
                    String cand = String.valueOf(chars);
                    if (geneBank.contains(cand)) { //如果bank里有才放进队列继续搜索
                        geneBank.remove(cand); //为了避免重复搜索，去掉已经搜索过的元素
                        q.add(cand);
                    }
                }
                chars[i] = tmp; //修改后恢复
            }
        }
        res++;
    }
    return -1;
}
```

```py
#py,28ms,96%
def minMutation(self, start, end, bank):
    geneBank = set(bank)
    res = 0
    q = deque([start])
    while q:
        for _ in range(len(q)):
            cur = q.popleft()
            if cur == end: return res
            chars = list(cur)
            for i in range(8):
                tmp = chars[i]
                for ch in "ACGT":
                    chars[i] = ch
                    cand = ''.join(chars)
                    if cand in geneBank:
                        geneBank.remove(cand)
                        q.append(cand)
                chars[i] = tmp
        res += 1
    return -1
```

2022.11.2 by Yong