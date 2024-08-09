# [93. Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/description/)
題目

- 卡關的點, 下次寫題目前要先考慮清楚：
  1.  驗證函數 `s[startIdx] == '0'` 不小心寫成 `s[startIdx] == 0`
  2.  字串左閉右閉 `(startIdx, i)`
  3.  如何將字串加入 "." 之後進行驗證
```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        res = []

        def backtracking(s, startIdx, cnt):
            if cnt == 3:
                if self.isValid(s, startIdx, len(s) - 1):
                    res.append(s)
                return
            
            for i in range(startIdx, len(s)):
                if self.isValid(s, startIdx, i):
                    cnt += 1
                    # print(f"{s[:i + 1]} . {s[i + 1:]}")
                    s = s[:i + 1] + '.' + s[i + 1:]
                    backtracking(s, i + 2, cnt)
                    cnt -= 1
                    # print(f"backtrackted!! {s}, {s[:i + 1] + s[i + 2:]}")
                    s = s[:i + 1] + s[i + 2:]
                else:
                    break
            
        backtracking(s, 0, 0)

        return res

    def isValid(self, s, startIdx, endIdx):
        if startIdx > endIdx: return False
        if startIdx != endIdx and s[startIdx] == '0': return False
        return 0 <= int(s[startIdx:endIdx+1]) <= 255
```

# [78. Subsets](https://leetcode.com/problems/subsets/description/)
題目

- 本題是子集，可以把它想成組合問題的一種
- 如果把組合/分割/子集 問題都抽象化成一顆樹的話，***組合與分割問題相當於搜集數的葉節點，而子集則是搜集所有樹的節點***
```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        res = []

        def backtracking(path, startIdx):
            res.append(path)

            for i in range(startIdx, len(nums)):
                path.append(nums[i])
                backtracking(path[:], i + 1)
                path.pop()

        backtracking([], 0)

        return res
```

# [90. Subsets II](https://leetcode.com/problems/subsets-ii/description/)
題目

- 此題關鍵在於樹層與樹枝去重，與 [40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/description/) 類似
```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        res, used = [], [False for _ in range(len(nums))]

        def backtracking(path, startIdx):
            res.append(path)

            for i in range(startIdx, len(nums)):
                # used[i - 1] == True，說明同一樹枝 nums[i - 1] 使用過
                # used[i - 1] == False，說明同一樹層 nums[i - 1] 使用過
                if i > 0 and nums[i - 1] == nums[i] and not used[i - 1]: continue
                path.append(nums[i])
                used[i] = True
                backtracking(path[:], i + 1)
                used[i] = False
                path.pop()

        nums.sort()
        backtracking([], 0)

        return res
```