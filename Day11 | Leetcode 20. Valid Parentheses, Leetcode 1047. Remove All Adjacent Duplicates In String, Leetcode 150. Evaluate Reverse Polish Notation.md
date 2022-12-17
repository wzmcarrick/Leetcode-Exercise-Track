## 20. Valid Parentheses ##

**Link:**[20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)

**Thoughts:**
  1. Initialize a stack S.
  2. Process each bracket of the expression one at a time.
  3. If we encounter an opening bracket, we simply push it onto the stack. This means we will process it later, let us simply move onto the sub-expression ahead.
  4. If we encounter a closing bracket, then we check the element on top of the stack. If the element at the top of the stack is an opening bracket of the same type, then we pop it off the stack and continue processing. Else, this implies an invalid expression.
  5. In the end, if we are left with a stack still having elements, then this implies an invalid expression.

    
**Solution:**
```
class Solution:
    def isValid(self, s: str) -> bool:
        dic = {'(':')','[':']','{':'}'}
        stack = []

        for e in s:
            if e in dic.keys():
                stack.append(e)
            elif len(stack) == 0 or dic[stack.pop()] != e:
                return False
        return len(stack) == 0
```

**Time Complexity:**  O(n) because we simply traverse the given string one character at a time and push and pop operations on a stack take O(1) time.

**Spcae Complexity:**  O(n) as we push all opening brackets onto the stack and in the worst case, we will end up pushing all the brackets onto the stack. 


## 1047. Remove All Adjacent Duplicates In String

**Link:**[1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/description/)

**Thoughts:**
  1. Initiate an empty output stack.

  2. Iterate over all characters in the string.

    - Current element is equal to the last element in stack? Pop that last element out of stack.

    - Current element is not equal to the last element in stack? Add the current element into stack.

  3. Convert stack into string and return it.


**Solution:**
```
class Solution:
    def removeDuplicates(self, s: str) -> str:
        stack = []

        for i in range(len(s)):
            if len(stack) == 0:
                stack.append(s[i])
            elif s[i] != stack[- 1]:
                stack.append(s[i])
            elif s[i] == stack[- 1]:
                stack.pop()
        return ''.join(stack)
```

**Time Complexity:**  O(n) where N is a string length.

**Spcae Complexity:** O(n - d) where D is a total length for all duplicates.



## 150. Evaluate Reverse Polish Notation

**Link:**[150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

**Thoughts:**
  1. / is regular division(returns float) and // is floor division(returns to nearest and lower int).

    eg:
    x = 5/2 #2.5 
    y = 5//2 #2
    a = -11/2 #-5.5
    b = -11//2 #-6

**Solution:**
```
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        operators = ['+','-', '*','/']
        for i in range(len(tokens)):
            if tokens[i] not in operators:
                stack.append(int(tokens[i]))
            else:
                first = int(stack.pop())
                seconde = int(stack.pop())
                if tokens[i] == '+':
                    result = first + seconde
                elif tokens[i] == '-':
                    result = seconde - first
                elif tokens[i] == '*':
                    result = seconde * first
                elif tokens[i] == '/':
                    result = int(seconde / first)

                stack.append(result)
        return stack.pop()
```

**Time Complexity:**  O(n) We do a linear search to put all numbers on the stack, and process all operators. Processing an operator requires removing 2 numbers off the stack and replacing them with a single number, which is an O(1) operation. Therefore, the total cost is proportional to the length of the input array. Unlike before, we're no longer doing expensive deletes from the middle of an Array or List.

**Spcae Complexity:** O(n) In the worst case, the stack will have all the numbers on it at the same time. This is never more than half the length of the input array.


