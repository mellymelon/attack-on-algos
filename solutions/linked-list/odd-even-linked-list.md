# [Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/description/)

把所有奇数位置的节点连起来后再连所有偶数位置的节点。

## 举个栗子🌰
```java
[1,2,3,4,5] -> [1,3,5,2,4]
```

## 思路

![p328](/pictures/p328.jpg)

## 题解

```java
//java
public ListNode oddEvenList(ListNode head) {
    if (head == null) return null;
    ListNode odd = head, even = head.next, evenHead = even;
    while (even != null && even.next != null) {
        odd.next = even.next;
        odd = odd.next;
        even.next = odd.next;
        even = even.next;
    }
    odd.next = evenHead;
    return head;
}
```

```js
//js
var oddEvenList = function(head) {
    if (!head) return null
    let odd = head, even = head.next, evenHead = even
    while (even && even.next) {
        odd.next = even.next
        odd = odd.next
        even.next = odd.next
        even = even.next
    }
    odd.next = evenHead
    return head
};
```

```swift
//swift
func oddEvenList(_ head: ListNode?) -> ListNode? {
    guard let head = head else { return nil }
    var odd = head, even = head.next
    let evenHead = even
    while even != nil && even!.next != nil{
        odd.next = even!.next
        odd = odd.next!
        even!.next = odd.next
        even = even!.next
    }
    odd.next = evenHead
    return head
}
```

```py
#py
def oddEvenList(self, head):
    if not head: return None
    odd, even, evenHead = head, head.next, head.next
    while even and even.next:
        odd.next = even.next
        odd = odd.next
        even.next = odd.next
        even = even.next
        odd.next = evenHead
    return head
```

```go
//go
func oddEvenList(head *ListNode) *ListNode {
    if head == nil { return head }
    odd, even, evenHead := head, head.Next, head.Next
    for even != nil && even.Next != nil {
        odd.Next = even.Next
        odd = odd.Next
        even.Next = odd.Next
        even = even.Next
    }
    odd.Next = evenHead
    return head
}
```

```cpp
//cpp
ListNode* oddEvenList(ListNode* head) {
    if (!head) return nullptr;
    ListNode* odd = head, *even = head->next, *evenHead = head->next;
    while (even && even->next) {
        odd->next = even->next;
        odd = odd->next;
        even->next = odd->next;
        even = even->next;
    }
    odd->next = evenHead;
    return head;
}
```

2020.5.18 by Yong