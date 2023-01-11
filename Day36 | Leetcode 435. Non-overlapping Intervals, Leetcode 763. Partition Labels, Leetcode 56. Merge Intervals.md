## 435. Non-overlapping Intervals ##

**Link:**[435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

**Thoughts:**
  - ![image](https://user-images.githubusercontent.com/69004164/211882242-0ccb7eb9-5ac0-401e-b2c7-7005038e46e8.png)
  

**Solutions**
```
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        # 按照右边界排序，然后从左向右遍历
        intervals.sort(key =  lambda x: (x[1]))
        print(intervals)
        res = 0
        for i in range(1,len(intervals)):
            if intervals[i][0] < intervals[i-1][1]:
                res += 1
                intervals[i] = intervals[i-1]
        return res
```
时间复杂度：O(nlog n) ，有一个快排
空间复杂度：O(n)，有一个快排，最差情况(倒序)时，需要n次递归调用。因此确实需要O(n)的栈空间

## 763. Partition Labels ##

**Link:**[763. Partition Labels](https://leetcode.com/problems/partition-labels/description/)

**Thoughts:**
  - 统计每一个字符最后出现的位置
  - 从头遍历字符，并更新字符的最远出现下标，如果找到字符最远出现位置下标和当前下标相等了，则找到了分割点
  - ![image](https://user-images.githubusercontent.com/69004164/211882558-214a6212-790d-4bf1-b5ab-0200637ef7d0.png)
  
**Solutions**
```
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        has = [0]*26
        for i in range(len(s)):
            has[ord(s[i]) - ord('a')] = i
        res = []
        l = 0
        r = 0
        for i in range(len(s)):
            r = max(r,has[ord(s[i]) - ord('a')])
            if i == r:
                res.append(r - l + 1)
                l = i + 1
        return res
```
时间复杂度：$O(n)$
空间复杂度：$O(1)$，使用的hash数组是固定大小

## 56. Merge Intervals ##

**Link:**[56. Merge Intervals](https://leetcode.com/problems/merge-intervals/description/)

**Thoughts:**
  - ![image](https://user-images.githubusercontent.com/69004164/211883068-53cab6ea-d8a8-4701-b003-e6fe2e779c68.png)
  
**Solutions**
```
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key = lambda x: (x[0],x[1]))
        res = []
        res.append(intervals[0])
        
        for i in range(1, len(intervals)):
            curr = intervals[i]
            last = res[-1]
            if curr[0] <= last[1]:
                last[1] = max(last[1], curr[1])
            else:
                res.append(curr)
        return res
```
时间复杂度：O(nlog n) ，有一个快排
空间复杂度：O(n)，有一个快排，最差情况(倒序)时，需要n次递归调用。因此确实需要O(n)的栈空间
