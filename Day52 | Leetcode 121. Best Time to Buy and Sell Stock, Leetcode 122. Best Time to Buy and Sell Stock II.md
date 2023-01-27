## 121. Best Time to Buy and Sell Stock ##

**Link:**[121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)

**Thought:**
1. 确定dp数组（dp table）以及下标的含义
- dp[i][1] 表示第i天持有股票所得最多现金 ，这里可能有同学疑惑，本题中只能买卖一次，持有股票之后哪还有现金呢? 其实一开始现金是0，那么加入第i天买入股票现金就是 -prices[i]， 这是一个负数。
- dp[i][0] 表示第i天不持有股票所得最多现金
2. 确定递推公式
  如果第i天持有股票即dp[i][1]， 那么可以由两个状态推出来

  第i-1天就持有股票，那么就保持现状，所得现金就是昨天持有股票的所得现金 即：dp[i - 1][1]
  第i天买入股票，所得现金就是买入今天的股票后所得现金即：-prices[i]
  那么dp[i][1]应该选所得现金最大的，所以dp[i][1] = max(dp[i - 1][1], -prices[i]);

  如果第i天不持有股票即dp[i][0]， 也可以由两个状态推出来

  第i-1天就不持有股票，那么就保持现状，所得现金就是昨天不持有股票的所得现金 即：dp[i - 1][0]
  第i天卖出股票，所得现金就是按照今天股票价格卖出后所得现金即：prices[i] + dp[i - 1][1]
  同样dp[i][0]取最大的，dp[i][0] = max(dp[i - 1][0], prices[i] + dp[i - 1][1]);
3. dp数组如何初始化
  其基础都是要从dp[0][0]和dp[0][1]推导出来。
4. 确定遍历顺序
  从递推公式可以看出dp[i]都是由dp[i - 1]推导出来的，那么一定是从前向后遍历。

5. 举例推导dp数组
  以示例1，输入：[7,1,5,3,6,4]为例，dp数组状态如下：
  ![image](https://user-images.githubusercontent.com/69004164/215156197-72831030-8486-47a5-9dd8-0720c39283a3.png)

**Solutions:**
```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:

        dp = [[0,0] for _ in range(len(prices))]

        dp[0][0] = 0
        dp[0][1] = -prices[0]

        for i in range(1,len(prices)):

            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
            dp[i][1] = max(dp[i-1][1], -prices[i])

        return dp[-1][0]
```
时间复杂度：O(n)
空间复杂度：O(n)

## 122. Best Time to Buy and Sell Stock II ##

**Link:**[122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)

**Thought:**
1. 确定dp数组（dp table）以及下标的含义
- dp[i][1] 表示第i天持有股票所得最多现金 ，这里可能有同学疑惑，本题中只能买卖一次，持有股票之后哪还有现金呢? 其实一开始现金是0，那么加入第i天买入股票现金就是 -prices[i]， 这是一个负数。
- dp[i][0] 表示第i天不持有股票所得最多现金
2. 确定递推公式
  如果第i天持有股票即dp[i][1]， 那么可以由两个状态推出来

  第i-1天就持有股票，那么就保持现状，所得现金就是昨天持有股票的所得现金 即：dp[i - 1][1]
  第i天买入股票，所得现金就是昨天不持有股票的所得现金减去 今天的股票价格：dp[i-1][0]-prices[i]
  那么dp[i][1]应该选所得现金最大的，所以dp[i][1] = max(dp[i - 1][1], dp[i-1][0]-prices[i]);

  如果第i天不持有股票即dp[i][0]， 也可以由两个状态推出来

  第i-1天就不持有股票，那么就保持现状，所得现金就是昨天不持有股票的所得现金 即：dp[i - 1][0]
  第i天卖出股票，所得现金就是按照今天股票价格卖出后所得现金即：prices[i] + dp[i - 1][1]
  同样dp[i][0]取最大的，dp[i][0] = max(dp[i - 1][0], prices[i] + dp[i - 1][1]);
3. dp数组如何初始化
  其基础都是要从dp[0][0]和dp[0][1]推导出来。
4. 确定遍历顺序
  从递推公式可以看出dp[i]都是由dp[i - 1]推导出来的，那么一定是从前向后遍历。

5. 举例推导dp数组
  以示例1，输入：[7,1,5,3,6,4]为例，dp数组状态如下：
  ![image](https://user-images.githubusercontent.com/69004164/215156197-72831030-8486-47a5-9dd8-0720c39283a3.png)

**Solutions:**
```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [[0,0] for _ in range(len(prices))]

        dp[0][0] = 0
        dp[0][1] = -prices[0]

        for i in range(1,len(prices)):
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
        return dp[-1][0]
```
时间复杂度：O(n)
空间复杂度：O(n)
