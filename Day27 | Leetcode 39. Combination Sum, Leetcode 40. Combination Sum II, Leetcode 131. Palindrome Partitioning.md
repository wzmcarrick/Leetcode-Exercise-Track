## 39. Combination Sum ##

**Link:**[39. Combination Sum](https://leetcode.com/problems/combination-sum/description/)

**Thoughts:**
  - **无重复元素** 的数组 candidates
  - candidates 中的数字可以**无限制重复被选取**
  - 本题还需要startIndex来控制for循环的起始位置，对于组合问题，什么时候需要startIndex呢？
    我举过例子，如果是一个集合来求组合的话，就需要startIndex，例如：77.组合 (opens new window)，216.组合总和III (opens new window)。
    如果是多个集合取组合，各个集合之间相互不影响，那么就不用startIndex，例如：17.电话号码的字母组合(opens new window)
    注意以上我只是说求组合的情况，如果是排列问题，又是另一套分析的套路，后面我再讲解排列的时候就重点介绍。

**Solution:**
```
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        path = []
        res = []
        curSum = 0

        def backtrack(path,curSum,start):
            if curSum == target:
                res.append(path[:])
                return
            
            if curSum > target:
                return
            
            for i in range(start, len(candidates)):
                path.append(candidates[i])
                curSum += candidates[i]
                backtrack(path,curSum,i)
                curSum -= candidates[i]
                path.pop()
                
        
        backtrack(path,curSum,0)
        return res
```
![image](https://user-images.githubusercontent.com/69004164/210285916-12ae97fb-32eb-48f3-a9b6-daa60b966899.png)

## 40. Combination Sum II ##

**Link:**[40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/description/)

**Thoughts:**
  - 这道题目和39.组合总和 (opens new window)如下区别：
    本题candidates 中的每个数字在每个组合中只能使用一次。
    本题数组candidates的元素是有重复的，而39.组合总和 (opens new window)是无重复元素的数组candidates
    最后本题和39.组合总和 (opens new window)要求一样，解集不能包含重复的组合。
  - 本题的难点在于区别2中：集合（数组candidates）有重复元素，但还不能有重复的组合。
  - 强调一下，树层去重的话，需要对数组排序！
  - 前面我们提到：要去重的是“同一树层上的使用过”，如何判断同一树层上元素（相同的元素）是否使用过了呢。

    如果candidates[i] == candidates[i - 1] 并且 used[i - 1] == false，就说明：前一个树枝，使用了candidates[i - 1]，也就是说同一树层使用过candidates[i - 1]。

**Solution:**
```
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        path = []
        res = []
        curSum = 0
        
        used = [False for __ in range(len(candidates))]

        # must not contain duplicate combinations: Need to sort first
        candidates.sort()

        def backtrack(path,start,curSum,used):
            if curSum == target:
                res.append(path[:])
                return
            if curSum > target:
                return
            
            for i in range(start, len(candidates)):
                if i > start and candidates[i] == candidates[i-1] and used[i-1] == False:
                    continue
                used[i] = True
                path.append(candidates[i])
                curSum += candidates[i]
                backtrack(path, i + 1, curSum,used)
                used[i] = False
                curSum -= candidates[i]
                path.pop()
            
        backtrack(path,0,curSum,used)
        return res
```
![image](https://user-images.githubusercontent.com/69004164/210286144-8f9a7c22-b0e1-4d68-ac00-7181040acee1.png)


## 131. Palindrome Partitioning ##

**Link:**[131. Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/description/)

**Thoughts:**
  - 递归用来纵向遍历，for循环用来横向遍历，切割线（就是图中的红线）切割到字符串的结尾位置，说明找到了一个切割方法。
  - 注意切割过的位置，不能重复切割，所以，backtracking(s, i + 1); 传入下一层的起始位置为i + 1
  - ![image](https://user-images.githubusercontent.com/69004164/210286227-aa44b8c5-ce8d-457c-b41c-35a37831cfb7.png)

**Solution:**
```
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        path = []
        res = []

        def backtrack(path,start):
            if start >= len(s):
                res.append(path[:])
                return
            
            for i in range(start, len(s)):
                temp = s[start:i+1]
                if temp == temp[::-1]:
                    path.append(temp)
                else:
                    continue
                backtrack(path,i+1)
                path.pop()

        backtrack(path,0)
        return res
```
![image](https://user-images.githubusercontent.com/69004164/210286376-e6e19822-56ed-443a-91a3-28ec41fee1ca.png)
