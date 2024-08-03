# [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/description/)
題目

<img width="760" alt="image" src="https://github.com/user-attachments/assets/e1aecc26-ac1a-40d4-a568-ee6fb9747bbd">

類似題： 
- [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)
- [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums:
            return None
        
        root_val = max(nums)
        root = TreeNode(root_val)

        idx = nums.index(root_val)

        l = nums[:idx]
        r = nums[idx+1:]

        root.left = self.constructMaximumBinaryTree(l)
        root.right = self.constructMaximumBinaryTree(r)

        return root
```

# [617. Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/description/)
題目

<img width="632" alt="image" src="https://github.com/user-attachments/assets/2332535f-49b8-4080-9ef6-b937c534ef87">

- 遞回法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root1 and not root2:
            return None
        sm = 0
        if root1: sm+=root1.val
        if root2: sm+=root2.val
        
        root = TreeNode(sm)

        if (root1 and root1.left) or (root2 and root2.left): 
            root.left = self.mergeTrees(root1=None if not root1 else root1.left, root2= None if not root2 else root2.left)
        if (root1 and root1.right) or (root2 and root2.right): 
            root.right = self.mergeTrees(root1=None if not root1 else root1.right, root2= None if not root2 else root2.right)
        
        return root
        
```

# [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/description/)
題目

<img width="624" alt="image" src="https://github.com/user-attachments/assets/23fc82b4-8fa8-4d00-8b33-a85e0b4cfd2b">

- dfs
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return 
        res = None
        
        def dfs(node):
            nonlocal val, res
            if not node: 
                return
            if val == node.val: 
                res = node
                
            if node.left: 
                dfs(node.left)
            if node.right: 
                dfs(node.right)
        
        dfs(root)
        
        return res
            

        
        
```

# [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/)
題目

<img width="512" alt="image" src="https://github.com/user-attachments/assets/eb28ec6a-6971-4a9d-b6c0-e8534119c68b">

- inorder BTS 保證所有元素照升序排列
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        if not root: return False
        arr = []

        def dfs(node):
            if not node: return
            if node.left: dfs(node.left)
            arr.append(node.val)
            if node.right: dfs(node.right)
        
        dfs(root)

        for i in range(1, len(arr)):
            if arr[i] < arr[i-1]: return False

        return True
```
