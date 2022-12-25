## 513. Find Bottom Left Tree Value ##

**Link: **[513. Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/description/)

**Thoughts: **
  - DFS: 二叉树递归框架代码是先递归左子树，后递归右子树，所以到最大深度时第一次遇到的节点就是左下角的节点。
  - BFS
 
**Solution**
DFS:
```
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        self.traverse(root)
        return self.res

    maxDepth = 0
    depth = 0
    res = 0
    
    def traverse(self, root):
        if not root:
            return
        self.depth += 1
        if self.depth > self.maxDepth:
            self.maxDepth = self.depth
            self.res = root.val
        self.traverse(root.left)
        self.traverse(root.right)
        self.depth -= 1
```

BFS:
```
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        q = collections.deque()
        q.append(root)
        left = []
        while q:
            size = len(q)
            left.append(q[0].val)
            for i in range(size):
                cur = q.popleft()

                if cur.left:
                    q.append(cur.left)
                if cur.right:
                    q.append(cur.right)
        
        return left[-1]
```

## 112. Path Sum ##

**Link**[112. Path Sum](https://leetcode.com/problems/path-sum/description/)

**Thoughts**
  - Top Down DFS
  - Bottom Up DFS
  - BFS

**Solution**
Top Down DFS:
```
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        self.traverse(root,targetSum)
        return self.isTrue

    pathSum = 0
    isTrue = False

    def traverse(self, root,target):
        if not root:
            return
        
        self.pathSum += root.val
        if not root.left and not root.right:
            if self.pathSum == target:
                self.isTrue = True
        self.traverse(root.left,target)
        self.traverse(root.right,target)
        self.pathSum -= root.val
```

Bottom Up DFS:
```
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
            return False
        
        if not root.left and not root.right and root.val == targetSum:
            return True
        
        left = self.hasPathSum(root.left, targetSum - root.val)
        right = self.hasPathSum(root.right, targetSum - root.val)

        return left or right
```

BFS:
```
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
            return False

        q = collections.deque()
        q.append((root,root.val))

        while q:
            size = len(q)
            for i in range(size):
                cur, pathSum = q.popleft()

                if not cur.left and not cur.right and pathSum == targetSum:
                    return True
                
                if cur.left:
                    q.append((cur.left,pathSum + cur.left.val))
                
                if cur.right:
                    q.append((cur.right,pathSum + cur.right.val))
        return False
```

## 113. Path Sum II ##

**Link: **[113. Path Sum II](https://leetcode.com/problems/path-sum-ii/description/)

**Thought: **
  - DFS:
  - BFS:

**Solutions:**
DFS:
```
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        self.traverse(root,targetSum)
        return self.res
    
    pathSum_ = 0
    res=[]
    path = []

    def traverse(self,root,targetSum):
        if not root:
            return
        
        self.pathSum_ += root.val
        self.path.append(root.val)

        if not root.left and not root.right:
            if self.pathSum_ == targetSum:
                self.res.append(list(self.path))
       
        self.traverse(root.left, targetSum)
        self.traverse(root.right, targetSum)

        self.pathSum_ -= root.val
        self.path.pop()
```

BFS:
```
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        if not root:
            return []
        que, temp = deque([root]), deque([(root.val, [root.val])])
        result = []
        while que:
            for _ in range(len(que)):
                node = que.popleft()
                value, path = temp.popleft()
                if (not node.left) and (not node.right):
                    if value == targetSum:
                        result.append(path)
                if node.left:
                    que.append(node.left)
                    temp.append((node.left.val+value, path+[node.left.val]))
                if node.right:
                    que.append(node.right)
                    temp.append((node.right.val+value, path+[node.right.val]))
        return result
```

## 105. Construct Binary Tree from Preorder and Inorder Traversal ##

**Link: **[105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)

**Thoughts:**
  - 第一步：如果数组大小为零的话，说明是空节点了。

  - 第二步：如果不为空，那么取后序数组最后一个元素作为节点元素。

  - 第三步：找到后序数组最后一个元素在中序数组的位置，作为切割点

  - 第四步：切割中序数组，切成中序左数组和中序右数组 （顺序别搞反了，一定是先切中序数组）

  - 第五步：切割后序数组，切成后序左数组和后序右数组

  - 第六步：递归处理左区间和右区间

**Solutions:**
DFS:
```
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder:
            return None
        
        rootVal = preorder[0]
        root = TreeNode(rootVal)

        mid = inorder.index(rootVal)

        inorderLeft = inorder[ : mid]
        inorderRight = inorder[mid + 1: ]

        preorderLeft = preorder[1:mid + 1]
        preorderRight = preorder[mid + 1: ]

        root.left = self.buildTree(preorderLeft, inorderLeft)
        root.right = self.buildTree(preorderRight, inorderRight)

        return root
```

## 106. Construct Binary Tree from Inorder and Postorder Traversal ##

**Link: **[106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)

**Thoughts**
  - Same as 105

**Solutions:**
DFS:
```
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        # check if postorder is exist
        if not postorder:
            return None
        # find the root val from postorder
        rootVal = postorder[-1]
        root = TreeNode(rootVal)

        if len(postorder) == 1:
            return root

        # find root index in inorder
        inorderRootIndex = inorder.index(rootVal)
        # cut inorder array, find the left and right of inorder
        inorderLeft = inorder[:inorderRootIndex]
        inorderRight = inorder[inorderRootIndex + 1:]
        # cut postorder array, find the left and right of postorder
        postorderLeft = postorder[:inorderRootIndex]
        postorderRight = postorder[inorderRootIndex:-1]
        # recursion, build the sub tree
        root.left = self.buildTree(inorderLeft, postorderLeft)
        root.right = self.buildTree(inorderRight, postorderRight)

        return root
```



