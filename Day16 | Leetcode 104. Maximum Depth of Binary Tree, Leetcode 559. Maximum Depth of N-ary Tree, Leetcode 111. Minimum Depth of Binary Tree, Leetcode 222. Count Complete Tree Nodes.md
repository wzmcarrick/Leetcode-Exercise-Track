## 104. Maximum Depth of Binary Tree ##

**Link: **[104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)

**Thoughts**
  - DFS: Given a node, the max depth of that node is the max of the max depth of left node and right node, then plus one
  - BFS

**Solutions:**
DFS:
```
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
```
**Time Compelexity: ** O(n)

**Space Complexity: ** in the worst case, the tree is completely unbalanced, e.g. each node has only left child node, the recursion call would occur NNN times (the height of the tree), therefore the storage to keep the call stack would be O(N)\mathcal{O}(N)O(N). But in the best case (the tree is completely balanced), the height of the tree would be log⁡(N)\log(N)log(N). Therefore, the space complexity in this case would be O(log⁡(N))\mathcal{O}(\log(N))O(log(N)).

BFS:
```
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        q = collections.deque()
        maxDepth = 0

        q.append(root)

        while q:
            size = len(q)
            maxDepth += 1
            for i in range(size):
                cur = q.popleft()

                if cur.left:
                    q.append(cur.left)
                if cur.right:
                    q.append(cur.right)
            
        
        return maxDepth
```

**Time Compelexity: ** O(n)

**Space Complexity: ** O(n)


## 559. Maximum Depth of N-ary Tree ##

**Link: **[559. Maximum Depth of N-ary Tree](https://leetcode.com/problems/maximum-depth-of-n-ary-tree/description/)

**Thoughts**
  - DFS: Given a node, the max depth of that node is the max of the max depth of children nodes, then plus one
  - BFS

**Solutions:**
DFS:
```
class Solution:
    def maxDepth(self, root: 'Node') -> int:
        if not root:
            return 0
        maxDep = 0
        for child in root.children:
            maxDep = max(maxDep, self.maxDepth(child))
        return maxDep + 1
```
**Time Compelexity: ** O(n)

**Space Complexity: **  in the worst case, the tree is completely unbalanced, e.g. each node has only one child node, the recursion call would occur NNN times (the height of the tree), therefore the storage to keep the call stack would be O(N)\mathcal{O}(N)O(N). But in the best case (the tree is completely balanced), the height of the tree would be log⁡(N)\log(N)log(N). Therefore, the space complexity in this case would be O(log⁡(N))\mathcal{O}(\log(N))O(log(N)).

BFS:
```
class Solution:
    def maxDepth(self, root: 'Node') -> int:
        if not root:
            return 0
        
        q = collections.deque()
        dep = 0
        q.append(root)

        while q:
            size = len(q)
            dep += 1
            for i in range(size):
                cur = q.popleft()
                
                for child in cur.children:
                    if child:
                        q.append(child)
        
        return dep
```
**Time Compelexity: ** O(n)

**Space Complexity: ** O(n)


## 111. Minimum Depth of Binary Tree ##

**Link: **[111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)

**Thoughts**
  - DFS: 所以，如果左子树为空，右子树不为空，说明最小深度是 1 + 右子树的深度。反之，右子树为空，左子树不为空，最小深度是 1 + 左子树的深度。 最后如果左右子树都不为空，返回左右子树深度最小值 + 1 。
  ![image](https://user-images.githubusercontent.com/69004164/209228112-70725faf-db40-49b2-abd6-6f8455b901ae.png)
  求二叉树的最小深度和求二叉树的最大深度的差别主要在于处理左右孩子不为空的逻辑。
  - BFS: BFS 算法和 DFS（回溯）算法的一大区别就是，BFS 第一次搜索到的结果是最优的

**Solutions:**
DFS: 
```
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        if root.left and not root.right:
            return self.minDepth(root.left) + 1
        
        if root.right and not root.left:
            return self.minDepth(root.right) + 1
        
        return min(self.minDepth(root.left), self.minDepth(root.right)) + 1
```
**Time Compelexity: ** O(n)

**Space Complexity: ** O(n)

BFS:
```
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        q = collections.deque()
        res = 0
        q.append(root)

        while q:
            size = len(q)
            res += 1
            for i in range(size):
                cur = q.popleft()

                if not cur.left and not cur.right:
                    return res
                
                if cur.left:
                    q.append(cur.left)
                if cur.right:
                    q.append(cur.right)
        return res
```
**Time Compelexity: ** O(n)

**Space Complexity: ** O(n)


## 222. Count Complete Tree Nodes ##

**Links**:[222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/description/)

**Thoughts:**
  - DFS: Given a node, the tree node number is the sum of left subnode number and right subnode number, then plus one
  - BFS:

DFS:
```
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        return self.countNodes(root.left) + self.countNodes(root.right) + 1
```
**Time Compelexity: ** O(n)

**Space Complexity: ** O(n)

BFS:
```
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        q = collections.deque()
        q.append(root)
        res = 0

        while q:
            size = len(q)
            for i in range(size):
                cur = q.popleft()
                res += 1
                if cur.left:
                    q.append(cur.left)
                if cur.right:
                    q.append(cur.right)
        return res
```
**Time Compelexity: ** O(n)

**Space Complexity: ** O(n)
















