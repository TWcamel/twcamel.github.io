# [455. Assign Cookies](https://leetcode.com/problems/assign-cookies/description/)
題目

<img width="537" alt="image" src="https://github.com/user-attachments/assets/9df2696d-c1e3-4af3-98cd-829d8989f478">

- 貪心沒有固定的模式，最好的分辨貪心的方式是利用一個例子嘗試找反例，如果找不到反例，那麼就可以嘗試用貪心來解，不行的話就用 DP
```python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()

        cnt, idx = 0, len(s) - 1
        for i in range(len(g) - 1, -1, -1):
            if idx >= 0 and s[idx] >= g[i]:
                cnt += 1
                idx -= 1
        
        return cnt
```

# [376. Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/description/)
題目

<img width="528" alt="image" src="https://github.com/user-attachments/assets/bc7de485-1ff2-4170-838d-45606b6ce53f">

- 本題需要考慮的是，平坡情況該如何處理
```python
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        res = 1
        currDiff, preDiff = 0, 0
        for i in range(len(nums) - 1):
            currDiff = nums[i+1] - nums[i]
            if (currDiff > 0 and preDiff <= 0) or (currDiff < 0 and preDiff >= 0):
                res += 1
                preDiff = currDiff
        return res
```

# [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/submissions/1358067068/)

<img width="529" alt="image" src="https://github.com/user-attachments/assets/e3c098e5-b312-472f-bce6-7024ba8e02b7">

- 遇到連續負數加總後小於零，這點暗示著很 Greedy
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        sum = float("-inf")
        subNum = 0

        for num in nums:
            subNum += num
            sum = max(sum, subNum)
            if subNum <= 0: 
                subNum = 0 
        
        return sum
```
