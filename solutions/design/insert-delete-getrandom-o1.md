# [Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

设计一个集合，要求插入、删除、随机获取都是O(1)时间。

## 举个栗子🌰
```java
RandomizedSet randomizedSet = new RandomizedSet();
randomizedSet.insert(1); // ->true，集合元素为{1}
randomizedSet.remove(2); // ->false，集合中没有2
randomizedSet.insert(2); // ->true，集合元素为{1,2}
randomizedSet.getRandom(); // {1,2}中的某一个
randomizedSet.remove(1); // true，去掉1
randomizedSet.insert(2); // false，集合中已经有2
randomizedSet.getRandom(); // ->2
```

## 思路

插入的元素都保存到数组里，删除时用数组的最后一个元素替换被删的元素。

因为插入或删除都需要O(1)时间判断元素是否已存在，可以用哈希表记录元素及其在数组里的位置。

## 题解

```java
//java
class RandomizedSet {
    List<Integer> A;
    Map<Integer, Integer> mp;
    Random rand = new Random();

    public RandomizedSet() {
        A = new ArrayList<>();
        mp = new HashMap<>();
    }

    public boolean insert(int val) {
        if (mp.containsKey(val)) return false;
        mp.put(val, mp.size()); //A.size()也可以
        A.add(val);
        return true;
    }

    public boolean remove(int val) {
        if (!mp.containsKey(val)) return false;
        int i = mp.get(val);
        int lastNum = A.get(A.size() - 1);
        A.set(i, lastNum); //用最后一个元素替换被删元素
        A.remove(A.size() - 1); //替换好后就可以删掉最后一个元素，因为它已经转移到新位置
        mp.put(lastNum, i); //更新最后一个元素的新位置
        mp.remove(val);
        return true;
    }

    public int getRandom() {
        return A.get(rand.nextInt(A.size()));
    }
}
```

```cpp
//cpp
class RandomizedSet {
private:
    vector<int> A;
    unordered_map<int, int> mp;

public:
    
    RandomizedSet() {
        
    }
    
    bool insert(int val) {
        if (mp.find(val) != mp.end()) return false;
        mp[val] = A.size();
        A.push_back(val);
        return true;
    }
    
    bool remove(int val) {
        if (mp.find(val) == mp.end()) return false;
        int i = mp[val], last = A.back();
        A[i] = last;
        A.pop_back();
        mp[last] = i;
        mp.erase(val);
        return true;
    }
    
    int getRandom() {
        return A[rand() % A.size()];
    }
};
```

```go
//go
type RandomizedSet struct {
    A []int
    Mp map[int]int
}


func Constructor() RandomizedSet {
    return RandomizedSet{[]int{}, make(map[int]int)}
}


func (this *RandomizedSet) Insert(val int) bool {
    if _, ok := this.Mp[val]; ok { return false }
    this.Mp[val] = len(this.A)
    this.A = append(this.A, val)
    return true
}


func (this *RandomizedSet) Remove(val int) bool {
    i, ok := this.Mp[val]
    if !ok { return false }
    last := this.A[len(this.A) - 1]
    this.A[i] = last
    this.A = this.A[:len(this.A) - 1]
    this.Mp[last] = i
    delete(this.Mp, val) //注意先更新mp再delete，因为更新的可能刚好就是最后一个元素，不需要保留
    return true
}


func (this *RandomizedSet) GetRandom() int {
    return this.A[rand.Intn(len(this.A))]
}
```

2020.6.12 by Yong