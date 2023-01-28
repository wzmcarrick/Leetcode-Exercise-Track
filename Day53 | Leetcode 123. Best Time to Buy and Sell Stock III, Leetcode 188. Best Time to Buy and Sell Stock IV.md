## 123. Best Time to Buy and Sell Stock III ##

**Link:** [123. Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/description/)

**Thoughts:**
1. ![image](https://user-images.githubusercontent.com/69004164/215292669-5c2b1852-7d12-47f8-944a-b793fba22e4e.png)
2. ![image](https://user-images.githubusercontent.com/69004164/215292679-9737624a-2d2b-4cb4-8b78-29fab42c8c55.png)

dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
我昨天本没有持有，但今天准备buy一次，那截止到昨天为止购买次数最多只有k-1次了，因为我要给今天的buy预留一次购买机会，今天买完，凑满最大购买次数k。

**Solution:**
```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:

        # dp[i][j][0]: at day i, j (between 0 and 2) transactions left, don't hold any stock
        # dp[i][1][1]: at day i, j (between 0 and 1) transactions left, hold stock 

        # dp[i][j][0] = max(dp[i-1][j][0], dp[i-1][j][1] + prices[i])
        # dp[i][j][1] = max(dp[i-1][j][1], dp[i-1][j-1][0] - prices[i])

        # dp[0][j][0] = 0
        # dp[0][j][1] = -prices[0]

        dp = [[[0,0] for _ in range(3)] for _ in range(len(prices))]

        for i in range(len(prices)):
            for j in range(2,0,-1):
                if i == 0:
                    dp[i][j][0] = 0
                    dp[i][j][1] = -prices[i]
                    continue
                dp[i][j][0] = max(dp[i-1][j][0], dp[i-1][j][1] + prices[i])
                dp[i][j][1] = max(dp[i-1][j][1], dp[i-1][j-1][0] - prices[i])    
        
        return dp[-1][2][0]
```

**Link:** [123. Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/description/)

**Thoughts:**
1. ![image](https://user-images.githubusercontent.com/69004164/215292669-5c2b1852-7d12-47f8-944a-b793fba22e4e.png)
2. ![image](https://user-images.githubusercontent.com/69004164/215292679-9737624a-2d2b-4cb4-8b78-29fab42c8c55.png)

dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
我昨天本没有持有，但今天准备buy一次，那截止到昨天为止购买次数最多只有k-1次了，因为我要给今天的buy预留一次购买机会，今天买完，凑满最大购买次数k。

**Solution:**
```
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:

        dp = [[[0,0] for _ in range(k+1)] for _ in range(len(prices))]

        for i in range(len(prices)):
            for j in range(k,0,-1):
                if i == 0:
                    dp[i][j][0] = 0
                    dp[i][j][1] = -prices[i]
                    continue
                dp[i][j][0] = max(dp[i-1][j][0], dp[i-1][j][1] + prices[i])
                dp[i][j][1] = max(dp[i-1][j][1], dp[i-1][j-1][0] - prices[i])
        return dp[-1][k][0]
```
