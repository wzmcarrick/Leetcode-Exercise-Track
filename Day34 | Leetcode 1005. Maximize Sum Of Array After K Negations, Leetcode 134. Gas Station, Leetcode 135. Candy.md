## 1005. Maximize Sum Of Array After K Negations ##

**Link:**[1005. Maximize Sum Of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/description/)

**Thoughts:**
  - 贪心的思路，局部最优：让绝对值大的负数变为正数，当前数值达到最大，整体最优：整个数组和达到最大
  - 那么如果将负数都转变为正数了，K依然大于0，此时的问题是一个有序正整数序列，如何转变K次正负，让 数组和 达到最大。
  - 那么又是一个贪心：局部最优：只找数值最小的正整数进行反转，当前数值可以达到最大（例如正整数数组{5, 3, 1}，反转1 得到-1 比 反转5得到的-5 大多了），全局最优：整个 数组和 达到最大。
  - 那么本题的解题步骤为：
      第一步：将数组按照绝对值大小从大到小排序，注意要按照绝对值的大小
      第二步：从前向后遍历，遇到负数将其变为正数，同时K--
      第三步：如果K还大于0，那么反复转变数值最小的元素，将K用完
      第四步：求和
  
**Solutions**
```
class Solution:
    def largestSumAfterKNegations(self, nums: List[int], k: int) -> int:
        
        A = sorted(nums,key=abs,reverse=True)
        
        for i in range(len(A)):
            if A[i] < 0 and k > 0:
                A[i] = - A[i]
                k -= 1
        while k > 0:
            A[-1] = - A[-1]
            k -= 1
        
        return sum(A)
```

## 134. Gas Station ##

**Link:**[134. Gas Station](https://leetcode.com/problems/gas-station/description/)

**Thoughts:**
  - 首先如果总油量减去总消耗大于等于零那么一定可以跑完一圈，说明 各个站点的加油站 剩油量rest[i]相加一定是大于等于零的。每个加油站的剩余量rest[i]为gas[i] - cost[i]。
  - i从0开始累加rest[i]，和记为curSum，一旦curSum小于零，说明[0, i]区间都不能作为起始位置，起始位置从i+1算起，再从0计算curSum。
  - 那么为什么一旦[i，j] 区间和为负数，起始位置就可以是j+1呢，j+1后面就不会出现更大的负数？
    如果出现更大的负数，就是更新j，那么起始位置又变成新的j+1了。而且j之前出现了多少负数，j后面就会出现多少正数，因为耗油总和是大于零的（前提我们已经确定了一定可以跑完全程）。
  - 那么局部最优：当前累加rest[j]的和curSum一旦小于0，起始位置至少要是j+1，因为从j开始一定不行。全局最优：找到可以跑一圈的起始位置。
  - ![image](https://user-images.githubusercontent.com/69004164/211392647-20eb0501-6aa0-4c32-8860-ed39db9f4bcf.png)

  
**Solutions**
```
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        curSum = 0
        totalSum = 0
        start = 0

        for i in range(len(gas)):
            curSum += gas[i] - cost[i]
            totalSum += gas[i] - cost[i]
            if curSum < 0:
                curSum = 0
                start = i + 1
        if totalSum < 0:
            return -1
        return start
```
时间复杂度：O(n)
空间复杂度：O(1)

## 135. Candy ##

**Link:**[135. Candy](https://leetcode.com/problems/candy/description/)

**Thoughts:**
  - 一定是要确定一边之后，再确定另一边，例如比较每一个孩子的左边，然后再比较右边，如果两边一起考虑一定会顾此失彼
  - 先确定右边评分大于左边的情况（也就是从前向后遍历）
    此时局部最优：只要右边评分比左边大，右边的孩子就多一个糖果，全局最优：相邻的孩子中，评分高的右孩子获得比左边孩子更多的糖果

    局部最优可以推出全局最优。

    如果ratings[i] > ratings[i - 1] 那么[i]的糖 一定要比[i - 1]的糖多一个，所以贪心：candyVec[i] = candyVec[i - 1] + 1
    
    ![image](https://user-images.githubusercontent.com/69004164/211393087-2cb583b2-c0f0-44bd-ad0a-003dc3dab93e.png)
    
  - 再确定左孩子大于右孩子的情况（从后向前遍历）
    如果 ratings[i] > ratings[i + 1]，此时candyVec[i]（第i个小孩的糖果数量）就有两个选择了，一个是candyVec[i + 1] + 1（从右边这个加1得到的糖果数量），一个是candyVec[i]（之前比较右孩子大于左孩子得到的糖果数量）。

    那么又要贪心了，局部最优：取candyVec[i + 1] + 1 和 candyVec[i] 最大的糖果数量，保证第i个小孩的糖果数量既大于左边的也大于右边的。全局最优：相邻的孩子中，评分高的孩子获得更多的糖果。

    局部最优可以推出全局最优。

    所以就取candyVec[i + 1] + 1 和 candyVec[i] 最大的糖果数量，candyVec[i]只有取最大的才能既保持对左边candyVec[i - 1]的糖果多，也比右边candyVec[i + 1]的糖果多。
    
    ![image](https://user-images.githubusercontent.com/69004164/211393572-91d93502-c9d0-40e6-aafe-e99efddb2d87.png)

**Solutions**
```
class Solution:
    def candy(self, ratings: List[int]) -> int:
        candy = [1] * len(ratings)
        
        for i in range(1, len(ratings)):
            if ratings[i] > ratings[i-1]:
                candy[i] = candy[i-1] + 1

        for i in range(len(ratings) - 1, 0,-1):
            if ratings[i-1] > ratings[i]:
                candy[i-1] = max(candy[i-1], candy[i]+1)

        return sum(candy)
```
