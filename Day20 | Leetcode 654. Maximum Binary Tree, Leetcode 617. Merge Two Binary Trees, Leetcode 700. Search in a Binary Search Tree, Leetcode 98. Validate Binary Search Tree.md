## 654. Maximum Binary Tree ##

**Link **[654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

**Thoughts**
  - DFS: create a binary tree
  - Find the root, then create left and right tree

**Solutions:**
```
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        
        if not nums:
            return None

        maxVal = 0
        for i in range(len(nums)):
            maxVal = max(nums[i], maxVal)
        root = TreeNode(maxVal)
        mid = nums.index(maxVal)
        
        left = nums[:mid]
        right = nums[mid+1:]

        root.left = self.constructMaximumBinaryTree(left)
        root.right = self.constructMaximumBinaryTree(right)

        return root
```

## 617. Merge Two Binary Trees ##

**Link **[617. Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/description/)

**Thoughts**
  - DFS: 
 

**Solutions:**
```
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root1:
            return root2
        if not root2:
            return root1
        
        root1.val += root2.val
        root1.left = self.mergeTrees(root1.left, root2.left)
        root1.right = self.mergeTrees(root1.right, root2.right)

        return root1
```

## 700. Search in a Binary Search Tree ##

**Link **[700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/description/)

**Thoughts** 
  - 递归法
  1. 确定递归函数的参数和返回值
    递归函数的参数传入的就是根节点和要搜索的数值，返回的就是以这个搜索数值所在的节点。

  代码如下：

  TreeNode* searchBST(TreeNode* root, int val)
  2. 确定终止条件
    如果root为空，或者找到这个数值了，就返回root节点。

  if (root == NULL || root->val == val) return root;
  3. 确定单层递归的逻辑
    看看二叉搜索树的单层递归逻辑有何不同。

    因为二叉搜索树的节点是有序的，所以可以有方向的去搜索。

    如果root->val > val，搜索左子树，如果root->val < val，就搜索右子树，最后如果都没有搜索到，就返回NULL。

  代码如下：

  TreeNode* result = NULL;
  if (root->val > val) result = searchBST(root->left, val);
  if (root->val < val) result = searchBST(root->right, val);
  return result;
    很多录友写递归函数的时候 习惯直接写 searchBST(root->left, val)，却忘了 递归函数还有返回值。

    递归函数的返回值是什么? 是 左子树如果搜索到了val，要将该节点返回。 如果不用一个变量将其接住，那么返回值不就没了。

所以要 result = searchBST(root->left, val)。
  
  - BFS
 

**Solutions:**
DFS:
```
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root1:
            return root2
        if not root2:
            return root1
        
        root1.val += root2.val
        root1.left = self.mergeTrees(root1.left, root2.left)
        root1.right = self.mergeTrees(root1.right, root2.right)

        return root1
```
BFS:
```
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:

        while root:
            cur = root

            if cur.val == val:
                return cur
            
            if cur.val > val:
                root = cur.left
            
            if cur.val < val:
                root = cur.right
        return None
```

## 98. Validate Binary Search Tree ##

**Link **[98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/)

**Thoughts**
  - DFS: 因为 BST 左小右大的特性是指 root.val 要比左子树的所有节点都更大，要比右子树的所有节点都小，你只检查左右两个子节点当然是不够的。正确解法是通过使用辅助函数，增加函数参数列表，在参数中携带额外信息，将这种约束传递给子树的所有节点
  - Another solution: Inorder Traversal of BST is sorted

**Solutions:**
DFS:
```
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        return self.isValid(root,None,None)
    
    def isValid(self, root, minRoot, maxRoot):
        if not root:
            return True
        if minRoot and root.val <= minRoot.val:
            return False
        if maxRoot and root.val >= maxRoot.val:
            return False
        
        return self.isValid(root.left, minRoot, root) and self.isValid(root.right,root,maxRoot)
```

Another DFS:
```
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        res = []
        def inorder(root):
            if not root:
                return
            
            inorder(root.left)
            res.append(root.val)
            inorder(root.right)
        
        inorder(root)
        
        for i in range(len(res)):
            if i > 0 and res[i] <= res[i-1]:
                return False
        return True
```
