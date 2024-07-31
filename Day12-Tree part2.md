# [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/)
題目

<img width="357" alt="image" src="https://github.com/user-attachments/assets/e0c14790-a8b4-4cf7-b0c6-83f5a51beeef">

- 遞迴法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root: return root

        def dfs(node):
            if not node: return

            tmp = node.left
            node.left = node.right
            node.right = tmp
            if node.left: dfs(node.left)
            if node.right: dfs(node.right)

        dfs(root)

        return root
```

- 統一寫法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        st = deque()
        if not root: return root
        st.append(root)

        while st:
            node = st[-1]
            if node:
                st.pop()
                if node.right: st.append(node.right)
                if node.left: st.append(node.left)
                st.append(node)
                st.append(None)
            else:
                st.pop()
                cur = st.pop()
                tmp = cur.left
                cur.left = cur.right
                cur.right = tmp
        
        return root
```

# [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/description/)
題目

<img width="360" alt="image" src="https://github.com/user-attachments/assets/e84124dc-b469-433f-a95a-d343ca27d5c2">

- 層序法 + 雙指針
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        arr, que = deque(), deque()
        if not root: return False
        que.append(root)

        while que:
            lst = []

            for _ in range(len(que)):
                node = que.popleft()

                if not node: lst.append(None) 
                else: 
                    lst.append(node.val)
                    que.append(node.left)
                    que.append(node.right)

            arr.append(lst)
        
        if len(arr) == 1: return True

        arr.popleft()

        for i in arr:
            l, r = 0, len(i) - 1
            while l < r:
                if i[l] != i[r]: return False
                l += 1
                r -= 1
            
        return True
```

# [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)
題目

<img width="438" alt="image" src="https://github.com/user-attachments/assets/e406f179-33b3-4ca3-ab34-18a1c273f8c0">

- dfs
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        level = 0
        if not root: return level
        st = [(root, 1)]

        while st:
            node, dep = st.pop()
            level = max(level, dep)
            if node.left: st.append((node.left, level + 1))
            if node.right: st.append((node.right, level + 1))
        
        return level
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
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        level, que = 0, deque()
        if not root: return level
        que.append(root)
        while que:
            for _ in range(len(que)):
                node = que.popleft()
                if node.left: que.append(node.left)
                if node.right: que.append(node.right)
            level += 1
        
        return level
```

# [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)
題目

<img width="405" alt="image" src="https://github.com/user-attachments/assets/ec582dd9-c08b-4a23-9811-afc3f41c8fa1">

- dfs
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def dfs(self, node):
        if not node: return 0
        if not node.left:
            return self.dfs(node.right) + 1
        if not node.right:
            return self.dfs(node.left) + 1
        return min(self.dfs(node.left), self.dfs(node.right)) + 1
    
    def interativeDfs(self, node):
        if not root: return 0
        st, min_dep = [(root, 1)], float('inf')

        while st:
            node, dep = st.pop()
            if not node.left and not node.right: min_dep = min(min_dep, dep)
            if node.left: st.append((node.left, dep+1))
            if node.right: st.append((node.right, dep+1))

        return min_dep

    def minDepth(self, root: Optional[TreeNode]) -> int:
        return self.dfs(root)
            
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
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        level, que = 1, deque()
        que.append(root)
        while que:
            
            for i in range(len(que)):
                node = que.popleft()
                if not node.left and not node.right: return level
                    
                if node and node.left: que.append(node.left)
                if node and node.right: que.append(node.right)

            level += 1

        return level
```
