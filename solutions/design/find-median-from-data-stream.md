# [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

求数据流的中位数。

## 举个栗子🌰
```java
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);
medianFinder.addNum(2);
medianFinder.findMedian(); // (1 + 2) / 2 = 1.5
medianFinder.addNum(3);
medianFinder.findMedian(); // 2.0
```

## 思路

用一个最大堆和一个最小堆维护和中位数有关的两个关键数字。

堆的作用是给数字排序，使两个堆大小相当就容易取中位数。

最大堆反着读，最小堆顺着读，它们的根节点就是和中位数有关的关键数字。

![p295](/pictures/p295.jpg)

## 题解

```java
//java
class MedianFinder {
    private Queue<Integer> minHeap, maxHeap;

    public MedianFinder() {
        minHeap = new PriorityQueue<>();
        maxHeap = new PriorityQueue<>();
    }

    public void addNum(int num) {
        if (maxHeap.size() < minHeap.size()) {
            minHeap.add(num);
            maxHeap.add(-minHeap.poll());
        } else {
            maxHeap.add(-num); //变负数再放进最大堆
            minHeap.add(-maxHeap.poll()); //拿出来时加上负号使之回正
        }
    }

    public double findMedian() {
        if (maxHeap.size() == minHeap.size()) return (-maxHeap.peek() + minHeap.peek()) / 2.0;
        return minHeap.peek();
    }
}
```

```cpp
//cpp
class MedianFinder {
private:
    priority_queue<int> minHeap, maxHeap;

public:
    MedianFinder() {}
    
    void addNum(int num) {
        if (minHeap.size() > maxHeap.size()) {
            minHeap.push(num);
            int top = minHeap.top(); minHeap.pop();
            maxHeap.push(-top);
        } else {
            maxHeap.push(-num);
            int top = maxHeap.top(); maxHeap.pop();
            minHeap.push(-top);
        }
    }
    
    double findMedian() {
        if (minHeap.size() > maxHeap.size()) return minHeap.top();
        return (-maxHeap.top() + minHeap.top()) / 2.0;
    }
};
```

2022.11.12 by Yong