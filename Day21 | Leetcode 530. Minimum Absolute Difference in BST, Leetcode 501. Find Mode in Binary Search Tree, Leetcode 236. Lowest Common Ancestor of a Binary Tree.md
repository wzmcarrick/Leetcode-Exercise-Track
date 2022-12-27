## 530. Minimum Absolute Difference in BST ##

**Link **[530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/)

**Thoughts**
  - Method 1: 把二叉搜索树转换成有序数组，然后遍历一遍数组，就统计出来最小差值了
  - Method 2: 在二叉搜素树中序遍历的过程中，我们就可以直接计算了。需要用一个pre节点记录一下cur节点的前一个节点。

**Solution**
Method 1:
```
inorder = []

        def traverse(root):
            if not root:
                return
            
            traverse(root.left)
            inorder.append(root.val)
            traverse(root.right)
        
        traverse(root)
        minDiff = float('inf')
        for i in range(len(inorder)):
            if i > 0:
                minDiff = min(minDiff, inorder[i] - inorder[i-1])
        return minDiff
```

Method 2:
```
class Solution:

    def __init__(self):
        self.prev = TreeNode(None)
        self.minDiff = float('inf')

    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        self.traverse(root)
        return self.minDiff

    def traverse(self, root):
        if not root:
            return
        
        self.traverse(root.left)
        if self.prev.val is not None:
            self.minDiff = min(self.minDiff, root.val - self.prev.val)
        self.prev = root
       
        self.traverse(root.right)
```

## 501. Find Mode in Binary Search Tree ##

**Link **[501. Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/)

**Thoughts**
  - Step 1: 弄一个指针指向前一个节点，这样每次cur（当前节点）才能和pre（前一个节点）作比较。而且初始化的时候pre = NULL，这样当pre为NULL时候，我们就知道这是比较的第一个元素。
  - Step 2: 如果 频率count 等于 maxCount（最大频率），当然要把这个元素加入到结果集中（以下代码为result数组）
  - Step 3: 频率count 大于 maxCount的时候，不仅要更新maxCount，而且要清空结果集（以下代码为result数组），因为结果集之前的元素都失效了。

**Solution**
```
class Solution:
    def __init__(self):
        self.pre = TreeNode()
        self.count = 0
        self.max_count = 0
        self.result = []
        
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        if not root: return None
        self.search_BST(root)
        return self.result

    def search_BST(self, cur: TreeNode) -> None:
        if not cur: return None
        self.search_BST(cur.left)
        # 第一个节点
        if not self.pre:
            self.count = 1
        # 与前一个节点数值相同
        elif self.pre.val == cur.val:
            self.count += 1 
        # 与前一个节点数值不相同
        else:
            self.count = 1
        self.pre = cur

        if self.count == self.max_count:
            self.result.append(cur.val)
        
        if self.count > self.max_count:
            self.max_count = self.count
            self.result = [cur.val]	# 清空self.result，确保result之前的的元素都失效
        
        self.search_BST(cur.right)
```

## 236. Lowest Common Ancestor of a Binary Tree ##

**Link **[236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)

**Thoughts**
  - 解题思路：每个节点要知道什么、做什么：什么时候做
    遍历or递归
    要知道自己的子树里是否有这两个数字->递归
    要做什么：返回自己子树是否有这两个数字->递归
    什么时候做：后序遍历，传递子树信息

    自下而上，这个函数就返回自己左右子树满足条件的node：返回自己或者不为None的一边。base case就是找到了
    如果一个节点能够在它的左右子树中分别找到 p 和 q，则该节点为 LCA 节点。

    时间：O(N)
    空间：O(N)

**Solution**
```
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        """
        解题思路：每个节点要知道什么、做什么：什么时候做
        遍历or递归
        要知道自己的子树里是否有这两个数字->递归
        要做什么：返回自己子树是否有这两个数字->递归
        什么时候做：后序遍历，传递子树信息

        自下而上，这个函数就返回自己左右子树满足条件的node：返回自己或者不为None的一边。base case就是找到了
        如果一个节点能够在它的左右子树中分别找到 p 和 q，则该节点为 LCA 节点。

        时间：O(N)
        空间：O(N)
        """
        if root is None: # base case
            return root
        
        if root == p or root == q: # Case 1：公共祖先就是我自己，也可以放在前序位置（要确保p,q在树中）
            return root

        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        
        # 后序遍历
        
        if left and right: # Case 2：自己子树包含这两个数
            return root
        else:
            return left or right # Case 3：
```
