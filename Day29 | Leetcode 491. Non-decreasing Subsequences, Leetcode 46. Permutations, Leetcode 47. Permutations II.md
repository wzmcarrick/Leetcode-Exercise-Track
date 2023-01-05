## 491. Non-decreasing Subsequences ##

**Link:**[491. Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences/description/?show=1)

**Thoughts:**
  - 同一父节点下的同层上使用过的元素就不能再使用
  - 深度遍历中每一层都会有一个全新的usage_list用于记录本层元素是否重复使用
  - 若当前元素值小于前一个时（非递增）或者曾用过，跳入下一循环
  - ![image](https://user-images.githubusercontent.com/69004164/210681479-437ad87a-f784-4c6d-a532-da523d6f4eea.png)

**Solution:**
```
class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        path = []
        res = []

        def backtrack(path, start):
 
            if len(path) >= 2:
                res.append(path[:])
            
            if start >= len(nums):
                return

            used = set()

            for i in range(start,len(nums)):
            
                if path and nums[i] < path[-1]:
                    continue
                if nums[i] in used:
                    continue
                used.add(nums[i])
                path.append(nums[i])
                backtrack(path, i + 1)
                path.pop()
        
        backtrack(path, 0)
        return res
```
![image](https://user-images.githubusercontent.com/69004164/210682017-a9b05a4a-01c8-4763-80f1-acd0dab210d6.png)


## 46. Permutations ##

**Link:**[46. Permutations](https://leetcode.com/problems/permutations/description/)

**Thoughts:**
  - 排列问题：
    1. 每层都是从0开始搜索而不是startIndex
    2. 需要used数组记录path里都放了哪些元素了 or 通过判断path中是否存在数字，排除已经选择的数字
  - ![image](https://user-images.githubusercontent.com/69004164/210682268-a733d058-63e1-437f-93a4-2201113cbca9.png)


**Solution:**
```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        path = []
        res = []

        def backtrack(path):
            if len(path) == len(nums):
                res.append(path[:])
                return
            
            for i in range(0, len(nums)):
                if path and nums[i] in path:
                    continue
                path.append(nums[i])
                backtrack(path)
                path.pop()
        
        backtrack(path)
        return res
```
![image](https://user-images.githubusercontent.com/69004164/210682448-24d35d7c-eaa7-4cde-ba01-4d2c90ffdae8.png)

## 47. Permutations II ##

**Link:**[47. Permutations II](https://leetcode.com/problems/permutations-ii/description/)

**Thoughts:**
  - 去重一定要对元素进行排序
  - ![image](https://user-images.githubusercontent.com/69004164/210682993-7edddf76-0e1f-462c-9c4f-a260c3882059.png)

**Solution:**
```
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        path = []
        res = []
        nums.sort()
        used = [0] * len(nums)

        def backtrack(path, used):
            if len(path) == len(nums):
                res.append(path[:])
                return
            
            for i in range(0, len(nums)):
                if i > 0 and nums[i] == nums[i -1] and used[i-1] == 0:
                    continue
                if used[i] == 1:
                    continue
                path.append(nums[i])
                used[i] = 1
                backtrack(path, used)
                used[i] = 0
                path.pop()
        
        backtrack(path, used)
        return res
```
![image](https://user-images.githubusercontent.com/69004164/210683438-8fff1d27-c490-4333-adbe-15c2af514c45.png)
![image](https://user-images.githubusercontent.com/69004164/210683456-85e8685b-93f0-4939-88a6-0103a8dc0d64.png)
