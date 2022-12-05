# [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

求链表的中间节点，如果有两个，返回两个中的第二个。

## 举个栗子🌰
```java
[1,2] -> [2]
[1,2,3] -> [2,3]
[1,2,3,4] -> [3,4]
```

## 思路

快慢双指针，慢指针一次走一步，快指针一次走两步，快指针走完时慢指针即为所求。

## 题解

Time: `O(N)`

Space: `O(1)`

```java
//java
public ListNode middleNode(ListNode head) {
    ListNode i = head, j = head; //i是慢指针，j是快指针
    while (j != null && j.next != null) {
        i = i.next; //i一次走一步
        j = j.next.next; //j一次走两步
    }
    return i;
}
```

```js
//js
var middleNode = function(head) {
    let i = j = head
    while (j && j.next) {
        i = i.next
        j = j.next.next
    }
    return i
};
```

```swift
//swift
func middleNode(_ head: ListNode?) -> ListNode? {
    var i = head, j = head
    while j?.next != nil {
        i = i!.next //如果j.next存在，i肯定也存在，所以强制upwrap
        j = j!.next!.next
    }
    return i
}
```

```py
#py
def middleNode(self, head):
    i = j = head
    while j and j.next:
        i = i.next
        j = j.next.next
    return i
```

```kt
//kt
fun middleNode(head: ListNode?): ListNode? {
    var i = head
    var j = head
    while (j != null && j.next != null) {
        i = i!!.next
        j = j.next!!.next
    }
    return i
}
```

```go
//go
func middleNode(head *ListNode) *ListNode {
    i, j := head, head
    for j != nil && j.Next != nil {
        i, j = i.Next, j.Next.Next
    }
    return i
}
```

```cpp
//cpp
ListNode* middleNode(ListNode* head) {
    ListNode* i = head, *j = head;
    while (j && j->next)
        i = i->next, j = j->next->next;
    return i;
}
```

2020.4.8 by Yong