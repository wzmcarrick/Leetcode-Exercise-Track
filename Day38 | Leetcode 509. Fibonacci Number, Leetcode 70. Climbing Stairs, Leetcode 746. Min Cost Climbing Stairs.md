## 509. Fibonacci Number ##

**Link: **[509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/description/)

**Thoughts:**
  - 对于动态规划问题，我将拆解为如下五步曲，这五步都搞清楚了，才能说把动态规划真的掌握了！
    1. 确定dp数组（dp table）以及下标的含义
    2. 确定递推公式
    3. dp数组如何初始化
    4. 确定遍历顺序
    5. 举例推导dp数组

**Solutions:**
```
class Solution:
    def fib(self, n: int) -> int:

        if n <= 1:
            return 0 if n == 0 else 1

        dp = [0] * (n+1)
        dp[0] = 0
        dp[1] = 1
        for i in range(2,n+1):
            dp[i] = dp[i-1] + dp[i-2]
        return dp[n]
```
时间复杂度：O(n)
空间复杂度：O(n)

## 70. Climbing Stairs ##

**Link: **[70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)

**Thoughts:**
  - 确定dp数组以及下标的含义
    dp[i]： 爬到第i层楼梯，有dp[i]种方法

    确定递推公式
    如何可以推出dp[i]呢？

    从dp[i]的定义可以看出，dp[i] 可以有两个方向推出来。

    首先是dp[i - 1]，上i-1层楼梯，有dp[i - 1]种方法，那么再一步跳一个台阶不就是dp[i]了么。

    还有就是dp[i - 2]，上i-2层楼梯，有dp[i - 2]种方法，那么再一步跳两个台阶不就是dp[i]了么。

    那么dp[i]就是 dp[i - 1]与dp[i - 2]之和！

    所以dp[i] = dp[i - 1] + dp[i - 2] 。

    在推导dp[i]的时候，一定要时刻想着dp[i]的定义，否则容易跑偏。

    这体现出确定dp数组以及下标的含义的重要性！

    dp数组如何初始化
    在回顾一下dp[i]的定义：爬到第i层楼梯，有dp[i]中方法。

    那么i为0，dp[i]应该是多少呢，这个可以有很多解释，但基本都是直接奔着答案去解释的。

    例如强行安慰自己爬到第0层，也有一种方法，什么都不做也就是一种方法即：dp[0] = 1，相当于直接站在楼顶。

    但总有点牵强的成分。

    那还这么理解呢：我就认为跑到第0层，方法就是0啊，一步只能走一个台阶或者两个台阶，然而楼层是0，直接站楼顶上了，就是不用方法，dp[0]就应该是0.

    其实这么争论下去没有意义，大部分解释说dp[0]应该为1的理由其实是因为dp[0]=1的话在递推的过程中i从2开始遍历本题就能过，然后就往结果上靠去解释dp[0] = 1。

    从dp数组定义的角度上来说，dp[0] = 0 也能说得通。

    需要注意的是：题目中说了n是一个正整数，题目根本就没说n有为0的情况。

    所以本题其实就不应该讨论dp[0]的初始化！

    我相信dp[1] = 1，dp[2] = 2，这个初始化大家应该都没有争议的。

    所以我的原则是：不考虑dp[0]如何初始化，只初始化dp[1] = 1，dp[2] = 2，然后从i = 3开始递推，这样才符合dp[i]的定义。

    确定遍历顺序
    从递推公式dp[i] = dp[i - 1] + dp[i - 2];中可以看出，遍历顺序一定是从前向后遍历的

**Solutions:**
```
class Solution:
    def climbStairs(self, n: int) -> int:

        if n <= 2:
            return 1 if n == 1 else 2
        
        dp = [0] * (n+1)

        dp[1] = 1
        dp[2] = 2

        for i in range(3,n+1):

            dp[i] = dp[i-1] + dp[i-2]

        return dp[n]
```
时间复杂度：$O(n)$
空间复杂度：$O(n)$

## 746. Min Cost Climbing Stairs ##

**Link: **[746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/description/)

**Thoughts:**
  - dp[i]的定义：到达第i台阶所花费的最少体力为dp[i]。
  - 可以有两个途径得到dp[i]，一个是dp[i-1] 一个是dp[i-2]。

    dp[i - 1] 跳到 dp[i] 需要花费 dp[i - 1] + cost[i - 1]。

    dp[i - 2] 跳到 dp[i] 需要花费 dp[i - 2] + cost[i - 2]。

    那么究竟是选从dp[i - 1]跳还是从dp[i - 2]跳呢？

    一定是选最小的，所以dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);

**Solutions:**
```
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        dp = [0] * (len(cost) + 1)
        
        dp[0] = 0
        dp[1] = 0

        for i in range(2, len(cost) + 1):
            dp[i] = min(dp[i-2] + cost[i-2], dp[i-1] + cost[i-1])

        return dp[len(cost)]
```
时间复杂度：O(n)
空间复杂度：O(n)
