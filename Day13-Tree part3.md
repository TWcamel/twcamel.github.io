# [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/description/)
題目

<img width="243" alt="image" src="https://github.com/user-attachments/assets/3df40120-2e6a-4b4d-86ce-f29bdc971990">

- 遞迴法
- 注意本題要求的是“高度”而不是“深度”
- 注意求高度時，因為是 bottom-up，因此必須是 **post-order traverse** 遍歷
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        if not root: return True
        return False if self.getH(root) == -1 else True
    
    def getH(self, node):
        if not node: return 0

        lH = self.getH(node.left)
        if lH == -1: return -1
        rH = self.getH(node.right)
        if rH == -1: return -1

        return -1 if abs(lH - rH) > 1 else 1 + max(lH, rH)
```

# [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/description/)
題目

<img width="247" alt="image" src="https://github.com/user-attachments/assets/0b603b03-4673-4dd8-83ac-259924a8504b">

- 遞迴法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        if not root: return []
        res = []

        def dfs(node, s):
            if not node: return s
            s += f"->{node.val}"
            if node.left: dfs(node.left, s)
            if node.right: dfs(node.right, s)
            if not node.left and not node.right: res.append(s)
        
        s = f"{root.val}"
        if root.left: dfs(root.left, s)
        s = f"{root.val}"
        if root.right: dfs(root.right, s)
        if not root.left and not root.right: res.append(s)
        
        return res
```

# [404. Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/description/)
題目

<img width="425" alt="image" src="https://github.com/user-attachments/assets/0e055693-f90b-4c89-925e-698d2063b464">

- dfs
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        sm = 0

        def dfs(node, tp):
            nonlocal sm
            if not node: return
            if node.left: dfs(node.left, 1)
            if node.right: dfs(node.right, 2)
            if not node.left and not node.right and tp == 1: sm += node.val
        
        dfs(root, 0)
            
        return sm
```

- bfs
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        que, arr = deque([(root, 0)]), []

        while que:
            lst = []

            for i in range(len(que)):
                node, tp = que.popleft() 
                lst.append(node.val)
                if not node.left and not node.right and tp == 1: lst.append(None)
                if node.left: que.append((node.left, 1))
                if node.right: que.append((node.right, 2))
            
            arr.append(lst)
        
        sm = 0
        if len(arr) == 1: return sm

        for ele in arr:
            for i, e in enumerate(ele):
                if e == None: 
                    sm += ele[i-1]

        return sm
```

# [222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/description/)
題目

<img width="657" alt="image" src="https://github.com/user-attachments/assets/972c5e32-7b49-4a1a-bf61-367296a8b3cc">

- 遞迴法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        sm = 0
        if not root: return sm

        def dfs(node):
            nonlocal sm
            if not node: return
            if node.left: dfs(node.left)
            sm += 1
            if node.right: dfs(node.right)
        
        dfs(root)

        return sm
```
