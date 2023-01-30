## 309. Best Time to Buy and Sell Stock with Cooldown ##

**Link:**[309. Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

**Thoughts:**

```
dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
dp[i][1] = max(dp[i-1][1], dp[i-2][0] - prices[i])
解释：第 i 天选择 buy 的时候，要从 i-2 的状态转移，而不是 i-1 。
```

**Solution:**
```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) == 1:
            return 0
        
        dp = [[0,0] for _ in range(len(prices))]

        dp[0][0] = 0
        dp[0][1] = -prices[0]

        for i in range(1,len(prices)):

            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
            if i == 1:
                dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
            else: 
                dp[i][1] = max(dp[i-1][1], dp[i-2][0] - prices[i])
        return dp[-1][0]
```

## 714. Best Time to Buy and Sell Stock with Transaction Fee ##

**Link:**[714. Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/)

**Thoughts:**

```
dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i] - fee)
dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
解释：相当于卖出股票的价格减小了。

```

**Solution:**
```
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        dp = [[0,0] for _ in range(len(prices))]

        dp[0][0] = 0
        dp[0][1] = -prices[0]

        for i in range(1, len(prices)):
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i] - fee)
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
        return dp[-1][0]
```
