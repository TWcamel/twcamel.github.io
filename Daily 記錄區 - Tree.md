# [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)
題目

<img width="537" alt="image" src="https://github.com/user-attachments/assets/91599f56-c05b-43eb-91cd-b480a67f1308">

思路
- 這題有卡住一段時間，卡住的原因是不熟悉 `l, r = dfs(node.left), dfs(node.right)`, 以及對應的回傳值寫法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        res = 0
        def dfs(node):
            nonlocal res
            if not node: return 0
            l, r = dfs(node.left), dfs(node.right)
            res = max(res, l + r)
            return 1 + max(l, r)
        dfs(root)
        return res
```