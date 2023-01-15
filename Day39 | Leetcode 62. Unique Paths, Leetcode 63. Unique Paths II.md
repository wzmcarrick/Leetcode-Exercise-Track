## 62. Unique Paths ##

**Link:**[62. Unique Paths](https://leetcode.com/problems/unique-paths/description/)

**Thoughts:**
  - 确定dp数组（dp table）以及下标的含义: dp[i][j] ：表示从（0 ，0）出发，到(i, j) 有dp[i][j]条不同的路径。
  - 递推公式: dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
  - dp数组的初始化: 首先dp[i][0]一定都是1，因为从(0, 0)的位置到(i, 0)的路径只有一条，那么dp[0][j]也同理。

**Solutions:**
```
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[1 for _ in range(n)] for _ in range(m)]
        
        if m == 1 or n == 1:
            return 1

        for r in range(1,m):
            for c in range(1,n):
                dp[r][c] = dp[r-1][c] + dp[r][c-1]

        return dp[m-1][n-1]
```
时间复杂度：O(m × n)
空间复杂度：O(m × n)

## 63. Unique Paths II ##

**Link:**[62. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/description/)

**Thoughts:**
  - 确定dp数组（dp table）以及下标的含义: dp[i][j] ：表示从（0 ，0）出发，到(i, j) 有dp[i][j]条不同的路径。
  - 递推公式: dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
    但因为有了障碍，(i, j)如果就是障碍的话应该就保持初始状态（初始状态为0）
  - dp数组的初始化: 首先dp[i][0]一定都是1，因为从(0, 0)的位置到(i, 0)的路径只有一条，那么dp[0][j]也同理。
    但如果(i, 0) 这条边有了障碍之后，障碍之后（包括障碍）都是走不到的位置了，所以障碍之后的dp[i][0]应该还是初始值0
  - ![image](https://user-images.githubusercontent.com/69004164/212503779-2871a238-467e-4101-b1ed-0e6e84564196.png)

**Solutions:**
```
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        
        //如果在起点或终点出现了障碍，直接返回0
        if obstacleGrid[0][0] == 1 or obstacleGrid[m-1][n-1] == 1:
            return 0

        dp = [[0 for _ in range(n)] for _ in range(m)]
        
        for i in range(0,m):
            if obstacleGrid[i][0] == 1:
                break
            dp[i][0] = 1
        
        for j in range(0,n):
            if obstacleGrid[0][j] == 1: # 遇到障碍物时，直接退出循环，后面默认都是0
                break
            dp[0][j] = 1

        for r in range(1,m):
            for c in range(1,n):
                if obstacleGrid[r][c] == 1:
                    dp[r][c] = 0
                else:
                    dp[r][c] = dp[r-1][c] + dp[r][c-1]
        
        return dp[m-1][n-1]
```
时间复杂度：O(m × n)
空间复杂度：O(m × n)
