## 84. Largest Rectangle in Histogram ##

**Link:** [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/description/)

**Thoughts:**
![image](https://user-images.githubusercontent.com/69004164/217953705-d508cade-2ca1-4b31-bf94-17f70fd50174.png)
![Uploading image.png…]()
[walk through](https://www.youtube.com/watch?v=zx5Sw9130L0)

**Solutions:**
```
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        maxArea = 0
        stack = []

        for i, h in enumerate(heights):
            start = i
            while stack and stack[-1][1] > heights[i]:
                index, height = stack.pop()
                maxArea = max(maxArea, (i-index) * height)
                start = index
            stack.append((start,h))
        
        for i, h in stack:
            maxArea = max(maxArea, (len(heights) - i) * h)
        
        return maxArea
```
