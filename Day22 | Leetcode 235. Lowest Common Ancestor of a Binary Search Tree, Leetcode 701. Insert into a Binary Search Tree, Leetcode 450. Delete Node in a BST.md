## 235. Lowest Common Ancestor of a Binary Search Tree ##

**Link: **[235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

**Thoughts:**
  - General DFS: works even if it is a Normal binary tree
  - DFS for BST
  - BFS for BST

**Solutions:**
General DFS:
```
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root:
            return root
        
        if root == p or root == q:
            return root
        
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        if left and right:
            return root
        
        if left and not right:
            return left
        if right and not left:
            return right
        
        if not left and not right:
            return None
```

DFS for BST:
```
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left,p,q)
        if root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right,p,q)
        return root
```

BFS:
```
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        cur = root
        while cur:
            if cur.val > p.val and cur.val > q.val:
                cur = cur.left
            elif cur.val < p.val and cur.val < q.val:
                cur = cur.right
            else: 
                return cur
```

## 701. Insert into a Binary Search Tree ##

**Link: **[701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/)

**Thoughts:**
  - DFS for BST

**Solutions:**
```
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            node = TreeNode(val)
            return node
        
        if root.val < val:
            root.right = self.insertIntoBST(root.right,val)
        if root.val > val:
            root.left = self.insertIntoBST(root.left,val)
        
        return root
```

## 450. Delete Node in a BST ##

**Link: **[450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/description/)

**Thoughts:**
  - 情况 1：A 恰好是末端节点，两个子节点都为空，那么它可以当场去世了：
  ![image](https://user-images.githubusercontent.com/69004164/209887944-f5ef2dd5-db0a-4e15-a044-ad6ca108a150.png)
  - 情况 2：A 只有一个非空子节点，那么它要让这个孩子接替自己的位置：
  ![image](https://user-images.githubusercontent.com/69004164/209888028-7f40b199-8948-4842-b786-5045046b08eb.png)
  - 情况 3：A 有两个子节点，麻烦了，为了不破坏 BST 的性质，A 必须找到左子树中最大的那个节点或者右子树中最小的那个节点来接替自己，我的解法是用右子树中最小节点来替换：
  ![image](https://user-images.githubusercontent.com/69004164/209888093-7fafc5a5-b2b8-4915-bd57-84f8e0cc4749.png)


**Solutions:**
```
class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        # base case:
        if not root:
            return root
        
        if root.val == key:
            # case 1 and 2: deleted node have no child or have one child
            if not root.left: return root.right
            if not root.right: return root.left
            # case 3: deleted node have two children

            # find the left most node in the right sub tree, put it as root
            cur = root.right
            while cur.left:
                cur = cur.left
            cur.left = root.left
            root = root.right
            return root
        
        if root.val > key:
            root.left = self.deleteNode(root.left,key)
        if root.val < key:
            root.right = self.deleteNode(root.right, key)
        
        return root
```
