## 454. 4Sum II

**Link:**[454. 4Sum II](https://leetcode.com/problems/4sum-ii/description/)

**Thoughts:**
  - HashTable
  - Store the sum of first two arrays to a hashmap, key is the sum, value is the occurance of the sum
  - Iterate the last two arrays, check the sum of last two arrays, if `-c-d` is in the hashmap, that means the total sum of four groups is 0, then add count by the occurance 
  
  1. For each a in A.
     - For each b in B.
       - If a + b exists in the hashmap m, increment the value.
       - Else add a new key a + b with the value 1.
  2. For each c in C.
     - For each d in D.
       - Lookup key -(c + d) in the hashmap m.
       - Add its value to the count cnt.
  3. Return the count cnt.
    
    
**Solution:**
```
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        sum1 = {}                    # define a hashamap to store the occurance sum of first two arrays
        for n in nums1:              # iterate through first array and second array
            for m in nums2:
                sum1[n+m] = sum1.get(n+m,0) + 1
        
        count = 0                    # initilize count as 0

        for n in nums3:              # iterate the third and fourth array 
            for m in nums4:
                if -n-m in sum1.keys():  # if in hashmap: add the occurance to count
                    count += sum1[-n-m]
        
        
        return count
```

**Time Complexity:** O(n^2) We have 2 nested loops to count sums, and another 2 nested loops to find complements.


**Space Complexity** O(n^2) for the hashmap. There could be up to O(n^2) distinct a + b keys.


## 383. Ransom Note

**Link:**[383. Ransom Note](https://leetcode.com/problems/ransom-note/description/)

**Thoughts:**
  - HashTable: two options: use hashmap or array
  - Use hashmap will cost more space and time, so better use array for effeciency
  - First store magazine count, since it is larger scope
  - Then minus the ransomNote count, if negative found, means more characaters need in ransomNote, return False
    
    
**Solution:**
```
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        
        count_ = [0] * 26

        for m in magazine:
            count_[ord(m) - ord('a')] += 1
        for r in ransomNote:
            count_[ord(r) - ord('a')] -= 1
        
        for i in count_:
            if i < 0: return False
        return True
```

**Time Complexity:** O(m) 


**Space Complexity** O(26)


## 15. 3Sum

**Link:**[15. 3Sum](https://leetcode.com/problems/3sum/description/)

**Thoughts:**
  - Sort the array first
  - Fix a value first, calculate the remaining value, then use two pointer to find the remaining value
  - Remove Dup result:
  - the fixed value remove dup `if i > 0 and nums[i] == nums[i - 1]: continue`
  - when find a solution, remove dup for last two element: `while l< r and nums[l] == nums[l - 1]: l += 1`
    
    
**Solution:**
```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        
        res = []
        nums.sort()

        for i in range(len(nums) - 2):
            if i > 0 and nums[i] == nums[i - 1]: continue
            remain = -nums[i]
            l = i+1
            r = len(nums) - 1
            while (l < r):
                twoSum = nums[l] + nums[r]
                if twoSum == remain: 
                    res.append([nums[i],nums[l],nums[r]])
                    l += 1
                    while l< r and nums[l] == nums[l - 1]: 
                        l += 1
                elif twoSum > remain:
                    r -= 1
                else:
                    l += 1
        return res
```

**Time Complexity:** O(n^2) Sorting the array takes O(nlog⁡n), so overall complexity is O(nlog⁡n+n^2). This is asymptotically equivalent to O(n^2).


**Space Complexity** from O(log⁡n) to O(n), depending on the implementation of the sorting algorithm. For the purpose of complexity analysis, we ignore the memory required for the output.


## 18. 4Sum

**Link:**[18. 4Sum](https://leetcode.com/problems/4sum/description/)

**Thoughts:**
  - Sort the array first
  - Fix a value first, calculate the remaining value, fix another value, calculate the remaining value, then use two pointer to find the remaining value
  - Remove Dup result:
  - the fixed value remove dup `if i > 0 and nums[i] == nums[i - 1]: continue`
  - when find a solution, remove dup for last two element: `while l< r and nums[l] == nums[l - 1]: l += 1`
    
    
**Solution:**
```
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        res = []
        nums.sort()

        if len(nums) < 4: return []

        for i in range(len(nums)-3):
            if i > 0 and nums[i] == nums[i - 1]: continue
            remainThreeSum = target - nums[i]
            for j in range(i+1, len(nums) - 2):
                if j > i+1 and nums[j] == nums[j - 1]: continue
                remainTwoSum = remainThreeSum - nums[j]
                l,r = j+1,len(nums) - 1
                while l < r:
                    twoSum = nums[l] + nums[r]
                    if remainTwoSum == twoSum:
                        res.append([nums[i],nums[j],nums[l],nums[r]])
                        l += 1
                        while l < r and nums[l] == nums[l - 1]: 
                            l += 1
                    elif twoSum > remainTwoSum:
                        r -= 1
                    else: 
                        l += 1
        return res
```

**Time Complexity:** O(n^3)


**Space Complexity** O(n)

