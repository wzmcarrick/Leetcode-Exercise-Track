## 1049. Last Stone Weight II ##

**Link:**[1049. Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/description/)

**Thought:**
  - 本题其实就是尽量让石头分成重量相同的两堆，相撞之后剩下的石头最小，这样就化解成01背包问题了
  - 确定dp数组以及下标的含义:
    - dp[j]表示容量（这里说容量更形象，其实就是重量）为j的背包，最多可以背最大重量为dp[j]。
  - 确定递推公式:
    - 本题则是：dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);
  - dp数组如何初始化:
    - dp[j]都初始化为0就可以了
  - 确定遍历顺序：
    - 在动态规划：关于01背包问题，你该了解这些！（滚动数组） (opens new window)中就已经说明：如果使用一维dp数组，物品遍历的for循环放在外层，遍历背包的for循环放在内层，且内层for循环倒序遍历！
  - 最后dp[target]里是容量为target的背包所能背的最大重量。
    那么分成两堆石头，一堆石头的总重量是dp[target]，另一堆就是sum - dp[target]。
    在计算target的时候，target = sum / 2 因为是向下取整，所以sum - dp[target] 一定是大于等于dp[target]的。
    那么相撞之后剩下的最小石头重量就是 (sum - dp[target]) - dp[target]。
    
**Solution:**
```
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        totalSum = sum(stones)

        dp = [0] * (totalSum//2 + 1)

        for i in range(len(stones)):
            for j in range(totalSum//2, stones[i] - 1, -1):

                dp[j] = max(dp[j], dp[j-stones[i]] + stones[i])
        # 那么分成两堆石头，一堆石头的总重量是dp[target]，另一堆就是sum - dp[target]。
        return totalSum - 2*dp[totalSum//2]
```
时间复杂度：O(m × n) , m是石头总重量（准确的说是总重量的一半），n为石头块数
空间复杂度：O(m)

## 494. Target Sum ##

**Link:**[494. Target Sum](https://leetcode.com/problems/target-sum/description/)

**Thought:**
  - 假设加法的总和为x，那么减法对应的总和就是sum - x。
  - 所以我们要求的是 x - (sum - x) = target 
  - x = (target + sum) / 2
  - 此时问题就转化为，装满容量为x的背包，有几种方法。
  - 这里的x，就是bagSize，也就是我们后面要求的背包容量。
  - 这次和之前遇到的背包问题不一样了，之前都是求容量为j的背包，最多能装多少。本题则是装满有几种方法。其实这就是一个组合问题了
  
  - 确定dp数组以及下标的含义:
    - dp[j] 表示：填满j（包括j）这么大容积的包，有dp[j]种方法
  - 确定递推公式:
    - 只要搞到nums[i]），凑成dp[j]就有dp[j - nums[i]] 种方法。
    - dp[j] += dp[j - nums[i]]
  - dp数组如何初始化:
    -  dp[0] 为 1
  - 确定遍历顺序：
    - 在动态规划：关于01背包问题，你该了解这些！（滚动数组） (opens new window)中就已经说明：如果使用一维dp数组，物品遍历的for循环放在外层，遍历背包的for循环放在内层，且内层for循环倒序遍历！
  - 举例推导dp数组:
    - 输入：nums: [1, 1, 1, 1, 1], S: 3
    - bagSize = (S + sum) / 2 = (3 + 5) / 2 = 4
    - ![image](https://user-images.githubusercontent.com/69004164/213299716-97ed7136-6ceb-4f95-89d7-5b282e2b61f2.png)

**Solution:**
```
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        # assume the positve part is x, then the negative part of nums is totalSum - x
        # what we want: x - (totalSum -x) = target
        # x = (totalSum + target) // 2

        # find how many kind of combination that the sum is x:
        # dp[j] means the number of combinations that the sum equals to j
        # j between 0 and nums[i]
        # dp[j] += dp[j-nums[i]]

        totalSum = sum(nums)

        x = (totalSum + target) // 2

        if abs(target) > totalSum or (totalSum + target) % 2 == 1: return 0

        dp = [0] * (x + 1)
        dp[0] = 1

        for i in range(len(nums)):
            for j in range(x, nums[i] - 1, -1):
                dp[j] += dp[j - nums[i]]

        return dp[x]
```
时间复杂度：O(n × m)，n为正数个数，m为背包容量
空间复杂度：O(m)，m为背包容量

## 474. Ones and Zeroes ##

**Link:**[474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/description/)

**Thought:**
  - 本题中strs 数组里的元素就是物品，每个物品都是一个！而m 和 n相当于是一个背包，两个维度的背包。
  - 理解成多重背包的同学主要是把m和n混淆为物品了，感觉这是不同数量的物品，所以以为是多重背包。
  - 但本题其实是01背包问题！
  - 只不过这个背包有两个维度，一个是m 一个是n，而不同长度的字符串就是不同大小的待装物品。
  
  - 确定dp数组以及下标的含义:
    - dp[i][j]：最多有i个0和j个1的strs的最大子集的大小为dp[i][j]。
  - 确定递推公式:
    - dp[i][j] 可以由前一个strs里的字符串推导出来，strs里的字符串有zeroNum个0，oneNum个1。
    - dp[i][j] 就可以是 dp[i - zeroNum][j - oneNum] + 1。
    - 然后我们在遍历的过程中，取dp[i][j]的最大值。
    - 所以递推公式：dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
  - dp数组如何初始化:
    -  初始为0，保证递推的时候dp[i][j]不会被初始值覆盖
  - 确定遍历顺序：
    - 在动态规划：关于01背包问题，你该了解这些！（滚动数组） (opens new window)中就已经说明：如果使用一维dp数组，物品遍历的for循环放在外层，遍历背包的for循环放在内层，且内层for循环倒序遍历！
    - 那么本题也是，物品就是strs里的字符串，背包容量就是题目描述中的m和n
  - 举例推导dp数组:
    - 以输入：["10","0001","111001","1","0"]，m = 3，n = 3为例
    - 最后dp数组的状态如下所示：
    -![image](https://user-images.githubusercontent.com/69004164/213300500-5230b5ea-70bb-452b-a586-eca42b43a2c9.png)

**Solution:**
```
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        # dp[i][j] the largest subset that at most i 0's and j 1's
        # i between m and strZero
        # j between n and strOne
        # dp[i][j] = max(dp[i][j], dp[i - strZero][j - strOne] + 1)

        dp = [[0 for _ in range(n+1)] for _ in range(m+1)]

        for s in strs:
            strOne = s.count('1')
            strZero = s.count('0')
            
            for i in range(m, strZero - 1, -1):
                for j in range(n, strOne - 1, -1):
                    dp[i][j] = max(dp[i][j], dp[i - strZero][j - strOne] + 1)
        return dp[m][n]
```
