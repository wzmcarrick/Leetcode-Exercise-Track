## 704 Binary Search

**Link:** [704 Binary Search](https://leetcode.com/problems/binary-search/)

**Thoughts:** 

 - If we set r = len(nums) - 1, then we need to use l<=r in the while loop: (both end inclusive)

```
    l,r = 0, len(nums) -1
    
    while l <= r:    
```  
```
    mid = r - 1
    mid = l + 1
```
       

**Time Complexity:**  O(logN)

**Spcae Complexity:**  O(1)
