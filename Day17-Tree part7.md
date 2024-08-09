# [701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/description/)
題目

<img width="704" alt="image" src="https://github.com/user-attachments/assets/c13fcc0e-1a74-4023-b0de-bc12da4be19c">

- 可依照順序遍歷，當遇到 null 時，將值給填上即可
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return TreeNode(val)
        
        def dfs(node):
            if val < node.val:
                if node.left:
                    dfs(node.left)
                else:
                    node.left = TreeNode(val)
            else:
                if node.right:
                    dfs(node.right)
                else:
                    node.right = TreeNode(val)
        
        dfs(root)
        return root
```

# [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/description/)
題目

<img width="540" alt="image" src="https://github.com/user-attachments/assets/1f8ddce6-17fa-451b-b779-1617db233b4c">

- 遞迴法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if root == None: return root

        def dfs(node):
            if not node: return node
            if node.val == key:
                if not node.left and not node.right: return None
                elif not node.left: return node.right
                elif not node.right: return node.left
                else:
                    ref = node.right
                    while ref.left:
                        ref = ref.left
                    ref.left = node.left
                    return node.right
            else:
                if key < node.val: node.left = dfs(node.left)
                if key > node.val: node.right = dfs(node.right)
                
            return node

        return dfs(root)
```

- 遞迴法2
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if not root:
            return root
        
        def findMin(node):
            while node.left:
                node = node.left
            return node
        
        def dfs(node, key):
            if not node:
                return node
            
            if key < node.val:
                node.left = dfs(node.left, key)
            elif key > node.val:
                node.right = dfs(node.right, key)
            else:
                # Node with only one child or no child
                if not node.left:
                    return node.right
                elif not node.right:
                    return node.left
                
                # Node with two children: Get the inorder successor (smallest in the right subtree)
                temp = findMin(node.right)
                
                # Copy the inorder successor's content to this node
                node.val = temp.val
                
                # Delete the inorder successor
                node.right = dfs(node.right, temp.val)
            
            return node
        
        return dfs(root, key)
```
