# [513. Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/description/)
題目

- dfs
- 題目要求要最左邊，且最下面的葉子，因此只要保證左邊子節點是第一個訪問的，且取最後一層的子節點即可
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        if not root: return -1
        max_dep, res = float('-inf'), -1
        
        def dfs(node, level):
            nonlocal max_dep, res
            if not node: return
            if not node.left and not node.right:
                if max_dep < level:
                    max_dep = level
                    res = node.val
            if node.left: dfs(node.left, level + 1)
            if node.right: dfs(node.right, level + 1)
        
        dfs(root, 1)
            
        return res
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
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        que, arr = deque([root]), []

        while que:
            lst = []

            for i in range(len(que)):
                node = que.popleft()
                if node.left: que.append(node.left)
                if node.right: que.append(node.right)
                lst.append(node.val)
                if not node.left and not node.right: lst.append(None)
            
            arr.append(lst)
        
        for i, e in enumerate(arr[-1]):
            if e == None:
                return arr[i -1]
                
        return -1


```

# [112. Path Sum](https://leetcode.com/problems/path-sum/description/)
題目

- dfs
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root: return False
        res = False

        def dfs(node, left):
            nonlocal res
            if not node: return 
            if node.left: dfs(node.left, left - node.val)
            if node.right: dfs(node.right, left - node.val)
            if not node.left and not node.right and left - node.val == 0: res = True
        
        dfs(root, targetSum)
            
        return res
```

# [113. Path Sum II](https://leetcode.com/problems/path-sum-ii/description/)
題目

- dfs
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        res = []
        if not root: return res
        
        def dfs(node, arr, left):
            if not node: return
            if node.left or node.right:
                arr.append(node.val)
            if not node.left and not node.right and left - node.val == 0: 
                arr.append(node.val)
                res.append(arr)
            if node.left: 
                dfs(node.left, arr[:], left - node.val)
            if node.right: 
                dfs(node.right, arr[:], left - node.val)

        dfs(root, [], targetSum)
        
        return res
```

# [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)
題目

- 以後序數組的最後一個元素為切割點，先切中序數組，依中序數組，反過來再切後序數組。一層一層切下去，每次後序數組最後一個元素就是節點元素
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        if not inorder or not postorder:
            return None
        
        val = postorder[-1]
        root = TreeNode(val)

        seperate = inorder.index(val)

        inorderL = inorder[:seperate]
        inorderR = inorder[seperate+1:]

        postorderL = postorder[:len(inorderL)]
        postorderR = postorder[len(inorderL): len(postorder) - 1]

        root.left = self.buildTree(inorderL, postorderL)
        root.right = self.buildTree(inorderR, postorderR)

        return root

```

# [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)
題目

- 前序數組的第一個元素為切割點，先切中序數組，依中序數組，反過來再切前序數組。一層一層切下去，每次前序數組最後一個元素就是節點元素
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder or not inorder:
            return None

        root_val = preorder[0]
        root = TreeNode(root_val)

        idx = inorder.index(root_val)

        inorderL = inorder[:idx]
        inorderR = inorder[idx+1:]

        preorderL = preorder[1 : 1 + len(inorderL)]
        preorderR = preorder[len(inorderL)+1 :]

        root.left = self.buildTree(preorderL, inorderL)
        root.right = self.buildTree(preorderR, inorderR)

        return root
```