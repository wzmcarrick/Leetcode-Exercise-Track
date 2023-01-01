## 216. Combination Sum III ##

**Link: **[216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/description/)

**Thoughts:**
  - Backtrack


**Solutions**
```
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        path = []
        res = []
        curSum = 0

        def backtrack(path, start, curSum):
            if len(path) == k and curSum == n:
                res.append(path[:])
                return
            
            for i in range(start,10):
                path.append(i)
                curSum += i
                backtrack(path,i+1,curSum)
                path.pop()
                curSum -= i
        
        backtrack(path,1,curSum)
        return res
```
![image](https://user-images.githubusercontent.com/69004164/210157450-2983f810-e571-43b6-8632-e16bd7b8b0db.png)


## 17. Letter Combinations of a Phone Number ##

**Link: **[17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

**Thoughts:**
  - Backtrack


**Solutions**
```
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if digits == '':
            return []
        path = []
        res = []
        dic = {
            2:'abc',
            3:'def',
            4:'ghi',
            5:'jkl',
            6:'mno',
            7:'pqrs',
            8:'tuv',
            9:'wxyz'
        }

        def backtrack(path, start):
            if len(path) == len(digits):
                res.append(''.join(path[:]))
                return
            
            for i in range(start, len(digits)):
                for ch in dic[int(digits[start])]:
                    path.append(ch)
                    backtrack(path,i+1)
                    path.pop()
        backtrack(path,0)
        return res
```
![image](https://user-images.githubusercontent.com/69004164/210158034-05e838d7-35c8-4396-8fc1-fc959b2eed41.png)
