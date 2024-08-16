# [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)
題目

- 題目要求為一次只能操作買/賣，但同日可賣再買，因此可以利潤最大就是當今日價格大於昨日價格就賣出
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        result = 0
        for i in range(1, len(prices)):
            result += max(prices[i] - prices[i - 1], 0)
        return result
```

# [55. Jump Game](https://leetcode.com/problems/jump-game/description/)
題目

- 每次區域內查看是否 cover 到 nums[i] - 1
```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        if len(nums) == 1: return True
        cover = 0
        for i in range(len(nums)):
            if i <= cover:
                cover = max(i + nums[i], cover)
                if cover >= len(nums) - 1:
                    return True
        return False
```

# [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/description/)
題目

- 與上一題類似，要看這步的涵蓋範圍，也要看下一步的涵蓋範圍，當走到最大涵蓋範圍，就確認需要走下一步
```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return 0

        currDistance, nextDistance = 0, 0
        res = 0

        for i in range(len(nums) - 1):
            nextDistance = max(i + nums[i], nextDistance)
            if i == currDistance:
                currDistance = nextDistance
                res += 1
        
        return res
```

# [1005. Maximize Sum Of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/description/)
題目

- 注意此題可以用暴力解非貪心法來完成，但此章節目的是要練習貪心法，因此此題是用貪心法來完成
- 注意這邊用了兩次貪心
```python
class Solution:
    def largestSumAfterKNegations(self, nums: List[int], k: int) -> int:
        A = sorted(nums, key=abs, reverse=True)

        for i in range(len(A)):
            if A[i] < 0 and k > 0:
                A[i] *= -1
                k -= 1
        
        if k % 2 > 0:
            A[-1] *= -1
        
        return sum(A)
```