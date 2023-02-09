## 503. Next Greater Element II ##

**Link:** [503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/description/)

**Thoughts:**
本篇我侧重与说一说，如何处理循环数组。
其实也可以不扩充nums，而是在遍历的过程中模拟走了两边nums。
模拟遍历两边nums，注意一下都是用i % len(nums)来操作

**Solution:**
```
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        res = [-1] * len(nums)

        stack = []

        for i in range(len(nums) * 2):
            while stack and nums[stack[-1]] < nums[i%len(nums)]:
                res[stack[-1]] = nums[i%len(nums)]
                stack.pop()
            stack.append(i%len(nums))
        return res
```

## 42. Trapping Rain Water ##

**Link:** [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/description/)

**Thoughts:**
![image](https://user-images.githubusercontent.com/69004164/217938892-d76d0aa5-8c8a-4353-bc52-814b7b65ef6e.png)
- [two pointers](https://www.youtube.com/watch?v=ZI2z5pq0TqA)

**Solution:**
```
class Solution:
    def trap(self, height: List[int]) -> int:
        l,r = 0, len(height) - 1
        res = 0
        leftMax = height[l]
        rightMax = height[r]

        while l < r:
            if leftMax < rightMax:
                l += 1
                res += max(min(leftMax, rightMax) - height[l], 0)
                leftMax = max(leftMax, height[l])
            else:
                r -= 1
                res += max(min(leftMax, rightMax) - height[r], 0)
                rightMax = max(rightMax, height[r])
        
        return res
```
