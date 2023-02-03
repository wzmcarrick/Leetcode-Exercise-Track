## 583. Delete Operation for Two Strings ##

**Link:**[583. Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/description/)

**Thought:**
画图！！！！画dp图！！根据dp图理解
dp[i][j] => i is row, j in column, can decide i is word1 or word2
when creating dp = [[]]: the inner is the length of column + 1, the outer is the length of row + 1
![image](https://user-images.githubusercontent.com/69004164/216709395-c09e077e-6c7a-4d5d-9b6c-d3bf01ebbe3c.png)
![image](https://user-images.githubusercontent.com/69004164/216709916-2f120ab2-a77e-4343-8406-d2792aa9e7df.png)

**Solution:**
```
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0] * (len(word2) + 1) for _ in range(len(word1) + 1)]

        for i in range(len(word1) + 1):
            dp[i][0] = i
        for j in range(len(word2) + 1):
            dp[0][j] = j
        
        for j in range(1, len(word2) + 1):
            for i in range(1, len(word1) + 1):
                if word1[i - 1] == word2[j - 1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i - 1][j] + 1, dp[i][j - 1] + 1)
        
        return dp[-1][-1]
```

## 72. Edit Distance ##

**Link:**[72. Edit Distance](https://leetcode.com/problems/edit-distance/description/)

**Thought:**
画图！！！！画dp图！！根据dp图理解
dp[i][j] => i is row, j in column, can decide i is word1 or word2
when creating dp = [[]]: the inner is the length of column + 1, the outer is the length of row + 1
![image](https://user-images.githubusercontent.com/69004164/216710153-c18f4c76-5713-4c0f-b656-bca7d1f6c198.png)
![image](https://user-images.githubusercontent.com/69004164/216710222-0d0cc5c8-c2b9-4518-a540-f7225a420bd1.png)
![image](https://user-images.githubusercontent.com/69004164/216710436-70f7251b-13dc-4fa1-a471-19f17addd5f8.png)
![image](https://user-images.githubusercontent.com/69004164/216710479-0a112d79-b2fb-433a-a880-e1481c731970.png)
![image](https://user-images.githubusercontent.com/69004164/216710518-648f275b-2ead-423a-a9be-cc7c6370ddd0.png)

**Solution:**
```
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0] * (len(word1) + 1) for _ in range(len(word2) + 1)]

        for i in range(len(word2) + 1):
            dp[i][0] = i
        for j in range(len(word1) + 1):
            dp[0][j] = j
        
        for i in range(1, len(word2) + 1):
            for j in range(1, len(word1) + 1):
                if word1[j-1] == word2[i-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j-1] + 1, dp[i-1][j] + 1, dp[i][j-1] + 1)

        return dp[-1][-1]
```
