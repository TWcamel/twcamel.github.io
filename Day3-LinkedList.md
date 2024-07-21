# [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/description/)
題目

![image](https://github.com/user-attachments/assets/092565e4-35ce-4cff-876a-117c4e5adb49)


思路
- 延續指針，鍊表也是將節點的下個指針指向下個節點
- 若要進行刪除，則將鍊表中的下個節點指向下下個即可完成刪除
- 為何要 dummy? 因為移除頭節點跟其他元素的操作規則不同，dummy 可以讓所以練表上的操作規則相同
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        dummy = ListNode()
        dummy.next = head
        curr = dummy

        while curr.next:
            if curr.next.val == val:
                curr.next = curr.next.next
            else:
                curr = curr.next

        return dummy.next
```

# [707. Design Linked List](https://leetcode.com/problems/design-linked-list/description/)
題目

![image](https://github.com/user-attachments/assets/cf67f1ae-fd87-4045-9eac-88c30741c46c)


思路
- 本題考驗對於鍊表的熟悉度，綜合各種操作元素
- 自己在寫這題時，addAtIndex 的 early return 條件下錯進而造成 10 分鐘的 debug, 要再多練習對於操作元素下標的熟悉度
```python
class ListNode:
    def __init__(self, val = 0, next = None):
        self.val = val
        self.next = next

class MyLinkedList:

    def __init__(self):
        self.dummy = ListNode()
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1

        curr = self.dummy

        while curr.next and index > 0:
            curr = curr.next
            index -= 1

        return curr.next.val

    def addAtHead(self, val: int) -> None:
        self.dummy.next = ListNode(val, self.dummy.next)
        self.size += 1

    def addAtTail(self, val: int) -> None:
        curr = self.dummy

        while curr.next:
            curr = curr.next
        
        curr.next = ListNode(val)
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return

        curr = self.dummy

        while curr.next and index > 0:
            index -= 1
            curr = curr.next
        
        curr.next = ListNode(val, curr.next)
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return

        curr = self.dummy

        while curr.next and index > 0:
            index -= 1
            curr = curr.next
        
        curr.next = curr.next.next
        self.size -= 1
        
# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
```

# [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/)
題目

![image](https://github.com/user-attachments/assets/908e32da-ad23-41e2-8b7a-56413e19ef4f)


思路
- 最終結果是整個鍊表翻轉過來，因此先指定兩個指針 pre, cur
- 為什麼要保存 tmp? 因為下次迭代改變 cur.next 指向，因此需要將其先存起來
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        cur, pre = head, None

        while cur:
            tmp = cur.next
            cur.next = pre
            pre = cur
            cur = tmp

        return pre
```
