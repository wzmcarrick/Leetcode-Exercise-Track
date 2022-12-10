## 24. Swap Nodes in Pairs

**Link:** [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/description/)

**Thoughts:** 

 - Both Recursive and Iterative
 - For Iterative: use dummy node: 1. store some pointers 2. reverse 3. update pointers
 - For Recursive: 1. base case 2. definition 

**Solution:**
Iterative: 
```
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(next = head)
        prev, curr = dummy, dummy.next

        while curr and curr.next:
            # store some pointers
            second = curr.next
            nxtPair = curr.next.next

            # reverse
            second.next = curr
            curr.next = nxtPair
            prev.next = second

            # update pointers

            prev = curr
            curr = nxtPair
        return dummy.next
```    

**Time Complexity:**  O(N)

**Spcae Complexity:**  O(1)

Recursive: 
```
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        
        if not head or not head.next:
            return head
        
        first = head
        second = head.next
        others = head.next.next

        second.next = first

        first.next = self.swapPairs(others)

        return second
```
**Time Complexity:**  O(N)

**Spcae Complexity:**  O(1)


## 19. Remove Nth Node From End of List

**Link:** [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)

**Thoughts:** 

 - Dummy Node, Two Pointers

**Solution:**
```
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        
        dummy = ListNode(next = head)

        s,f = dummy, dummy

        for i in range(n):
            f = f.next
        
        while f.next:
            f =  f.next
            s = s.next
        
        s.next = s.next.next

        return dummy.next
```    
**Time Complexity:**  O(N)

**Spcae Complexity:**  O(1)


## 160. Intersection of Two Linked Lists

**Link:** [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)

**Thoughts:** 

 - Use hashset -> need extra space
 - Use pointer -> no extra space need: find the length for both linkedlist, and let the pointer in short list shift until both length is equal, then check
  
  
**Solutions:** 
Hashset: 
```
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        
        # create a hashset
        hashset = set()
        # store all nodes in headA to hashset
        curA = headA
        while curA:
            hashset.add(curA)
            curA = curA.next
        # check nodes in headB, if in hashset then return, else means no intersection, return null
        curB = headB
        while curB:
            if curB in hashset: return curB
            curB = curB.next
        return None
```
**Time Complexity:**  O(m + n)

**Spcae Complexity:**  O(m)

Pointer: 
```
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        
        cur = headA # 求链表A的长度
        lenA = 0
        while cur:
            cur = cur.next
            lenA += 1
        
        cur = headB # 求链表B的长度
        lenB = 0
        while cur:
            cur = cur.next
            lenB += 1
        
        curA = headA
        curB = headB
        if lenB < lenA: # 让curB为最长链表的头，lenB为其长度
            curA, curB = curB, curA
            lenA, lenB = lenB, lenA 
        
        for i in range(lenB - lenA): # 让curA和curB在同一起点上（末尾位置对齐）
            curB = curB.next
        
        while curB: #  遍历curA 和 curB，遇到相同则直接返回
            if curA == curB: return curB
            curA = curA.next
            curB = curB.next
        return None
```
**Time Complexity:**  O(m + n)

**Spcae Complexity:**  O(1)


## 142. Linked List Cycle II

**Link:** [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/description/)

**Thoughts:** 

 - Two Pointers: 
 - if there is a cycle, `slow` and `fast` pointer will be encountered eventually
 - but the encountered point may not be the start of the cycle
 - move one pointer back to `head`, and shift both pointers equally
 - the point where they meet this time will be the start of the cycle
 ![image](https://user-images.githubusercontent.com/69004164/206877512-1225c158-ecbc-4445-bd06-8fc93dda1048.png)
 ![image](https://user-images.githubusercontent.com/69004164/206877515-6cd79cb4-0ae7-4b3f-9143-c2680efef3e5.png)
 
**Solution:**
```
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        s,f = head,head
        
        while f and f.next: # make sure fast.next is not null because fast shifts by two 
            s = s.next
            f = f.next.next

            if s == f: break
        # fast 遇到空指针说明没有环
        if not f or not f.next:
            return None

        f = head
        
        while s != f:
            s = s.next
            f = f.next
        
        return s
```

**Time Complexity:**  O(n)

**Spcae Complexity:**  O(1)
