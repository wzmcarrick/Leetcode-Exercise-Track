## 93. Restore IP Addresses ##

**Link:**[93. Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/description/)

**Thoughts:**
  - 切割问题
  - 本质切割问题使用回溯搜索法，本题只能切割三次，所以纵向递归总共四层，因为不能重复分割，所以需要start_index来记录下一层递归分割的起始位置
  - 添加变量point_num来记录逗号的数量[0,3]

**Solution:**
```
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        '''
        本质切割问题使用回溯搜索法，本题只能切割三次，所以纵向递归总共四层
        因为不能重复分割，所以需要start_index来记录下一层递归分割的起始位置
        添加变量point_num来记录逗号的数量[0,3]
        '''
        result = []
        

        def backtrack(path, start, pointNum):
            if pointNum == 3:
                if self.isValid(path,start,len(path) - 1):
                    result.append(path[:])
                return
            
            for i in range(start, len(path)):
                if self.isValid(path,start,i):
                    path = path[:i+1] + '.' + path[i+1:]
        
                    # pointNum += 1
                    backtrack(path,i+2,pointNum + 1)
                    # pointNum -= 1
                    path = path[:i+1] + path[i+2:]
                else:
                    break
        backtrack(s, 0, 0)
        return result

    def isValid(self, string, start, end):
        if start > end:
            return False
        if string[start] == '0' and start != end:
            return False
        if not 0 <= int(string[start:end+1]) <= 255:
            return False
        
        return True

```
![image](https://user-images.githubusercontent.com/69004164/210454295-0b70ddbc-d710-4d32-b880-3a2cf6356292.png)


## 78. Subsets ##

**Link:**[78. Subsets](https://leetcode.com/problems/subsets/description/)

**Thoughts:**
  - 子集问题和组合问题、分割问题的的区别: 子集是收集树形结构中树的所有节点的结果。而组合问题、分割问题是收集树形结构中叶子节点的结果
  - 无序，取过的元素不会重复取，写回溯算法的时候，for就要从startIndex开始，而不是从0开始
  - 求排列问题的时候，就要从0开始，因为集合是有序的，{1, 2} 和{2, 1}是两个集合

**Solution:**
```
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        res = []
        path = []

        def backtrack(path,start):
            res.append(path[:])

            for i in range(start, len(nums)):
                path.append(nums[i])
                backtrack(path, i + 1)
                path.pop()
        
        backtrack(path, 0)
        return res
```
![image](https://user-images.githubusercontent.com/69004164/210455748-97bd95f0-9652-4a0d-b5ef-3cc796b46d06.png)


## 90. Subsets II ##

**Link:**[90. Subsets II](https://leetcode.com/problems/subsets-ii/description/)

**Thoughts:**
  - 注意去重需要先对集合排序
  - 本题也可以不使用used数组来去重，因为递归的时候下一个startIndex是i+1而不是0。如果要是全排列的话，每次要从0开始遍历，为了跳过已入栈的元素，需要使用used。
  - ![image](https://user-images.githubusercontent.com/69004164/210457898-68db4945-be0f-449d-81eb-ce4df28a9296.png)

**Solution:**
```
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        path = []
        res =[]
        nums.sort()
        used = [0] * len(nums)

        def backtrack(path, start):
            res.append(path[:])
            
            for i in range(start, len(nums)):
                if i > start and nums[i] == nums[i-1]:
                    continue
                path.append(nums[i])
                backtrack(path,i+1)
                path.pop()
        
        backtrack(path,0)
        return res
```
![image](https://user-images.githubusercontent.com/69004164/210458465-144054fd-19b9-443e-a423-fa7f75b5cfd8.png)

