## 344. Reverse String

**Link:**[344. Reverse String](https://leetcode.com/problems/reverse-string/description/)

**Thoughts:**
  - Two pointers
    
**Solution:**
```
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        l,r = 0, len(s) - 1
        while l < r: 
            temp = s[l]
            s[l] = s[r]
            s[r] = temp
            l += 1
            r -= 1
```

**Time Complexity:** O(n)


**Space Complexity** O(1)


## 541. Reverse String II

**Link:**[541. Reverse String II](https://leetcode.com/problems/reverse-string-ii/description/)

**Thoughts:**
  - 当需要固定规律一段一段去处理字符串的时候，要想想在在for循环的表达式上做做文章
  - Even no need to consider `If there are less than 2k but greater than or equal to k characters` in python because python slice will handle it automatically.
  - In python, `string` is immutable, cannot change the content of the string. Thus, we convert to `list`
    
    
**Solution:**
```
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        
        res = list(s)
        for i in range(0,len(s),2*k):
            res[i:i+k] = reversed(res[i:i+k])
        
        return ''.join(res)
```

**Time Complexity:** O(mn) 


**Space Complexity** O(n)


## 剑指 Offer 05. 替换空格

**Link:**[剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/description/)

**Thoughts:**
  - Iterate through list, find `' '` and replace with `'%20'`
    
**Solution:**
```
class Solution:
    def replaceSpace(self, s: str) -> str:
        res = []
        for c in s:
            if c == " ": res.append('%20')
            else:
                res.append(c)
        return ''.join(res)
```

**Time Complexity:** O(n)


**Space Complexity** O(n)


## 151. Reverse Words in a String

**Link:**[151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/description/)

**Thoughts:**
  - Remove the extra space
  - Reverse the whole array
  - Reverse each word
    
    
**Solution:**
```
class Solution:
    def reverseWords(self, s: str) -> str:

        #1.去除多余的空格
        l,r = 0, len(s) - 1

        while l <= r and s[l] == ' ':
            l += 1
        while l <= r and s[r] == ' ':
            r -= 1
        
        arr = []

        while l <= r:
            if s[l] != ' ':
                arr.append(s[l])
            elif arr[-1] != ' ':
                arr.append(s[l])
            l += 1

        #2.翻转字符数组
        left, right = 0, len(arr) - 1
        self.reverse(arr,left,right)

        #3.翻转每个单词
        i = 0
        while i < len(arr):
            for j in range(i,len(arr)):
                if j == len(arr) - 1 or arr[j + 1] == ' ':
                    self.reverse(arr,i,j)
                    i = j+2

        return ''.join(arr)

    def reverse(self, arr, left, right):     
            while left < right:
                arr[left],arr[right] = arr[right],arr[left]
                left += 1
                right -= 1
```

**Time Complexity:** O(n)


**Space Complexity** O(n)

## 剑指 Offer 58 - II. 左旋转字符串

**Link:**[剑指 Offer 58 - II. 左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

**Thoughts:**
  - 
    
    
**Solution:**
```
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        res = []
        for i in range(n,len(s)):
            res.append(s[i])
        for i in range(n):
            res.append(s[i])
        
        return ''.join(res)
```

**Time Complexity:** O(n)


**Space Complexity** O(n)
