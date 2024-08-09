# [669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/description/)
題目

<img width="538" alt="image" src="https://github.com/user-attachments/assets/38c59346-1b39-4fe5-b790-2dd2c850bd4d">

- 遞回法
  - 我們需要刪除後的元素保留有 BTS 的特性，因此用 bottom-up
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if not root: return root
        if root.val < low: return self.trimBST(root.right, low, high)
        if root.val > high: return self.trimBST(root.left, low, high)
        root.left = self.trimBST(root.left, low, high)
        root.right = self.trimBST(root.right, low, high)
        return root
```

- 迴圈法
  - 先將指針移動到左閉右閉的區間中
  - 接著對左右孩子分別處理剪枝流程
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if not root: return root
        
        while root and (root.val < low or root.val > high):
            if root.val < low: root = root.right
            else: root = root.left
        
        ref = root
        while ref:
            while ref.left and ref.left.val < low:
                ref.left = ref.left.right
            ref = ref.left

        ref = root
        while ref:
            while ref.right and ref.right.val > high:
                ref.right = ref.right.left
            ref = ref.right
        
        return root
```

# [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)
題目

<img width="538" alt="image" src="https://github.com/user-attachments/assets/ff157449-c301-4c38-afc6-5d3064048cf5">

- 類似題
  - [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)
  - [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/description/)
  - [701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/description/)
  - [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/description/)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        def traverse(nums, l, r):
            if l > r: return None
            mid = (l + r) // 2
            node = TreeNode(nums[mid])
            node.left = traverse(nums, l, mid - 1)
            node.right = traverse(nums, mid + 1, r)
            return node
        
        return traverse(nums, 0, len(nums) - 1)
```

# [538. Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/description/)
題目

<img width="543" alt="image" src="https://github.com/user-attachments/assets/6e1a7e25-fe66-465e-b015-460282afd2d3">

- 類似題
  - [530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/)
  - [501. Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        sm = 0
        def dfs(node):
            nonlocal sm
            if not node: return
            dfs(node.right)
            node.val += sm
            sm = node.val
            dfs(node.left)
            
        dfs(root)

        return root
```
