# [232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/description/)
題目

思路
- 用兩個 Stack, 一個管理 in, 另一個管理 out, 即可達到模擬 Queue 的效果 
```python
class MyQueue:

    def __init__(self):
        self.size = 0
        # in, out
        self.s1, self.s2 = [], []

    def push(self, x: int) -> None: 
        self.s1.append(x)
        self.size += 1

    def pop(self) -> int:
        if self.size == 0:
            return
            
        lenS1 = len(self.s1)
        while lenS1 > 0:
            self.s2.append(self.s1.pop())
            lenS1 -= 1
        res = self.s2.pop()
        lenS2 = len(self.s2)
        while lenS2 > 0:
            self.s1.append(self.s2.pop())
            lenS2 -= 1
        self.size -=1
        return res
        

    def peek(self) -> int:
        lenS1 = len(self.s1)
        while lenS1 > 0:
            self.s2.append(self.s1.pop())
            lenS1 -= 1
        res = self.s2[-1]
        lenS2 = len(self.s2)
        while lenS2 > 0:
            self.s1.append(self.s2.pop())
            lenS2 -= 1
        return res
        

    def empty(self) -> bool:
        return not (len(self.s1) > 0 or len(self.s2) > 0) or self.size < 1
        


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```

# [225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/description/)
題目

思路
- 在 Python 中沒有 Queue 實作，因此用 collections.deque()，模擬用一條 queue 完成 stack 操作
```python
class MyStack:

    def __init__(self):
        self.que = deque()
        self.size = 0

    def push(self, x: int) -> None:
        self.que.appendleft(x)
        self.size += 1

    def pop(self) -> int:
        res = self.que.popleft()
        self.size -= 1
        return res

    def top(self) -> int:
        res = self.que.popleft()
        self.que.appendleft(res)
        return res

    def empty(self) -> bool:
        return self.size < 1
        


# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```

# [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)
題目

思路
- 此題延續前面的 Stack 題型，在做題前須先仔細思考所有 false case 再進行答題 
```python
class Solution:
    def isValid(self, s: str) -> bool:
        arr = []
        for i in s:
            if i == "(":
                arr.append(")")
            elif i == "[":
                arr.append("]")
            elif i == "{":
                arr.append("}")
            # senario 2: ([})
            # senario 3: ()]
            elif not arr or arr[-1] != i:
                return False
            else:
                arr.pop()

        # senario 1: [()
        return True if not arr else False
                

```

# [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/description/)
題目

思路
- 題目關鍵為相鄰單字要移除，因此使用隊列，當頭元素與目前操作元素相等，代表他們相鄰了
- 持續移除直到遍歷完整個字串，再依照最先進入的元素依序貼到要回傳的字串即可完成
```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        que = deque()
        for i in s:
            if len(que) > 0 and que[-1] == i:
                que.pop()
            else: 
                que.append(i)
        
        res = ""
        while que:
            res += que.popleft()

        return res
        
```