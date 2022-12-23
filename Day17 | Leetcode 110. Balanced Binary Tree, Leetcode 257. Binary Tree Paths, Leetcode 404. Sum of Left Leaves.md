## 110. Balanced Binary Tree ##

**Link:** [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/description/)

**Thoughts:**
  - Height-Balanced: A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.
  - DFS: For each given node, first calculate the max height of left child and right child, and at the post-order position check if the node is balanced based on the max height difference

**Solution:**
DFS:
```
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        
        if not root:
            return True
        
        self.depth(root)
        return self.isBalance

    isBalance = True
    
    def depth(self, root):
        if not root:
            return 0
        left = self.depth(root.left)
        right = self.depth(root.right)
        if abs(left - right) > 1:
            self.isBalance = False
        return max(left,right) + 1
```

**Time Complexity:** O(n)

**Space Complexity:** O(n)


## 257. Binary Tree Paths ##

**Link:**[257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/description/)

**Thoughts:**
  - BackTracking: 
  ![image](https://user-images.githubusercontent.com/69004164/209369030-2041a482-958a-4de7-a081-8abcbbcaf9bd.png)
  前序遍历的代码在进入某一个节点之前的那个时间点执行，后序遍历代码在离开某个节点之后的那个时间点执行。回想我们刚才说的，「路径」和「选择」是每个节点的属性，函数在树上游走要正确处理节点的属性，那么就要在这两个特殊时间点搞点动作：
  ![image](https://user-images.githubusercontent.com/69004164/209369055-e6ccb25a-b922-4790-99c7-a39988289752.png)
  我们只要在递归之前做出选择，在递归之后撤销刚才的选择，就能正确得到每个节点的选择列表和路径。
  ```
  for 选择 in 选择列表:
    # 做选择
    将该选择从选择列表移除
    路径.add(选择)
    backtrack(路径, 选择列表)
    # 撤销选择
    路径.remove(选择)
    将该选择再加入选择列表

  ```
  我们只要在递归之前做出选择，在递归之后撤销刚才的选择，就能正确得到每个节点的选择列表和路径。
  
**Solutions:**
DFS:
```
class Solution:

    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:

        path = []
        res = []
        
        self.TreePath(root,path,res)
        return res
    
    # 1. path: record in Track []
    # 2. options: left , right node
    # 3. end condition: reach leaf

    def TreePath(self, root,path,res):
        if not root:
            return
        
        if not root.left and not root.right:
            path.append(str(root.val))
            res.append('->'.join(path))
            path.pop()
            return
        
        path.append(str(root.val))
        self.TreePath(root.left,path,res)
        self.TreePath(root.right,path,res)
        path.pop()
```

**Time Complexity:** O(n)

**Space Complexity:** O(n)


## 404. Sum of Left Leaves ##

**Link:** [404. Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/description/)

**Thoughts:**
  - BFS: Find left leaf
  - DFS: 遍历一遍二叉树，然后找到那些左叶子节点，累加它们的值

**Solution:**
DFS:
```
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        
        self.traverse(root)
        return self.leftsum
    
    leftsum = 0

    def traverse(self, root):
        if not root:
            return
        
        if root.left and not root.left.left and not root.left.right:
            self.leftsum += root.left.val
        
        self.traverse(root.left)
        self.traverse(root.right)
```

**Time Complexity:** O(n)

**Space Complexity:** O(n)

BFS:
```
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        q = collections.deque()
        q.append(root)
        leftsum = 0
        while q:
            size = len(q)
            for i in range(size):
                cur = q.popleft()

                if cur.left:
                    q.append(cur.left)
                    if not cur.left.left and not cur.left.right:
                        leftsum += cur.left.val
                if cur.right:
                    q.append(cur.right)
        return leftsum
```

**Time Complexity:** O(n)

**Space Complexity:** O(n)
