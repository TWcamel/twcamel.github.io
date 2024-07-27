# [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)
題目

- 遞迴法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
    
        def traverse(node: TreeNode):
            if not node:
                return

            res.append(node.val)
            traverse(node.left)
            traverse(node.right)

        traverse(root)

        return res
```

- 迴圈法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res, st = [], deque()
        if not root: return res

        st.append(root)
        while st:
            node = st.pop()
            res.append(node.val)
            if node.right: st.append(node.right)
            if node.left: st.append(node.left)

        return res
```

- 統一寫法
```python
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res, st = [], deque()
        if not root: return res
        st.append(root)

        while st:
            node = st[-1]
            if node:
                st.pop()
                if node.right: st.append(node.right)
                if node.left: st.append(node.left)
                st.append(node)
                st.append(None) # mark that we need to operate this
            else:
                st.pop() # remove the null element
                curr = st.pop()
                res.append(curr.val)

        return res
```

# [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)
題目

- 遞迴法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        
        def traverse(node):
            if not node:
                return
            traverse(node.left)
            traverse(node.right)
            res.append(node.val)

        traverse(root)
        return res
```

- 迴圈法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res, st = [], deque()
        if not root: return res
        st.append(root)
        
        while st:
            node = st.pop()
            res.append(node.val)
            if node.left: st.append(node.left)
            if node.right: st.append(node.right)
        
        res.reverse()
        return res
```

- 統一寫法
```python
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res, st = [], deque()
        if not root: return res

        st.append(root)

        while st:
            node = st[-1]
            if node:
                st.pop()
                st.append(node)
                st.append(None)

                if node.right: st.append(node.right)
                if node.left: st.append(node.left)
            else:
                st.pop()
                res.append(st.pop().val)

        return res

```

# [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)
題目

- 遞迴法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []

        def traverse(node):
            if not node:
                return
            
            traverse(node.left)
            res.append(node.val)
            traverse(node.right)

        traverse(root)

        return res
```

- 迴圈法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res, st = [], deque()
        node = root
        while node or st:
            # node is not empty
            if node:
                st.append(node)
                node = node.left # left
            else:
                curr = st.pop()
                res.append(curr.val) # middle
                node = curr.right # right
        return res
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
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res, st = [], deque()
        if not root: return res
        st.append(root)

        while st:
            node = st[-1]
            if node:
                st.pop()

                if node.right: st.append(node.right)
                st.append(node)
                st.append(None) # mark that we need to operate this
                if node.left: st.append(node.left)
            else:
                st.pop() # remove the null element
                curr = st.pop()
                res.append(curr.val)

        return res
```

# [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        res, que = [], deque()
        if not root: return res
        que.append(root)

        while que:
            lst = []
            
            for _ in range(len(que)):
                node = que.popleft()
                lst.append(node.val)
                if node.left: que.append(node.left)
                if node.right: que.append(node.right)

            res.append(lst)
                
        
        return res
        
```