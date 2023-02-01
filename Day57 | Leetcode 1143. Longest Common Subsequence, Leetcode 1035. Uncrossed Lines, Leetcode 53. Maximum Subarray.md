## 1143. Longest Common Subsequence ##

**Link:**[1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/description/)

**Thoughts:**

![image](https://user-images.githubusercontent.com/69004164/216135828-c96852e3-99ad-4c73-813f-41f08eec5320.png)
![image](https://user-images.githubusercontent.com/69004164/216135870-bf03f261-c19a-4a8a-8640-be715282da15.png)
![image](https://user-images.githubusercontent.com/69004164/216135927-0cb80b41-75a2-476b-8b97-2806e444871d.png)

**Solution:**
```
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        dp = [[0] * (len(text2) + 1) for _ in range(len(text1) + 1)]
        for i in range(1, len(text1) + 1):
            for j in range(1, len(text2) + 1):
                if text1[i-1] == text2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else: 
                    dp[i][j] = max(dp[i][j-1], dp[i-1][j])

        return dp[-1][-1]
```

## 1035. Uncrossed Lines ##

**Link:**[1035. Uncrossed Lines](https://leetcode.com/problems/uncrossed-lines/description/)

**Thoughts:**
本题说是求绘制的最大连线数，其实就是求两个字符串的最长公共子序列的长度

**Solution:**
```
class Solution:
    def maxUncrossedLines(self, nums1: List[int], nums2: List[int]) -> int:
        dp = [[0]* (len(nums2) + 1) for _ in range(len(nums1) + 1)]

        for i in range(1, len(nums1) + 1):
            for j in range(1, len(nums2) + 1):
                if nums1[i-1] == nums2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i][j-1], dp[i-1][j])
        
        return dp[-1][-1]
```

## 53. Maximum Subarray ##

**Link:**[53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/description/)

**Thoughts:**
![image](https://user-images.githubusercontent.com/69004164/216137804-e810f17e-ea75-4905-9bb3-e1b40975e537.png)
![image](https://user-images.githubusercontent.com/69004164/216137855-6487cacd-b95b-43bb-a420-7e99fde7ec0a.png)

**Solution:**
```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:

        dp = [0] * (len(nums))
        res = nums[0]
        dp[0] = nums[0]
        
        for i in range(1, len(nums)):
            dp[i] = max(dp[i-1] + nums[i], nums[i])
            res = max(dp[i],res)

        return res        
```
