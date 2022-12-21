## 226. Invert Binary Tree

**Link:** [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/)

**Thoughts:** 

 - DFS or BFS


DFS: 
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        # base case
        if not root:
            return
        # for each node, switch their left and right child
        tmp = root.left
        root.left = root.right
        root.right = tmp

        # go to left and right
        self.invertTree(root.left)
        self.invertTree(root.right)
        
        return root
```

**Time Complexity:**  O(n)

**Spcae Complexity:**  O(n)


BFS: 
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return
        q = collections.deque()
        q.append(root)

        while q:
            size = len(q)
            for i in range(size):
                cur = q.popleft()

                tmp = cur.left
                cur.left = cur.right
                cur.right = tmp

                if cur.left:
                    q.append(cur.left)
                if cur.right:
                    q.append(cur.right)
        return root
```
       

**Time Complexity:**  O(n)

**Spcae Complexity:**  O(n)


## 101. Symmetric Tree

**Link:** [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/description/)

**Thoughts:** 

 - BFS or DFS

DFS:
```
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        
        def mirror(left,right):

            if not left or not right:
                return False
            elif not left and not right:
                return True
            elif left.val != right.val:
                return False

            outside = mirror(left.left, right.right)
            inside = mirror(left.right, right.left)
            
            return outside and inside
        
        return mirror(root.left, root.right)
```  
       

**Time Complexity:**  O(N)

**Spcae Complexity:**  O(N)

BFS: 
```
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        
        q = collections.deque()
        q.append(root)

        while q:
            size = len(q)
            level = []
            for i in range(size):
                cur = q.popleft()
                if cur != None:
                    level.append(cur.val)
                else:
                    level.append(None)
                    continue

                q.append(cur.left)
                q.append(cur.right)

            l,r = 0, len(level) - 1
            while l < r:
                if level[l] != level[r]:
                    return False
                l += 1
                r -= 1
        return True
```

**Time Complexity:**  O(N)

**Spcae Complexity:**  O(N)
