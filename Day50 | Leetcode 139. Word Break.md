## 139. Word Break ##

**Link:**[139. Word Break](https://leetcode.com/problems/word-break/description/)

**Thoughts:**

- 单词就是物品，字符串s就是背包，单词能否组成字符串s，就是问物品能不能把背包装满。

- 拆分时可以重复使用字典中的单词，说明就是一个完全背包！

1. 确定dp数组以及下标的含义
dp[i] : 字符串长度为i的话，dp[i]为true，表示可以拆分为一个或多个在字典中出现的单词。

2. 确定递推公式
  如果确定dp[j] 是true，且 [j, i] 这个区间的子串出现在字典里，那么dp[i]一定是true。（j < i ）。

  所以递推公式是 if([j, i] 这个区间的子串出现在字典里 && dp[j]是true) 那么 dp[i] = true。

3. dp数组如何初始化
  从递推公式中可以看出，dp[i] 的状态依靠 dp[j]是否为true，那么dp[0]就是递推的根基，dp[0]一定要为true，否则递推下去后面都都是false了。

  那么dp[0]有没有意义呢？

  dp[0]表示如果字符串为空的话，说明出现在字典里。

  但题目中说了“给定一个非空字符串 s” 所以测试数据中不会出现i为0的情况，那么dp[0]初始为true完全就是为了推导公式。

  下标非0的dp[i]初始化为false，只要没有被覆盖说明都是不可拆分为一个或多个在字典中出现的单词。

4. 确定遍历顺序
题目中说是拆分为一个或多个在字典中出现的单词，所以这是完全背包。

还要讨论两层for循环的前后顺序。

如果求组合数就是外层for循环遍历物品，内层for遍历背包。

如果求排列数就是外层for遍历背包，内层for循环遍历物品。

而本题其实我们求的是排列数，为什么呢。 拿 s = "applepenapple", wordDict = ["apple", "pen"] 举例。

"apple", "pen" 是物品，那么我们要求 物品的组合一定是 "apple" + "pen" + "apple" 才能组成 "applepenapple"。

"apple" + "apple" + "pen" 或者 "pen" + "apple" + "apple" 是不可以的，那么我们就是强调物品之间顺序。

所以说，本题一定是 先遍历 背包，再遍历物品。

5. 举例推导dp[i]
以输入: s = "leetcode", wordDict = ["leet", "code"]为例，dp状态如图：
![image](https://user-images.githubusercontent.com/69004164/214622392-e5f0c340-3347-4dd9-a799-fca80bfddd88.png)

**Solution:**
```
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        dp = [0] * (len(s)+1)

        dp[0] = 1
        # dp[i]: means when the length of string s is i, whether the word in worDict can fill in that string
        # dp[j] = True if dp[i] = True and substring[i,j] is in wordDict
        # dp[0] = True
        # first iterate through length of string, then iterate through wordDict because it is permutation

        for i in range(1, len(s)+1):
            for j in range(len(wordDict)):
                if i >= len(wordDict[j]):
                    subStr = s[i - len(wordDict[j]):i]
                    if dp[i-len(wordDict[j])] == 1 and subStr == wordDict[j]:
                        dp[i] = 1
        return dp[-1]
```
时间复杂度：O(n^3)，因为substr返回子串的副本是O(n)的复杂度（这里的n是substring的长度）
空间复杂度：O(n)
