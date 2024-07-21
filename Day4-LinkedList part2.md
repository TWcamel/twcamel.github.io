# [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/description/)
題目

思路
- 此題與 206. 不同處在於，如果將 n%2 作為一個小區，在小區內反轉鍊表，最後會不小心寫成一個 cycle 不然就是小區一的最後一個節點找不到下個小區的第一個節點
- 最終返回結果需是初始鍊表的第二個節點，因此使用 dummy
- 由於要反轉處是成雙的，因此迴圈內的中止條件也要判斷下下個節點是否為空
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(next = head)
        curr = dummy

        while curr.next and curr.next.next:
            next = curr.next
            next2 = curr.next.next.next

            curr.next = curr.next.next
            curr.next.next = next
            curr.next.next.next = next2

            curr = curr.next.next

        return dummy.next
```

# [19. Remove Nth Node From End of List](https://leetcode.com/problems/swap-nodes-in-pairs/description/)
題目

思路
- 觀察一下，此題的總結點數量與倒數第 n 個節點，與回傳值之間的關係
- 用快慢指針，快指針走完所有節點，扣掉倒數第 n 個節點，每次迭代慢指針都往前一步，當走到第 cnt - n 個節點時，剛好就是第 n 個節點的前一個節點
- 類似題: 203.
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:

        dummy = ListNode(next=head)
        slow, fast = None, head
        cnt = 0

        while fast:
            fast = fast.next
            cnt += 1
        
        while cnt:
            slow = slow.next
            cnt -= 1
            
        slow.next = slow.next.next

        return dummy.next
        
```

# [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/description/)
題目

思路
- 首先看到題目的時候寫了以下的 code 
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head == None:
            return None
            
        slow, fast = head, head

        while fast.next and fast.next.next:
            fast = fast.next.next
            if fast == slow:
                return slow
            slow = slow.next

        return None
```

- 錯在哪?
  - 假設 head 離環入口點為距離為 x, 環中，x 離快慢指針相遇的點為 y, 相遇點到 x 的距離為 z
  - 上面的 code 指出了 x+y，快慢指針相遇的點，而題目要求是 x+y+z 相遇的點

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head == None:
            return None
            
        slow, fast = head, head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            
            if slow == fast:
                start = head
                ptr = slow

                while ptr != start:
                    ptr = ptr.next
                    start = start.next

                return ptr

        return None
            
        
```

# [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)
題目

- 此題考相遇的點，由於兩條鍊表長短不同，因此先遍歷完兩邊的長度，當把兩邊差異都消除後，兩邊一起往前走一步就會到達相遇點
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        if headA == None or headB == None:
            return None

        dummyA, dummyB = ListNode(next=headA), ListNode(next=headB)
        currA, currB = dummyA.next, dummyB.next
        sizeA, sizeB = 0, 0 

        while currA:
            currA = currA.next
            sizeA += 1

        while currB:
            currB = currB.next
            sizeB += 1

        minus = 0
        currA, currB = dummyA.next, dummyB.next

        if sizeA > sizeB:
            minus = sizeA - sizeB
            while currA.next and minus:
                currA = currA.next
                minus -= 1
        elif sizeA < sizeB:
            minus = sizeB - sizeA
            while currB.next and minus:
                currB = currB.next
                minus -= 1

        while currA and currB:
            if currA == currB:
                return currA
            currA = currA.next
            currB = currB.next
        
        return None
```