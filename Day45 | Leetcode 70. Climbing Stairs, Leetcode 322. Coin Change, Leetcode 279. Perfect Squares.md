## 70. Climbing Stairs ##

**Link:**[70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)

**Thoughts:**
  - this questions equals to: have items that their weight is from 1 - m, how many combination to fill the bag with size is n? (m = 2 for this question)
        
     1. dp[i] means when the bag size is i, how many combinations
     2. dp[i] += dp[i-j]; j is the weight of current item that will be put in the bag: j between 1 to m
     3. dp[0] = 1
     4. how to iterate: iterate twice, one for item, one for bag size:
       since we need combinations that {1,3} and {3,1} are considered different, we will first iterate bag size, then iterate item
       since we can use items unlimited times, we will iterate items from front to end

**Solution:**

```
class Solution:
    def climbStairs(self, n: int) -> int:
        # have items that their weight is from 1 - m, how many combination to fill the bag with size is n?
        
        # 1. dp[i] means when the bag size is i, how many combinations
        # 2. dp[i] += dp[i-j]; j is the weight of current item that will be put in the bag: j between 1 to m
        # 3. dp[0] = 1
        # 4. how to iterate: iterate twice, one for item, one for bag size:
        #   since we need combinations that {1,3} and {3,1} are considered different, we will first iterate bag size, then iterate item
        #   since we can use items unlimited times, we will iterate items from front to end

        dp = [0] * (n+1)

        dp[0] = 1

        for i in range(1,n+1): # iterate bag size first
            for j in range(1, 2+1): # iterate item
                if i >= j:
                    dp[i] += dp[i-j]
        return dp[n]
```

## 322. Coin Change ##

**Link:**[322. Coin Change](https://leetcode.com/problems/coin-change/description/)

**Thoughts:**
1. 确定dp数组以及下标的含义
dp[j]：凑足总额为j所需钱币的最少个数为dp[j]

2. 确定递推公式
凑足总额为j - coins[i]的最少个数为dp[j - coins[i]]，那么只需要加上一个钱币coins[i]即dp[j - coins[i]] + 1就是dp[j]（考虑coins[i]）
所以dp[j] 要取所有 dp[j - coins[i]] + 1 中最小的。
递推公式：dp[j] = min(dp[j - coins[i]] + 1, dp[j]);

3. dp数组如何初始化
首先凑足总金额为0所需钱币的个数一定是0，那么dp[0] = 0;
其他下标对应的数值呢？
考虑到递推公式的特性，dp[j]必须初始化为一个最大的数，否则就会在min(dp[j - coins[i]] + 1, dp[j])比较的过程中被初始值覆盖。
所以下标非0的元素都是应该是最大值。

4. 确定遍历顺序
本题求钱币最小个数，那么钱币有顺序和没有顺序都可以，都不影响钱币的最小个数。
所以本题并不强调集合是组合还是排列。
如果求组合数就是外层for循环遍历物品，内层for遍历背包。
如果求排列数就是外层for遍历背包，内层for循环遍历物品。
所以本题的两个for循环的关系是：外层for循环遍历物品，内层for遍历背包或者外层for遍历背包，内层for循环遍历物品都是可以的！
那么我采用coins放在外循环，target在内循环的方式。
本题钱币数量可以无限使用，那么是完全背包。所以遍历的内循环是正序
综上所述，遍历顺序为：coins（物品）放在外循环，target（背包）在内循环。且内循环正序。

5. 举例推导dp数组
以输入：coins = [1, 2, 5], amount = 5为例
![image](https://user-images.githubusercontent.com/69004164/213809994-fcfc4703-dad6-4fce-9f1e-e841afd59145.png)

**Solution:**
```
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
    
        dp = [amount + 1] * (amount + 1)

        dp[0] = 0

        for i in range(len(coins)):
            
            for j in range(coins[i], amount + 1):
                
                dp[j] = min(dp[j], dp[j-coins[i]]+1)

        return dp[amount] if dp[amount] != amount + 1 else -1
```

## 279. Perfect Squares ##

**Link:**[279. Perfect Squares](https://leetcode.com/problems/perfect-squares/description/)

**Thoughts:**
完全平方数就是物品（可以无限件使用），凑个正整数n就是背包，问凑满这个背包最少有多少物品？

1. 确定dp数组（dp table）以及下标的含义
dp[j]：和为j的完全平方数的最少数量为dp[j]

2. 确定递推公式
dp[j] 可以由dp[j - i * i]推出， dp[j - i * i] + 1 便可以凑成dp[j]。

此时我们要选择最小的dp[j]，所以递推公式：dp[j] = min(dp[j - i * i] + 1, dp[j]);

3. dp数组如何初始化
dp[0]表示 和为0的完全平方数的最小数量，那么dp[0]一定是0。

有同学问题，那0 * 0 也算是一种啊，为啥dp[0] 就是 0呢？

看题目描述，找到若干个完全平方数（比如 1, 4, 9, 16, ...），题目描述中可没说要从0开始，dp[0]=0完全是为了递推公式。

非0下标的dp[j]应该是多少呢？

从递归公式dp[j] = min(dp[j - i * i] + 1, dp[j]);中可以看出每次dp[j]都要选最小的，所以非0下标的dp[j]一定要初始为最大值，这样dp[j]在递推的时候才不会被初始值覆盖。

4. 确定遍历顺序
我们知道这是完全背包，

如果求组合数就是外层for循环遍历物品，内层for遍历背包。

如果求排列数就是外层for遍历背包，内层for循环遍历物品。

所以本题外层for遍历背包，内层for遍历物品，还是外层for遍历物品，内层for遍历背包，都是可以的！

5. 举例推导dp数组
已输入n为5例，dp状态图如下：
![image](https://user-images.githubusercontent.com/69004164/213810813-be113d40-b12a-4550-b967-b8c0ebe3c78a.png)

**Solution:**
```
class Solution:
    def numSquares(self, n: int) -> int:
        arr = []
        for i in range(1, n+1):
            if i*i <= n:
                arr.append(i*i)

        dp = [n + 1] * (n + 1)

        dp[0] = 0

        for i in range(len(arr)):
            for j in range(arr[i], n + 1):
                dp[j] = min(dp[j], dp[j-arr[i]]+ 1)
        
        return dp[n] if dp[n] != n + 1 else 0 
```
