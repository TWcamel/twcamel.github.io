# [39. Combination Sum](https://leetcode.com/problems/combination-sum/description/)
題目

- 回溯 (未剪枝): 題目有規定可以無限重複值，直到找到目標，或大於目標則返回
```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res, sum = [], 0

        def backtracking(path, startingIdx):
            nonlocal sum
            if sum > target:
                return
            elif sum == target:
                res.append(path)
                return
            
            for i in range(startingIdx, len(candidates)):
                sum += candidates[i]
                path.append(candidates[i])
                backtracking(path[:], i)
                sum -= candidates[i]
                path.pop()
            
        
        backtracking([], 0)

        return res
```

- 減枝版本: 注意這邊有先排序過，原因在於排序後的加總可以直接比較是否已經大於目標值，若大於的話則可以返回目前答案
```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res, sum = [], 0

        def backtracking(path, startingIdx):
            nonlocal sum
            if sum == target:
                res.append(path)
                return
            
            for i in range(startingIdx, len(candidates)):
                if sum + candidates[i] > target: break #排序後，加總大於目標值，可以減枝
                sum += candidates[i]
                path.append(candidates[i])
                backtracking(path[:], i)
                sum -= candidates[i]
                path.pop()
            
        candidates.sort()  # 需要排序       
        backtracking([], 0)

        return res
```

# [40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/description/)
題目

- 這題關鍵在於需要做樹層去重, 樹枝去重兩個地方
```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        res, sum = [], 0

        def backtracking(startingIdx, path):
            nonlocal sum
            if sum == target:
                res.append(path)
                return
            
            for i in range(startingIdx, len(candidates)):
                if i > startingIdx and candidates[i-1] == candidates[i]: continue #樹枝去重
                if sum + candidates[i] > target: break #剪枝

                sum += candidates[i] 
                path.append(candidates[i]) 
                backtracking(i + 1, path[:]) #回溯
                sum -= candidates[i]
                path.pop()
            
        
        candidates.sort() #樹層去重，需要排序
        backtracking(0, [])

        return res
```

# [131. Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/description/)
題目

- 第一次遇到分割問題，此問題跟組合問題很像，組合問題問的是有幾種組合，切割問題問有幾種切割方式
- 本題有兩個關鍵: (1)如何切割?, (2)判斷回文
```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        res = []

        def backtracking(path, startIdx):
            if startIdx >= len(s):
                res.append(path)
                return
            
            for i in range(startIdx, len(s)): # (1)如何切割 
                if self.isPalindrome(s[startIdx:i]): # (2)判斷回文
                    sub = s[startIdx:i]
                    path.append(sub)
                else:
                    continue
                backtracking(path[:], i + 1)
                path.pop()
            
        backtracking([], 0)

        return res

    def isPalindrome(self, s):
        l, r = 0, len(s) - 1 

        while l < r:
            if not s[l] == s[r]:
                return False
            l += 1
            r -= 1
        
        return True

```