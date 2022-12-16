## 28. Find the Index of the First Occurrence in a String

**Link:**[28. Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)

**Thoughts:**
  - [KMP string searching algorithms (video)](https://www.youtube.com/watch?v=JoF0Z7nVSrA)
  - [字符串匹配的KMP算法](https://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html)
    
**Solution:**
```
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        # KMP
        # initialize LPS array:
        lps = [0] * len(needle)

        # get LPS array:

        prevLPS, i = 0, 1

        while i < len(needle):
            if needle[i] == needle[prevLPS]:
                lps[i] = prevLPS + 1
                prevLPS += 1
                i += 1
            elif prevLPS == 0:
                lps[i] = 0
                i += 1
            else:
                prevLPS = lps[prevLPS - 1]
        
        # search
        i = 0  # pointer for haystack
        j = 0  # pointer for needle

        while i < len(haystack):
            if haystack[i] == needle[j]:
                i += 1
                j += 1
            else:
                if j == 0:
                    i += 1
                else: 
                    j = lps[j - 1]
            
            if j == len(needle):
                return i - j
        return -1
```

**Time Complexity:** O(m+n)

**Space Complexity** O(n)


## 459. Repeated Substring Pattern

**Link:**[459. Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/)

**Thoughts:**
  - Waiting for second round
