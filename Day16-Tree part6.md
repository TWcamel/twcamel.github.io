# [530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/)
題目

- 類似題 [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        if not root: return 0 
        res, arr = float("inf"), []

        def dfs(node):
            nonlocal res
            if not node: return
            if node.left: dfs(node.left)
            arr.append(node.val)
            if node.right: dfs(node.right)
            
        dfs(root)

        l, r = 0, 1
        
        while r < len(arr):
            res = min(res, abs(arr[r] - arr[l]))
            l += 1
            r += 1

        return res
```

# [501. Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/)
題目

類似題 
- [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/)
- [530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        if not root: return []
        res, arr = [], []

        def dfs(node):
            if not node: return
            arr.append(node.val)
            if node.left: dfs(node.left)
            if node.right: dfs(node.right)
        
        dfs(root)

        counter = Counter(arr)
        mo_k, mo_v = counter.most_common(1)[0]

        for k,v in counter.items():
            if v == mo_v:
                res.append(k)
            
        return res
```

# [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)
題目

- 本題需要 bottom-up, 從下往上回溯
- 回溯過程中，即使已經找到答案，依然要爆完整棵樹，因為要運用到遞回的 return 值
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if q == root or p == root or root == None: return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        if left != None and right != None: return root

        if left != None and right == None: return left
        elif left == None and right != None: return right
        else: return None

```
