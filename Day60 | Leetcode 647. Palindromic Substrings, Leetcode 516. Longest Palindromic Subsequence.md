## 647. Palindromic Substrings ##

**Link:** [647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/description/)

**Thought:**

![image](https://user-images.githubusercontent.com/69004164/216849238-18df5279-9510-40b0-b6cb-c2aaf63e5eb9.png)
![image](https://user-images.githubusercontent.com/69004164/216849246-13fad174-1876-4189-b9fe-6a37c0b50682.png)
![image](https://user-images.githubusercontent.com/69004164/216849261-c1890ecc-310a-4fb2-9920-7f768f33876a.png)
![image](https://user-images.githubusercontent.com/69004164/216849297-16f3f932-b626-48f7-b166-2d3b3d232cda.png)
![image](https://user-images.githubusercontent.com/69004164/216849305-a27afb49-b5c7-46b7-bdd4-b75d39f1c0c9.png)

**Solution:**
```
class Solution:
    def countSubstrings(self, s: str) -> int:
        dp = [[False]* len(s) for _ in range(len(s))]
        res = 0
        for i in range(len(s) - 1, -1, -1):
            for j in range(i, len(s)):
                if s[i] == s[j]:
                    if j - i <= 1:
                        dp[i][j] = True
                        res += 1
                    elif dp[i+1][j-1]:
                        dp[i][j] = True
                        res += 1
        return res
```
## 516. Longest Palindromic Subsequence ##

**Link:** [516. Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/description/)

**Thought:**

![image](https://user-images.githubusercontent.com/69004164/216849564-c4fc9906-8da5-4cb4-8ef2-2b0d598f9c13.png)
![image](https://user-images.githubusercontent.com/69004164/216849577-50e37072-0cf3-4575-b8c9-1cff7b7024b1.png)
![image](https://user-images.githubusercontent.com/69004164/216849601-710035cb-609d-411d-b29d-74d33ef98f72.png)
![image](https://user-images.githubusercontent.com/69004164/216849611-5b51e532-5f09-433d-8f31-5270d7e0ec02.png)

**Solution:**
```
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        dp = [[0] * len(s) for _ in range(len(s))]

        for i in range(len(s)):
            dp[i][i] = 1
        
        for i in range(len(s) - 1, -1, -1):
            for j in range(i + 1, len(s)):
                if s[i] == s[j]:
                    dp[i][j] = dp[i+1][j-1] + 2
                else:
                    dp[i][j] = max(dp[i+1][j], dp[i][j-1])
        
        return dp[0][-1]
```





