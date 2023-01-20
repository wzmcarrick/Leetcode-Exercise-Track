## 518. Coin Change II ##

**Link:**[518. Coin Change II](https://leetcode.com/problems/coin-change-ii/description/)

**Thoughts:**
  - 纯完全背包是凑成背包最大价值是多少，而本题是要求凑成总金额的物品组合个数！
  - 组合不强调元素之间的顺序，排列强调元素之间的顺序

  - 确定dp数组以及下标的含义
    dp[j]：凑成总金额j的货币组合数为dp[j]
  - 确定递推公式
    dp[j] += dp[j - coins[i]]
  - dp数组如何初始化
    首先dp[0]一定要为1，dp[0] = 1是 递归公式的基础。如果dp[0] = 0 的话，后面所有推导出来的值都是0了。
  - 确定遍历顺序
    ![image](https://user-images.githubusercontent.com/69004164/213597205-4ccfcb04-4acf-419b-9fd2-c27fb3f90601.png)
  - 举例推导dp数组
    输入: amount = 5, coins = [1, 2, 5] ，dp状态图如下：
    ![image](https://user-images.githubusercontent.com/69004164/213597275-df73edd6-9743-4d5e-8d1a-0c70977de1c1.png)

**Solution:**
```
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        
        dp = [0] * (amount + 1)

        dp[0] = 1

        for i in range(len(coins)):
            for j in range(coins[i], amount + 1):
                dp[j] += dp[j-coins[i]]

        return dp[amount]
```

## 377. Combination Sum IV ##

**Link:**[377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/description/)

**Thoughts:**
  - 题目描述说是求组合，但又说是可以元素相同顺序不同的组合算两个组合，其实就是求排列！
  - 组合不强调顺序，(1,5)和(5,1)是同一个组合。排列强调顺序，(1,5)和(5,1)是两个不同的排列。

  - 确定dp数组以及下标的含义
    dp[i]: 凑成目标正整数为i的排列个数为dp[i]
  - 确定递推公式
    dp[i] += dp[i - nums[j]]
  - dp数组如何初始化
    因为递推公式dp[i] += dp[i - nums[j]]的缘故，dp[0]要初始化为1，这样递归其他dp[i]的时候才会有数值基础
  - 确定遍历顺序
    如果求组合数就是外层for循环遍历物品，内层for遍历背包。
    如果求排列数就是外层for遍历背包，内层for循环遍历物品。
    如果把遍历nums（物品）放在外循环，遍历target的作为内循环的话，举一个例子：计算dp[4]的时候，结果集只有 {1,3} 这样的集合，不会有{3,1}这样的集合，因为nums遍历放在外层，3只能出现在1后面！
    所以本题遍历顺序最终遍历顺序：target（背包）放在外循环，将nums（物品）放在内循环，内循环从前到后遍历
    ![image](https://user-images.githubusercontent.com/69004164/213597205-4ccfcb04-4acf-419b-9fd2-c27fb3f90601.png)
  - 举例推导dp数组
    ![image](https://user-images.githubusercontent.com/69004164/213597659-19c39c8d-bec5-4235-8788-b7ee33055355.png)


**Solution:**
```
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [0] * (target + 1)

        dp[0] = 1

        for j in range(target + 1):
            for i in range(len(nums)):
                if j - nums[i] >= 0:
                   dp[j] += dp[j - nums[i]]
        
        return dp[target]
```
