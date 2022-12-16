## 232. Implement Queue using Stacks ##

**Link:**[232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/description/)

**Thoughts:**
  - Stack: First In Last Out
  - Queue: First In Frist In
  - Need to use two Stacks:
  - ![image](https://user-images.githubusercontent.com/69004164/208151084-852e6c74-a5d9-4eb2-a084-77b3d0b5e130.png)
  - ![image](https://user-images.githubusercontent.com/69004164/208151111-85ab472b-2a34-41c2-b388-c1dea5a8bf01.png)

    
**Solution:**
```
class MyQueue:

    def __init__(self):
        self.stack1 = []
        self.stack2 = []

    def push(self, x: int) -> None:
        self.stack1.append(x)
       
    def pop(self) -> int:
        self.move()
        return self.stack2.pop()

    def peek(self) -> int:
        self.move()
        return self.stack2[-1]

    def empty(self) -> bool:
        return len(self.stack2) == 0 and len(self.stack1) == 0
    
    def move(self):
        if len(self.stack2) == 0:
            while len(self.stack1) != 0:
                self.stack2.append(self.stack1.pop())


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```


## 225. Implement Stack using Queues

**Link:**[225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)

**Thoughts:**
  - Stack: First In Last Out
  - Queue: First In Frist In
  - Only use one queue: 
  - ![image](https://user-images.githubusercontent.com/69004164/208152077-48e4c5c5-a620-4fa3-b2b0-9905762551b9.png)
  - ![image](https://user-images.githubusercontent.com/69004164/208152112-5bdfc2e7-ae4f-4eb0-bae2-2c757f7644f4.png)


**Solution:**
```
class MyStack:

    def __init__(self):
        self.queue = collections.deque()

    def push(self, x: int) -> None:
        self.queue.append(x)
        for _ in range(len(self.queue) - 1):
            self.queue.append(self.queue.popleft())


    def pop(self) -> int:
        return self.queue.popleft()

    def top(self) -> int:
        return self.queue[0]

    def empty(self) -> bool:
        return len(self.queue) == 0
        


# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```
