## 416. Partition Equal Subset Sum ##

**Link:**[416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/description/)

**Thoughts:**
  - 背包的体积为sum / 2
  - 背包要放入的商品（集合里的元素）重量为 元素的数值，价值也为元素的数值
  - 背包如果正好装满，说明找到了总和为 sum / 2 的子集。
  - 背包中每一个元素是不可重复放入。
  
  - 确定dp数组以及下标的含义:
    01背包中，dp[j] 表示： 容量为j的背包，所背的物品价值最大可以为dp[j]。
    本题中每一个元素的数值既是重量，也是价值。
    套到本题，dp[j]表示 背包总容量（所能装的总重量）是j，放进物品后，背的最大重量为dp[j]。
    那么如果背包容量为target， dp[target]就是装满 背包之后的重量，所以 当 dp[target] == target 的时候，背包就装满了。
  - 确定递推公式:
    01背包的递推公式为：dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    本题，相当于背包里放入数值，那么物品i的重量是nums[i]，其价值也是nums[i]。
    所以递推公式：dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]);
  - dp数组如何初始化:
    在01背包，一维dp如何初始化，已经讲过，
    从dp[j]的定义来看，首先dp[0]一定是0。
  - 确定遍历顺序:
    在动态规划：[关于01背包问题，你该了解这些！（滚动数组）](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-2.html#%E4%B8%80%E7%BB%B4dp%E6%95%B0%E7%BB%84-%E6%BB%9A%E5%8A%A8%E6%95%B0%E7%BB%84)中就已经说明：如果使用一维dp数组，物品遍历的for循环放在外层，遍历背包的for循环放在内层，且内层for循环倒序遍历！

**Solution**
```
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        totalSum = sum(nums)

        if totalSum % 2 == 1:
            return False
        
        dp = [0]* (totalSum // 2 + 1)

        dp[0] = 0

        for i in range(0,len(nums)):
            for j in range(totalSum//2, nums[i] - 1, -1):
                dp[j] = max(dp[j], dp[j-nums[i]] + nums[i])

        if dp[totalSum//2] == totalSum//2:
            return True
        else:
            return False
```
时间复杂度：O(n^2)
空间复杂度：O(n)，虽然dp数组大小为一个常数，但是大常数
