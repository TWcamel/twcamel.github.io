# [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/description/)
題目

- 類似題: [452. Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/), [435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/description/) 
- #1. 判斷是否有重疊，若重疊則合併成一個，不重疊則直接貼到結果集內
```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals = sorted(intervals, key=lambda x : x[0])
        res = []

        res.append(intervals[0])
        for i in range(1, len(intervals)):
            if res[-1][1] >= intervals[i][0]:
                res[-1][1] = max(res[-1][1], intervals[i][1])
            else:
                res.append(intervals[i])
        return res
```

# [738. Monotone Increasing Digits](https://leetcode.com/problems/monotone-increasing-digits/description/)
題目

- #1. 如何想到 greedy? >> 為了保持單調性質，可以若 s[i-1] > s[i], 可以將 s[i-1] 減去一, 然後再將 s[i] 以後的數字都設定為 `9`, 這就可以聯想到貪心
- #2. 但此題還考驗需要重複利用上次結果的特性，因此要後向前序遍歷
```python
class Solution:
    def monotoneIncreasingDigits(self, n: int) -> int:
        lst = list(str(n))

        for i in range(len(lst) - 1, 0, -1): #2
            if lst[i - 1] > lst[i]: #1
                lst[i - 1] = str(int(lst[i - 1]) - 1)
                lst[i:] = '9' * (len(lst) - i)
            
        return int(''.join(lst))
```

# [968. Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/description/)
題目

- 這邊要考的地方是
  - #1. 如何進行遍歷?
    - 是 top-down 還是 bottom-up?
      - 回歸題目初心，是要找最少的 camer，若放頭節點只能少一個 camera，但放葉節點的 parent，則可以指數性的減少 camera
  - #2. 遍歷順序為何?
      - 前中後序遍歷？
        - 本題需要 bottom-up，因此使用後序遍歷
  - #3. 如何相隔兩個節點放 camera?
    - 節點需要狀態, `0`: 無覆蓋, `1`: 有 camera, `2`: 有覆蓋
      - `#@0`: 空節點回 `2`，若空節點回 `1` or `0`, 都不符合題目要求, 並 return `2`
      - `#@1`: 當其 child nodes 其中一個為 `0`, 此節點需放置 camera, 並 return `1`
      - `#@2`: 當 child 其中一個已經有 camera `1`，此節點須標示為有覆蓋，並 return `2`
      - `#@3`: 若頭節點被標示為 `0`, 則需要將結果 += 1
      - `#@4`: 所有的 child 都被覆蓋了, 則回傳未覆蓋 `0`
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minCameraCover(self, root: Optional[TreeNode]) -> int:
        res = 0
        def bottomUp(node): #1
            nonlocal res
            if not node: #@0
                return 2
            
            l = bottomUp(node.left) #2
            r = bottomUp(node.right) 

            if l == 2 and r == 2: #@4
                return 0
            elif l == 0 or r == 0: #@1
                res += 1
                return 1
            elif l == 1 or r == 1: #@2
                return 2
            else: 
                return -1
        
        if bottomUp(root) == 0: #@3
            res += 1
        
        return res
```