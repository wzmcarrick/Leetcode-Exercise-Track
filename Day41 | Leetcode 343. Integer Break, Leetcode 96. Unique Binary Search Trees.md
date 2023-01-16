## 343. Integer Break ##

**Link:**[343. Integer Break](https://leetcode.com/problems/integer-break/description/)

**Thoughts:**
  - 确定dp数组（dp table）以及下标的含义: dp[i]：分拆数字i，可以得到的最大乘积为dp[i]。
  - 确定递推公式: 可以从1遍历j，然后有两种渠道得到dp[i]. 一个是j * (i - j) 直接相乘。一个是j * dp[i - j]，相当于是拆分(i - j)，对这个拆分不理解的话，可以回想dp数组的定义。
    dp[i] = max(dp[i], max((i - j) * j, dp[i - j] * j))
  - dp的初始化: dp[2] = 1，从dp[i]的定义来说，拆分数字2，得到的最大乘积是1
  - 确定遍历顺序: dp[i] 是依靠 dp[i - j]的状态，所以遍历i一定是从前向后遍历，先有dp[i - j]再有dp[i]
  - (因为拆分一个数n 使之乘积最大，那么一定是拆分成m个近似相同的子数相乘才是最大)

**Solution:**
```
class Solution:
    def integerBreak(self, n: int) -> int:
        dp = [0] * (n+1)
        dp[2] = 1
         # 假设对正整数 i 拆分出的第一个正整数是 j（1 <= j < i），则有以下两种方案：
            # 1) 将 i 拆分成 j 和 i−j 的和，且 i−j 不再拆分成多个正整数，此时的乘积是 j * (i-j)
            # 2) 将 i 拆分成 j 和 i−j 的和，且 i−j 继续拆分成多个正整数，此时的乘积是 j * dp[i-j]
        for i in range(3, n+1):
            for j in range(1,(i//2)+1):
                dp[i] = max(dp[i], max((i-j)*j, dp[i-j]*j))

        return dp[n]
```
时间复杂度：O(n^2)
空间复杂度：O(n)

## 96. Unique Binary Search Trees ##

**Link:**[96. Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/description/)

**Thoughts:**
  - 确定dp数组（dp table）以及下标的含义: dp[i] ： 1到i为节点组成的二叉搜索树的个数为dp[i]。也可以理解是i个不同元素节点组成的二叉搜索树的个数为dp[i]
  - 确定递推公式: dp[i] += dp[以j为头结点左子树节点数量] * dp[以j为头结点右子树节点数量], j相当于是头结点的元素，从1遍历到i为止。
    dp[i] += dp[j - 1] * dp[i - j]; ，j-1 为j为头结点左子树节点数量，i-j 为以j为头结点右子树节点数量
  - dp的初始化: 初始化，只需要初始化dp[0]就可以了，推导的基础，都是dp[0]。从定义上来讲，空节点也是一棵二叉树，也是一棵二叉搜索树，这是可以说得通的
    dp[0] = 1
  - 确定遍历顺序: 首先一定是遍历节点数，从递归公式：dp[i] += dp[j - 1] * dp[i - j]可以看出，节点数为i的状态是依靠 i之前节点数的状态。那么遍历i里面每一个数作为头结点的状态，用j来遍历。

**Solution:**
```
class Solution:
    def numTrees(self, n: int) -> int:
        dp = [0] * (n+1)

        dp[0] = 1

        for i in range(1,n+1):
            for j in range(0, i):
                dp[i] += dp[j]*dp[i-1-j]
        return dp[n]
```
时间复杂度：O(n^2)
空间复杂度：O(n)
