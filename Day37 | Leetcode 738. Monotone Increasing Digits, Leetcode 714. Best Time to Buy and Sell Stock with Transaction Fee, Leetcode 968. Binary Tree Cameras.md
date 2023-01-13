## 738. Monotone Increasing Digits ##

**Link:**[738. Monotone Increasing Digits](https://leetcode.com/problems/monotone-increasing-digits/description/)

**Thoughts:**
  - 一旦出现strNum[i - 1] > strNum[i]的情况（非单调递增），首先想让strNum[i - 1]--，然后strNum[i]给为9，这样这个整数就是89，即小于98的最大的单调递增整数。
  - 局部最优：遇到strNum[i - 1] > strNum[i]的情况，让strNum[i - 1]--，然后strNum[i]给为9，可以保证这两位变成最大单调递增整数。
  - 全局最优：得到小于等于N的最大单调递增的整数。
  - 从后向前遍历，就可以重复利用上次比较得出的结果

**Solutions**
```
class Solution:
    def monotoneIncreasingDigits(self, n: int) -> int:
        nums = list(str(n))
        print(nums)

        for i in range(len(nums) - 1, 0, -1):
            if int(nums[i]) < int(nums[i-1]):
                nums[i-1] = str(int(nums[i-1]) - 1)
                nums[i:] = '9'*(len(nums) - i)
        
        return int(''.join(nums))
```
时间复杂度：O(n)，n 为数字长度
空间复杂度：O(n)，需要一个字符串，转化为字符串操作更方便


## 714. Best Time to Buy and Sell Stock with Transaction Fee ##

**Link:**[714. Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/)

**Thoughts:**
  - https://labuladong.github.io/algo/di-er-zhan-a01c6/yong-dong--63ceb/yi-ge-fang-3b01b/

**Solutions**
```
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:

        n = len(prices)
        # base case:
        dp = [[0,0] for _ in range(n)]
        
        dp[0][0] = 0
        dp[0][1] = -prices[0]

        # function:
        for i in range(1,n):
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i] - fee)

            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])

        # res:

        return dp[n-1][0]
```
时间复杂度：O(n)
空间复杂度：O(n)


## 968. Binary Tree Cameras ##

**Link:**[968. Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/description/)

**Thoughts:**
  - 要从下往上看，局部最优：让叶子节点的父节点安摄像头，所用摄像头最少，整体最优：全部摄像头数量所用最少
  - 在二叉树中如何从低向上推导呢？
    可以使用后序遍历也就是左右中的顺序，这样就可以在回溯的过程中从下到上进行推导了。
  - 如何隔两个节点放一个摄像头
    此时需要状态转移的公式，大家不要和动态的状态转移公式混到一起，本题状态转移没有择优的过程，就是单纯的状态转移！

    来看看这个状态应该如何转移，先来看看每个节点可能有几种状态：

    有如下三种：

    该节点无覆盖
    本节点有摄像头
    本节点有覆盖
    我们分别有三个数字来表示：

    0：该节点无覆盖
    1：本节点有摄像头
    2：本节点有覆盖
    
    主要有如下四类情况：

    情况1：左右节点都有覆盖
    左孩子有覆盖，右孩子有覆盖，那么此时中间节点应该就是无覆盖的状态了。
    
    情况2：左右节点至少有一个无覆盖的情况
    此时摄像头的数量要加一，并且return 1，代表中间节点放摄像头。
    
    情况3：左右节点至少有一个有摄像头
    其实就是 左右孩子节点有一个有摄像头了，那么其父节点就应该是2（覆盖的状态）
    
    情况4：头结点没有覆盖
    所以递归结束之后，还要判断根节点，如果没有覆盖，result++
    
**Solutions**
```
class Solution:
    def minCameraCover(self, root: Optional[TreeNode]) -> int:
         # Greedy Algo:
        # 从下往上安装摄像头：跳过leaves这样安装数量最少，局部最优 -> 全局最优
        # 先给leaves的父节点安装，然后每隔两层节点安装一个摄像头，直到Head
        # 0: 该节点未覆盖
        # 1: 该节点有摄像头
        # 2: 该节点有覆盖
        res = 0

        # 从下往上遍历：后序（左右中）
        def traveral(root):

        # Definition and Usage
        # The nonlocal keyword is used to work with variables inside nested functions, where the variable should not belong to the inner function.
        # Use the keyword nonlocal to declare that the variable is not local.
            nonlocal res

            if not root:
                return 2

            left = traveral(root.left)
            right = traveral(root.right)

            # Case 1:
            # 左右节点都有覆盖
            if left == 2 and right == 2:
                return 0

            # Case 2: 左右节点有没覆盖的
                # left == 0 && right == 0 左右节点无覆盖
                # left == 1 && right == 0 左节点有摄像头，右节点无覆盖
                # left == 0 && right == 1 左节点有无覆盖，右节点摄像头
                # left == 0 && right == 2 左节点无覆盖，右节点覆盖
                # left == 2 && right == 0 左节点覆盖，右节点无覆盖
            elif left == 0 or right == 0:
                res += 1
                return 1

             # Case 3:
                # left == 1 && right == 2 左节点有摄像头，右节点有覆盖
                # left == 2 && right == 1 左节点有覆盖，右节点有摄像头
                # left == 1 && right == 1 左右节点都有摄像头
            
            elif left == 1 or right == 1:
                return 2
            
             # 其他情况前段代码均已覆盖

        if traveral(root) == 0:
            res += 1
        
        return res
```
