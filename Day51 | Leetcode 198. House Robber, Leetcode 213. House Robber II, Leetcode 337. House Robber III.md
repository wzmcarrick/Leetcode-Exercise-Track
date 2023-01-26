## 198. House Robber ##

**Link:**[198. House Robber](https://leetcode.com/problems/house-robber/description/)

**Thoughts:**
1. 确定dp数组（dp table）以及下标的含义:
  dp[i]：考虑下标i（包括i）以内的房屋，最多可以偷窃的金额为dp[i]。

2. 确定递推公式:
  决定dp[i]的因素就是第i房间偷还是不偷。
  如果偷第i房间，那么dp[i] = dp[i - 2] + nums[i] ，即：第i-1房一定是不考虑的，找出 下标i-2（包括i-2）以内的房屋，最多可以偷窃的金额为dp[i-2] 加上第i房间偷到的钱。
  如果不偷第i房间，那么dp[i] = dp[i - 1]，即考 虑i-1房，（注意这里是考虑，并不是一定要偷i-1房，这是很多同学容易混淆的点）
  然后dp[i]取最大值，即dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);

3. dp数组如何初始化:
  从递推公式dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);可以看出，递推公式的基础就是dp[0] 和 dp[1]
  从dp[i]的定义上来讲，dp[0] 一定是 nums[0]，dp[1]就是nums[0]和nums[1]的最大值即：dp[1] = max(nums[0], nums[1]); 

4. 确定遍历顺序
  dp[i] 是根据dp[i - 2] 和 dp[i - 1] 推导出来的，那么一定是从前到后遍历！

5. 举例推导dp数组
  以示例二，输入[2,7,9,3,1]为例。
  ![image](https://user-images.githubusercontent.com/69004164/214912301-3a249ccb-64de-4579-8464-29e4c2f2a870.png)

**Solution:**
```
class Solution:
    def rob(self, nums: List[int]) -> int:
        # dp[i] means at house i, the max amount of money that can rob
        # dp[i] = max(dp[i-1], dp[i-2] + nums[i])
        # dp[0] = nums[0]

        dp = [0] * len(nums)

        dp[0] = nums[0]
        if len(nums) >= 2:
            dp[1] = max(nums[0], nums[1])

        for i in range(2,len(nums)):
            dp[i] = max(dp[i-1], dp[i-2] + nums[i])
        return dp[-1]
```

## 213. House Robber II ##

**Link:**[213. House Robber II](https://leetcode.com/problems/house-robber-ii/description/)

**Thoughts:**
- Since House[1] and House[n] are adjacent, they cannot be robbed together. Therefore, the problem becomes to rob either House[1]-House[n-1] or House[2]-House[n], depending on which choice offers more money. Now the problem has degenerated to the House Robber, which is already been solved.

1. 确定dp数组（dp table）以及下标的含义:
  dp[i]：考虑下标i（包括i）以内的房屋，最多可以偷窃的金额为dp[i]。

2. 确定递推公式:
  决定dp[i]的因素就是第i房间偷还是不偷。
  如果偷第i房间，那么dp[i] = dp[i - 2] + nums[i] ，即：第i-1房一定是不考虑的，找出 下标i-2（包括i-2）以内的房屋，最多可以偷窃的金额为dp[i-2] 加上第i房间偷到的钱。
  如果不偷第i房间，那么dp[i] = dp[i - 1]，即考 虑i-1房，（注意这里是考虑，并不是一定要偷i-1房，这是很多同学容易混淆的点）
  然后dp[i]取最大值，即dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);

3. dp数组如何初始化:
  从递推公式dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);可以看出，递推公式的基础就是dp[0] 和 dp[1]
  从dp[i]的定义上来讲，dp[0] 一定是 nums[0]，dp[1]就是nums[0]和nums[1]的最大值即：dp[1] = max(nums[0], nums[1]); 

4. 确定遍历顺序
  dp[i] 是根据dp[i - 2] 和 dp[i - 1] 推导出来的，那么一定是从前到后遍历！

5. 举例推导dp数组
  以示例二，输入[2,7,9,3,1]为例。
  ![image](https://user-images.githubusercontent.com/69004164/214912301-3a249ccb-64de-4579-8464-29e4c2f2a870.png)

**Solution:**
```
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]
        
        nums1 = nums[:-1]
        nums2 = nums[1:]
        
        def robsep(nums):
            if len(nums) == 1:
                return nums[0]
            dp = [0] * len(nums)

            dp[0] = nums[0]

            dp[1] = max(nums[0], nums[1])

            for i in range(2, len(nums)):
                dp[i] = max(dp[i-1], dp[i-2] + nums[i])
            
            return dp[-1]
        
        return max(robsep(nums1), robsep(nums2))
```

## 337. House Robber III ##

**Link:**[337. House Robber III](https://leetcode.com/problems/house-robber-iii/description/)

**Thoughts:**
- 本题一定是要**后序遍历**，因为通过递归函数的返回值来做下一步计算。与198.打家劫舍，213.打家劫舍II一样，关键是要讨论当前节点抢还是不抢。
- 如果抢了当前节点，两个孩子就不能动，如果没抢当前节点，就可以考虑抢左右孩子（注意这里说的是“考虑”）
![image](https://user-images.githubusercontent.com/69004164/214913503-387664db-1b61-42fa-a967-ab194250221d.png)
![image](https://user-images.githubusercontent.com/69004164/214913548-89b5dba2-abf6-47d8-83cd-caec306e36cf.png)
![image](https://user-images.githubusercontent.com/69004164/214913619-bff24d47-6088-4cb3-af6d-bed1382ef4f8.png)
![image](https://user-images.githubusercontent.com/69004164/214913672-137cb6b4-d6c6-4333-8d1d-5efbced7e5c2.png)

**Solution:**
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        
        def robTree(self, curr):
            if not curr: 
                return [0,0]
            left = robTree(self, curr.left)
            right = robTree(self, curr.right)
            # rob the curr:
            val1 = left[0] + right[0] + curr.val
            # don't rob the curr:
            val2 = max(left[0],left[1]) + max(right[0],right[1])

            return [val2, val1]

        result = robTree(self,root)
        return max(result[0], result[1])

```
