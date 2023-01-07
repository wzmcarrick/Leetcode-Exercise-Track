## 122. Best Time to Buy and Sell Stock II ##

**Link:**[122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)

**Thoughts:**
  - 这道题我用的动态规划(https://labuladong.github.io/algo/di-er-zhan-a01c6/yong-dong--63ceb/yi-ge-fang-3b01b/)

**Solution:**
```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)

        dp = [[0,0] for _ in range(n)]

        dp[0][0] = 0
        dp[0][1] = -prices[0]
        for i in range(1,n):
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
        return dp[-1][0]
```

## 55. Jump Game ##

**Link:**[55. Jump Game](https://leetcode.com/problems/jump-game/description/)

**Thoughts:**
  - 这个问题就转化为跳跃覆盖范围究竟可不可以覆盖到终点
  - 每次移动取最大跳跃步数（得到最大的覆盖范围），每移动一个单位，就更新最大覆盖范围。
  - 贪心算法局部最优解：每次取最大跳跃步数（取最大覆盖范围），整体最优解：最后得到整体最大覆盖范围，看是否能到终点。
  - ![image](https://user-images.githubusercontent.com/69004164/211172115-c35b0af1-cc95-4779-b510-d8ecbe883a71.png)

**Solution:**
```
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        cover = 0
        n = len(nums)
        if n == 1: 
            return True
        # python不支持动态修改for循环中变量
        for i in range(n):
            if i <= cover:
                cover = max(cover, i + nums[i])
                if cover >= n - 1:
                    return True
        return False
```

## 45. Jump Game II ##

**Link:**[45. Jump Game II](https://leetcode.com/problems/jump-game-ii/description/)

**Thoughts:**
  - 我们站在索引 0 的位置，可以向前跳 1，2 或 3 步，你说应该选择跳多少呢？显然应该跳 2 步调到索引 2，因为 nums[2] 的可跳跃区域涵盖了索引区间 [3..6]，比其他的都大。
  - ![image](https://user-images.githubusercontent.com/69004164/211172168-8a9c9866-5f38-4818-a350-04d750a2ca05.png)
  - ![image](https://user-images.githubusercontent.com/69004164/211172191-9abf3010-7e8e-46c5-86b7-dd880f96e074.png)
  - i 和 end 标记了可以选择的跳跃步数，farthest 标记了所有选择 [i..end] 中能够跳到的最远距离，jumps 记录了跳跃次数

**Solution:**
```
class Solution:
    def jump(self, nums: List[int]) -> int:
        curDis = 0
        res = 0
        nxtDis = 0

        for i in range(len(nums) - 1):
            nxtDis = max(nxtDis, nums[i] + i)
            if i == curDis:
                curDis = nxtDis
                res += 1
        
        return res
```
