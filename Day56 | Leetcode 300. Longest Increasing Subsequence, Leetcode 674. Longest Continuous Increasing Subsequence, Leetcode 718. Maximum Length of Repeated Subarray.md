## 300. Longest Increasing Subsequence ##

**Link:**[300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/description/)

**Thoughts:**
1. dp[i]的定义
- 本题中，正确定义dp数组的含义十分重要。
- dp[i]表示i之前包括i的以nums[i]结尾的最长递增子序列的长度
- 为什么一定表示 “以nums[i]结尾的最长递增子序” ，因为我们在 做 递增比较的时候，如果比较 nums[j] 和 nums[i] 的大小，那么两个递增子序列一定分别以nums[j]为结尾 和 nums[i]为结尾， 要不然这个比较就没有意义了，不是尾部元素的比较那么 如何算递增呢。
2. 状态转移方程
- 位置i的最长升序子序列等于j从0到i-1各个位置的最长升序子序列 + 1 的最大值。

- 所以：if (nums[i] > nums[j]) dp[i] = max(dp[i], dp[j] + 1);

- 注意这里不是要dp[i] 与 dp[j] + 1进行比较，而是我们要取dp[j] + 1的最大值。

3. dp[i]的初始化
- 每一个i，对应的dp[i]（即最长递增子序列）起始大小至少都是1.

4. 确定遍历顺序
- dp[i] 是有0到i-1各个位置的最长递增子序列 推导而来，那么遍历i一定是从前向后遍历。

- j其实就是遍历0到i-1，那么是从前到后，还是从后到前遍历都无所谓，只要吧 0 到 i-1 的元素都遍历了就行了。 所以默认习惯 从前向后遍历。

- 遍历i的循环在外层，遍历j则在内层，代码如下：
![image](https://user-images.githubusercontent.com/69004164/215901319-ef7aba39-5f54-4f3e-9bcb-3ee675ffa3bb.png)

5. 举例推导dp数组
输入：[0,1,0,3,2]，dp数组的变化如下：
![image](https://user-images.githubusercontent.com/69004164/215901615-cb816230-a819-43f8-9818-604f16da621c.png)

**Solution:**
```
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp = [1] * len(nums)

        for i in range(len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1)
        
        length = 0
        for e in dp:
            length = max(length, e)
        return length
```

## 674. Longest Continuous Increasing Subsequence ##

**Link:**[674. Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/)

**Thoughts:**
1. dp[i]的定义
- dp[i]：以下标i为结尾的连续递增的子序列长度为dp[i]。

- 注意这里的定义，一定是以下标i为结尾，并不是说一定以下标0为起始位置。
2. 状态转移方程
- 如果 nums[i] > nums[i - 1]，那么以 i 为结尾的连续递增的子序列长度 一定等于 以i - 1为结尾的连续递增的子序列长度 + 1 。

- 即：dp[i] = dp[i - 1] + 1;

- 注意这里就体现出和动态规划：300.最长递增子序列的区别！

 - 因为本题要求连续递增子序列，所以就必要比较nums[i]与nums[i - 1]，而不用去比较nums[j]与nums[i] （j是在0到i之间遍历）。

 - 既然不用j了，那么也不用两层for循环，本题一层for循环就行，比较nums[i] 和 nums[i - 1]。
3. dp[i]的初始化
- 每一个i，对应的dp[i]（即最长递增子序列）起始大小至少都是1.

4. 确定遍历顺序
- 从递推公式上可以看出， dp[i + 1]依赖dp[i]，所以一定是从前向后遍历。
![image](https://user-images.githubusercontent.com/69004164/215903122-b519491d-2281-4cf7-86e4-fc06d5a7d894.png)

5. 举例推导dp数组
已输入nums = [1,3,5,4,7]为例，dp数组状态如下：
![image](https://user-images.githubusercontent.com/69004164/215903167-d286adfe-32a3-4eb7-ae6c-b96ffbc9d5fa.png)

**Solution:**
```
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        dp = [1] * len(nums)

        for i in range(1, len(nums)):
            if nums[i] > nums[i-1]:
                dp[i] = dp[i-1] + 1
        length = 0
        for e in dp:
            length = max(e, length)
        return length
```

## 718. Maximum Length of Repeated Subarray ##

**Link:**[718. Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray/description/)

**Thoughts:**
1. 确定dp数组（dp table）以及下标的含义
 - dp[i][j] ：以下标i - 1为结尾的A，和以下标j - 1为结尾的B，最长重复子数组长度为dp[i][j]。 （特别注意： “以下标i - 1为结尾的A” 标明一定是 以A[i-1]为结尾的字符串 ）

 - 此时细心的同学应该发现，那dp[0][0]是什么含义呢？总不能是以下标-1为结尾的A数组吧。

 - 其实dp[i][j]的定义也就决定着，我们在遍历dp[i][j]的时候i 和 j都要从1开始。
2. 确定递推公式
 - 根据dp[i][j]的定义，dp[i][j]的状态只能由dp[i - 1][j - 1]推导出来。

 - 即当A[i - 1] 和B[j - 1]相等的时候，dp[i][j] = dp[i - 1][j - 1] + 1;

 - 根据递推公式可以看出，遍历i 和 j 要从1开始！
3. dp数组如何初始化
 - 根据dp[i][j]的定义，dp[i][0] 和dp[0][j]其实都是没有意义的！

 - 但dp[i][0] 和dp[0][j]要初始值，因为 为了方便递归公式dp[i][j] = dp[i - 1][j - 1] + 1;

 - 所以dp[i][0] 和dp[0][j]初始化为0。

 - 举个例子A[0]如果和B[0]相同的话，dp[1][1] = dp[0][0] + 1，只有dp[0][0]初始为0，正好符合递推公式逐步累加起来。
4. 确定遍历顺序
 - 外层for循环遍历A，内层for循环遍历B。

 - 那又有同学问了，外层for循环遍历B，内层for循环遍历A。不行么？

 - 也行，一样的，我这里就用外层for循环遍历A，内层for循环遍历B了。

 - 同时题目要求长度最长的子数组的长度。所以在遍历的时候顺便把dp[i][j]的最大值记录下来。

![image](https://user-images.githubusercontent.com/69004164/215904266-62b866cc-2a90-45c4-87f2-d6dc7407241f.png)

5. 举例推导dp数组
拿示例1中，A: [1,2,3,2,1]，B: [3,2,1,4,7]为例，画一个dp数组的状态变化，如下：
![image](https://user-images.githubusercontent.com/69004164/215904295-7e1c7b31-5b5d-46ba-a4bd-a33d0c662119.png)

**Solution**
```
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        dp = [[0] * (len(nums2) + 1) for _ in range(len(nums1) + 1)]
        res = 0
        for i in range(1, len(nums1) + 1):
            for j in range(1, len(nums2) + 1):
                if nums1[i-1] == nums2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                res = max(res, dp[i][j])
    
        return res
```
