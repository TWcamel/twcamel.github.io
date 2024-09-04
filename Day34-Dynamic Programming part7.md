# [198. House Robber](https://leetcode.com/problems/house-robber/description/)
題目

<img width="534" alt="image" src="https://github.com/user-attachments/assets/df255d61-6b1e-41ef-8fe4-74daf648353e">

- 思路
  1. dp[i]：在考慮下標為 i 以內的房子中，能夠偷到的最大金額為 dp[i]
  2. dp[i] = max(dp[i-1], dp[i-2] + nums[i])
  3. dp[0] = nums[0], dp[1] = max(nums[0], nums[1])
  4. dp[i] 是從 dp[i-1], dp[i-2] 推導的，因此遞推一定是從前向後
  5. dry run
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return 0
        if len(nums) == 1:
            return nums[0]

        dp = [0] * len(nums)
        dp[0] = nums[0]
        dp[1] = max(nums[0], nums[1])

        for i in range(2, len(nums)):
            dp[i] = max(dp[i-1], dp[i-2] + nums[i])
        
        return dp[len(nums) - 1]
```

# [213. House Robber II](https://leetcode.com/problems/house-robber-ii/description/)
題目

<img width="665" alt="image" src="https://github.com/user-attachments/assets/f8765c64-2a6c-4db1-80de-965f953ac3ee">

- 思路：把以下情況都考慮進來之後分開解決，就可以轉化成 [198. House Robber](https://leetcode.com/problems/house-robber/description/)
  - 情況一：不考慮頭尾元素，只考慮中間元素
  - 情況二：考慮頭元素，不考慮尾元素
  - 情況三：考慮尾元素，不考慮頭元素
  - 情況二＆三包含了情況一
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        length = len(nums)
        if length == 0:
            return 0
        if length == 1:
            return nums[0]
        res1 = self.robRange(nums, 0, len(nums) - 2) #情況二：考慮頭元素，不考慮尾元素
        res2 = self.robRange(nums, 1, len(nums) - 1) #情況三：考慮尾元素，不考慮頭元素

        return max(res1, res2) # 情況二＆三包含了情況一
        
    def robRange(self, nums, start, end):
        if end == start: 
            return nums[start]

        dp = [0] * len(nums)
        dp[start] = nums[start]
        dp[start + 1] = max(nums[start], nums[start + 1])
        
        for i in range(start + 2, end + 1):
            dp[i] = max(dp[i-1], dp[i-2] + nums[i])
        
        return dp[end]
```
# [337. House Robber III](https://leetcode.com/problems/house-robber-iii/description/)
題目: 本題是樹形 dp 的經典問題

<img width="537" alt="image" src="https://github.com/user-attachments/assets/07dde6c6-c80d-4944-b51b-aacde3f28f96">

- 思路: 可以用遞迴解，但會 TLE
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        if not root.left and not root.right:
            return root.val
        # 偷 parent
        val1 = root.val
        if root.left: val1 += self.rob(root.left.left) + self.rob(root.left.right)
        if root.right: val1 += self.rob(root.right.left) + self.rob(root.right.right)
        # 不偷 parent
        val2 = self.rob(root.left) + self.rob(root.right)
        return max(val1, val2)
```
- 思路: 可以用遞迴解+記憶
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class solution:
    mem = {}
    def rob(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        if not root.left and not root.right:
            return root.val
        if self.mem.get(root):
            return self.mem[root]
        # 偷 parent
        val1 = root.val
        if root.left: val1 += self.rob(root.left.left) + self.rob(root.left.right)
        if root.right: val1 += self.rob(root.right.left) + self.rob(root.right.right)
        # 不偷 parent
        val2 = self.rob(root.left) + self.rob(root.right)
        self.mem[root] = max(val1, val2) #紀錄該節點 "考慮結果" 
        return max(val1, val2)
```
- 思路: 從上面遞回解法可以感受到結果是從之前的結果推演過來，有濃濃的 dp 味，因此使用 dp 來解
  1. 本題關鍵在於決定該節點要偷還是不偷，因此本題 dp 為一個二維數組, 下標 0 代表不偷該節點，所得到的最大金額；下標 1 表示偷了該節點，所得到的最大金額
  2. 遇到空節點，無論偷還是不偷價值都為 0, 相當於初始化
  3. 後序遍歷：因為最後所偷到的金額是從葉節點推演過來，因此要是後序遍歷
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0

        def robTree(node):
            if not node:
                return [0, 0]
            l, r = robTree(node.left), robTree(node.right)
            # 偷 current, 不能偷子節點
            val1 = node.val + l[0] + r[0]
            # 不偷 current
            val2 = max(l[0],l[1]) + max(r[0], r[1])
            return [val2, val1]
        
        noSteal, steal = robTree(root)
        return max(noSteal, steal)
```