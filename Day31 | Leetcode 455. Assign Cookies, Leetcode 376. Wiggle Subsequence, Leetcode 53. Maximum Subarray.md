## 455. Assign Cookies ##

**Link:**[455. Assign Cookies](https://leetcode.com/problems/assign-cookies/description/)

**Thoughts:**
  - 贪心算法一般分为如下四步：
    1. 将问题分解为若干个子问题
    2. 找出适合的贪心策略
    3. 求解每一个子问题的最优解
    4. 将局部最优解堆叠成全局最优解
  
  - 为了满足更多的小孩，就不要造成饼干尺寸的浪费。大尺寸的饼干既可以满足胃口大的孩子也可以满足胃口小的孩子，那么就应该优先满足胃口大的。
  - 这里的局部最优就是大饼干喂给胃口大的，充分利用饼干尺寸喂饱一个，全局最优就是喂饱尽可能多的小孩。
  - 可以尝试使用贪心策略，先将饼干数组和小孩数组排序。
  - 然后从后向前遍历小孩数组，用大饼干优先满足胃口大的，并统计满足小孩数量。
  
  - 从代码中可以看出我用了一个index来控制饼干数组的遍历，遍历饼干并没有再起一个for循环，而是采用自减的方式，这也是常用的技巧。

**Solution:**
```
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()
        res = 0
        start = len(s) - 1
         # 思路2：优先考虑胃口
        for i in range(len(g) - 1, -1, -1): # 先喂饱大胃口
            if start >= 0 and s[start] >= g[i]:
                start -= 1
                res += 1
        return res
```

## 376. Wiggle Subsequence ##

**Link:**[376. Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/description/)

**Thoughts:**
  - 局部最优：删除单调坡度上的节点（不包括单调坡度两端的节点），那么这个坡度就可以有两个局部峰值。
  - 整体最优：整个序列有最多的局部峰值，从而达到最长摆动序列。
  - ![image](https://user-images.githubusercontent.com/69004164/211113825-c7fd47ae-8db8-4212-a20d-712914aa22ad.png)
  - 本题代码实现中，还有一些技巧，例如统计峰值的时候，数组最左面和最右面是最不好统计的。例如序列[2,5]，它的峰值数量是2，如果靠统计差值来计算峰值个数就需要考虑数组最左面和最右面的特殊情况。所以可以针对序列[2,5]，可以假设为[2,2,5]，这样它就有坡度了即preDiff = 0，如图：
  - ![image](https://user-images.githubusercontent.com/69004164/211113985-e17cd758-194b-44e7-9585-ec1b6dc54aeb.png)


**Solution:**
```
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        preC, curC, res = 0,0,1 #题目里nums长度大于等于1，当长度为1时，其实到不了for循环里去，所以不用考虑nums长度

        for i in range(len(nums) - 1):
            curC = nums[i+1] - nums[i]
            if curC * preC <= 0 and curC != 0: #差值为0时，不算摆动
                res += 1
                preC = curC #如果当前差值和上一个差值为一正一负时，才需要用当前差值替代上一个差值
        return res
```
时间复杂度：O(n)
空间复杂度：O(1)


## 53. Maximum Subarray ##

**Link:**[53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/description/)

**Thoughts:**
  - 如果 -2 1 在一起，计算起点的时候，一定是从1开始计算，因为负数只会拉低总和，这就是贪心贪的地方！
  - 局部最优：当前“连续和”为负数的时候立刻放弃，从下一个元素重新计算“连续和”，因为负数加上下一个元素 “连续和”只会越来越小。
  - 全局最优：选取最大“连续和”
  - 局部最优的情况下，并记录最大的“连续和”，可以推出全局最优。
  - 从代码角度上来讲：遍历nums，从头开始用count累积，如果count一旦加上nums[i]变为负数，那么就应该从nums[i+1]开始从0累积count了，因为已经变为负数的count，只会拖累总和。
  - ![image](https://user-images.githubusercontent.com/69004164/211114186-e1e0231a-6368-425e-bfcf-a2167ec9728b.png)

**Solution:**
```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        curSum = 0
        larSum = -float('inf')
        for i in range(len(nums)):
            curSum += nums[i]
            larSum = max(curSum, larSum)
            if curSum < 0:
                curSum = 0
                continue
        return larSum
```
时间复杂度：O(n)
空间复杂度：O(1)
