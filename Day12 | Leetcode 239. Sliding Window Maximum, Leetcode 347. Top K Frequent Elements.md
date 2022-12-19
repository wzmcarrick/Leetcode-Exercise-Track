## 239. Sliding Window Maximum ##

**Link:**[239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/description/)

**Thoughts:**
  1. Monotonic queue - the order of the elements is strictly increasing or strictly decreasing
  2. 用来辅助解决滑动窗口相关最值问题

    
**Solution:**
```
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        deque = collections.deque()  # index
        res = []
        l = r = 0

        while r < len(nums):
            while len(deque) != 0 and nums[r] > nums[deque[-1]]:
                deque.pop()
            deque.append(r)
            
            if l > deque[0]:
                deque.popleft()
            
            if r >= k - 1:
                res.append(nums[deque[0]])
                l += 1
            r += 1
        return res

```

**Time Complexity:**  O(n) 

**Spcae Complexity:**  O(n) 


## 347. Top K Frequent Elements

**Link:**[347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/)

**Thoughts:**
  1. Use Bucket Sort, store the frequency as key, the number has that frequency as value, loop through the bucket sort list from last to first


**Solution:**
```
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        
        # 1. Use Bucket Sort, store the frequency as key, the number has that frequency as value, loop through the bucket sort list from last to first
        count = {}

        for n in nums:
            count[n] = count.get(n,0) + 1

        freq = [[] for i in range(len(nums) + 1)]

        for index, count in count.items():
            freq[count].append(index)

        res = []
        for i in range(len(freq) - 1, -1, -1):
            for n in freq[i]:
                res.append(n)
                if len(res) == k:
                    return res
```

**Time Complexity:**  O(n) 

**Spcae Complexity:** O(n) 
