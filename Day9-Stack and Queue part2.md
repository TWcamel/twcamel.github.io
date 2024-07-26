# [150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)
題目

思路
- 仔細觀察，若用 stack 可以輕易完成此題目
```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        res, st = 0, deque()
        for i in tokens:
            if i == "+":
                res = int(st.pop()) + int(st.pop())
                st.append(res)
            elif i == "-": 
                res = -int((st.pop())) + int(st.pop())
                st.append(res)
            elif i == "*": 
                res = int(st.pop()) * int(st.pop())
                st.append(res)
            elif i == "/":
                num1 = st.pop()
                num2 = st.pop()
                res = int(num2) / int(num1)
                st.append(res)
            else:
                st.append(i)

        res = int(st.pop())
        
        return res
```

# [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/description/)
題目

思路
- 此題需要用單調隊列來解，但首先一個非常困難的問題是，該如何讓隊列裡面的頭元素維持最大
- 此時我們需要一個隊列，讓元素保持大至小排列，並依照窗口大小逐步剔除隊列元素
```python
class MyQue: 
    def __init__(self):
        self.que = deque()
    
    def pop(self, x):
        if self.que and self.que[0] == x:
            self.que.popleft()
        
    def push(self, x):
        while self.que and x > self.que[-1]:
            self.que.pop()
        self.que.append(x)
        
    def front(self):
        return self.que[0]
        
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        sz = len(nums)
        if sz == k:
            return [max(nums)]
        
        que, res = MyQue(), []
        for i in range(k):
            que.push(nums[i])
        res.append(que.front())

        for i in range(k, sz):
            que.pop(nums[i - k])
            que.push(nums[i])
            res.append(que.front())
        
        return res
```

# [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/)
題目

思路
- 法一: 此題可以當作上一題 [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/description/) 的延續，將 nums 排序後，將數組中的每組數字，最後一位留下記數值，其餘留零，接著用一個 單調隊列 維護記數值大到小排j
- 法二: Counter() 作法
```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        nums.sort()
        res, counter = [], Counter(nums)

        for k, v in counter.most_common(k):
            res.append(k)
        
        return res
```