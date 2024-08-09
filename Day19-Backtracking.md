# [77. Combinations](https://leetcode.com/problems/combinations/description/)
題目

<img width="524" alt="image" src="https://github.com/user-attachments/assets/2ef5ae00-9c7a-4f8b-abcf-edae6ea1028b">

- 回溯經典題，可以將題目抽象成Ｎ叉樹
```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        result = []

        def backtracking(path, startIdx):
            nonlocal n, k 
            if len(path) == k:
                result.append(path[:])
                return
            
            for i in range(startIdx, n + 1):
                path.append(i)
                backtracking(path, i + 1)
                path.pop()
            
        backtracking([], 1)
    
        return result
```

- 同上，剪枝版本，差異在於樹深走已經符合題目要求，就不需要再繼續往寬度搜索了 (舉例: n = 4, k = 4 情況)
```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        result = []

        def backtracking(path, startIdx):
            nonlocal n, k 
            if len(path) == k:
                result.append(path[:])
                return
            
            for i in range(startIdx, n - (k - len(path)) + 2):
                path.append(i)
                backtracking(path, i + 1)
                path.pop()
            
        backtracking([], 1)
    
        return result
```

# [216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/description/)
題目

<img width="728" alt="image" src="https://github.com/user-attachments/assets/2d53a126-c52e-46c7-90c6-cc69fac48fbd">

- 本題為 [77. Combinations](https://leetcode.com/problems/combinations/description/) 延伸，可視為和為 n 的 k 個數的組合，而整個集合已經是固定的了[1,...,9]
```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res = []

        def backtracking(sum, startIdx, path):
            if k == len(path):
                if sum == n: res.append(path[:])
                return
            
            for i in range(startIdx, 9 + 1):
                sum += i
                path.append(i)
                backtracking(sum, i + 1, path)
                sum -= i
                path.pop()
                
        backtracking(0, 1, [])

        return res
```

- 剪枝: (1) 當 sum 已經大於題目要求. (2) 當橫向的第一個搜索數值已經大於 n. 以上兩個條件其中之一都可以不用繼續搜索 
```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res = []

        def backtracking(sum, startIdx, path):
            if k == len(path):
                if sum == n: res.append(path[:])
                return
            
            for i in range(startIdx, (9 - (k - len(path)) + 1) + 1): # (2) 當橫向的第一個搜索數值已經大於 n
                sum += i
                path.append(i)
                if sum > n: #(1) 當 sum 已經大於題目要求
                    sum -= i
                    path.pop()
                    return
                backtracking(sum, i + 1, path)
                sum -= i
                path.pop()
                
        backtracking(0, 1, [])

        return res
```

# [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)
題目

<img width="532" alt="image" src="https://github.com/user-attachments/assets/05d5ec8b-83c6-4ba5-834d-ae3ddb90645c">

- 這題跟上面兩題很類似，都是組合類型的問題，而關鍵在於，本題要先建立一個字符對應表，再用此表去找進行回溯
```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not len(digits): return []

        letters = [
            "",     # 0
            "",     # 1
            "abc",  # 2
            "def",  # 3
            "ghi",  # 4
            "jkl",  # 5
            "mno",  # 6
            "pqrs", # 7
            "tuv",  # 8
            "wxyz"  # 9
        ]

        res, s = [], ""
        
        def backtracking(idx):
            nonlocal s
            if len(digits) == idx:
                res.append(s)
                return

            digit = int(digits[idx])
            letter = letters[digit]
            for i in range(len(letter)):
                s += letter[i]
                backtracking(idx + 1)
                s = s[:-1]

        backtracking(0)

        return res
```
