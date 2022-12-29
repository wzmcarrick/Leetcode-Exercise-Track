## 669. Trim a Binary Search Tree ## 

**Link: **[669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/description/)

**Thoughts:**
  - 明确了递归函数的定义之后进行思考，如果一个节点的值没有落在 [lo, hi] 中，有两种情况：

  1、**root.val < lo，这种情况下 root 节点本身和 root 的左子树全都是小于 lo 的，都需要被剪掉**。

  2、**root.val > hi，这种情况下 root 节点本身和 root 的右子树全都是大于 hi 的，都需要被剪掉**。
  
**Solution:**
```
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        # base case
        if not root:
            return root
        # do something:# 单层递归逻辑
        if root.val < low:
            # 若当前root节点小于左界：只考虑其右子树，用于替代更新后的其本身，抛弃其左子树整体
            return self.trimBST(root.right, low, high)
        
        if root.val > high:
            # 若当前root节点大于右界：只考虑其左子树，用于替代更新后的其本身，抛弃其右子树整体
            return self.trimBST(root.left, low, high)
        
        root.left = self.trimBST(root.left, low, high)
        root.right = self.trimBST(root.right, low, high)
        # 返回更新后的剪枝过的当前节点root
        return root
```
     
## 108. Convert Sorted Array to Binary Search Tree ## 

**Link: **[108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)

**Thoughts:**
  - 二叉树的构建问题很简单，说白了就是：构造根节点，然后构建左右子树。一个有序数组对于 BST 来说就是中序遍历结果，根节点在数组中心，数组左侧是左子树元素，右侧是右子树元素。
  
**Solution:**
```
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        if len(nums) < 1:
            return None
        mid = (0 + len(nums) - 1) // 2
        left = nums[0: mid]
        right = nums[mid+1:]
        root = TreeNode(nums[mid])
        root.left = self.sortedArrayToBST(left)
        root.right = self.sortedArrayToBST(right)

        return root
```

## 538. Convert BST to Greater Tree ## 

**Link: **[538. Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/description/)

**Thoughts:**
  - 这道题需要用到「遍历」的思维。维护一个外部累加变量 sum，在遍历 BST 的过程中增加 sum，同时把 sum 赋值给 BST 中的每一个节点，就将 BST 转化成累加树了。但是注意顺序，正常的中序遍历顺序是先左子树后右子树，这里需要反过来，先右子树后左子树。
  
**Solution:**
```
class Solution:
    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        self.traverse(root)
        return root
    
    summ = 0
    def traverse(self, root):
        if not root:
            return root
        self.traverse(root.right)
        self.summ += root.val
        root.val = self.summ
        self.traverse(root.left)
```
