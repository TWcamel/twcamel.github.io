# [300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/description/)
題目

- 思路： 子序列問題是經典的 dp 問題，這題也是第一次寫到的類似題目，可以當作日後參考
  1. dp[i]: 下標 i 以前，包含 i，以 nums[i] 為結尾的最長遞增子序列長度
  2. dp[i] = max(dp[i], dp[j] + 1)
  3. dp[i] 皆為 1
  4. dp[i] 小到大遞推; dp[j] 小到大 or dp[j] 大到小遞推
  5. dry run:
       | 下標 i | 0   | 1   | 2   | 3                            | 4                             |
       | ------ | --- | --- | --- | ---------------------------- | ----------------------------- |
       | i = 1  | 1   | 2   | 1   | 1                            | 1                             |
       | i = 2  | 1   | 2   | 1   | 1                            | 1                             |
       | i = 3  | 1   | 2   | 1   | 3                            | 1                             |
       | i = 4  | 1   | 2   | 1   | <span style="color: red;"> 3 | <span style="color: red;">  3 |
```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp = [1] * len(nums)
        res = 1
        
        for i in range(1, len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1)
            res = max(dp[i], res)

        return res
```

# [674. Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/)
題目

- 類似題： [300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/description/)
- 思路： 看到子序就想到 dp, 本題要求要是最長的連續子序列
  1. 以下標 i 為結尾的連續最長遞增子序列長度為 dp[i]
  2. dp[i] = dp[i-1] + 1 (若 nums[i] > nums[i-1], 那麼以 i 結尾的連續最長遞增子序列長度一定為 dp[i] + 1)
  3. dp[i] 皆為 1
  4. 左至右遞推
  5. dry run: nums = [1,3,5,4,7]
       | 下標 i | 0   | 1   | 2                            | 3   | 4   |
       | ------ | --- | --- | ---------------------------- | --- | --- |
       | dp[i]  | 1   | 2   | <span style="color: red;"> 3 | 1   | 2   |
```python
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        dp = [1] * len(nums)
        res = 1
        for i in range(1, len(nums)):
            if nums[i] > nums[i-1]:
                dp[i] = dp[i-1] + 1
            res = max(res, dp[i])
        return res
```

# [718. Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray/description/)
題目

- 思路： 看到子序就想到 dp, 兩數組最長子序列，可以視作求二維版本最大連續子序列 [674. Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/)
  1. dp[i][j]: 以下標 i-1 為結尾的 A, 和以下標 j-1 為結尾的 B, 重複最長子序列為 dp[i][j]
  2. dp[i][j] = dp[i-1][j-1] + 1
  3. 根據 dp 定義
     - dp[i][0], dp[0][j] 無意義
     - 為了方便公式遞推 dp[i][0] = 0, dp[0][j] = 0
     - 舉例：當A[0]==B[0] -> dp[1][1] = dp[0][0] + 1 = 1, 符合公式定義
  4. 外Ａ內Ｂ, 左向右遞推
  5. dru run: A = [1,2,3,2,1], B = [3,2,1,4,7]
       |     |     | j   | 1   | 2   | 3                            | 4   | 5   |
       | --- | --- | --- | --- | --- | ---------------------------- | --- | --- |
       | i   |     | B   | 3   | 2   | 1                            | 4   | 7   |
       | 0   | A   | 0   | 0   | 0   | 0                            | 0   | 0   |
       | 1   | 1   | 0   | 0   | 0   | 1                            | 0   | 0   |
       | 2   | 2   | 0   | 0   | 1   | 0                            | 0   | 0   |
       | 3   | 3   | 0   | 1   | 0   | 0                            | 0   | 0   |
       | 4   | 2   | 0   | 0   | 2   | 0                            | 0   | 0   |
       | 5   | 1   | 0   | 0   | 0   | <span style="color: red;"> 3 | 0   | 0   |
```python
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        dp = [[0] * (len(nums2) + 1) for _ in range(len(nums1) + 1)]
        res = 0

        for i in range(1, len(nums1) + 1):
            for j in range(1, len(nums2) + 1):
                if nums1[i-1] == nums2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                res  = max(res, dp[i][j])
        
        return res
```