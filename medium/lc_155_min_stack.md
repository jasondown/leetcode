**155. Min Stack**

**Medium**

[View on Leetcode](https://leetcode.com/problems/min-stack/)

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:

- MinStack() initializes the stack object.
- `void push(int val)` pushes the element val onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

**Example 1**:

>
    Input
    ["MinStack","push","push","push","getMin","pop","top","getMin"]
    [[],[-2],[0],[-3],[],[],[],[]]

    Output
    [null,null,null,null,-3,null,0,-2]

    Explanation
    MinStack minStack = new MinStack();
    minStack.push(-2);
    minStack.push(0);
    minStack.push(-3);
    minStack.getMin(); // return -3
    minStack.pop();
    minStack.top();    // return 0
    minStack.getMin(); // return -2

**Constraints**:

- `-231 <= val <= 231 - 1`
- Methods `pop`, `top` and `getMin` operations will always be called on non-empty stacks.
- At most `3 * 104` calls will be made to `push`, `pop`, `top`, and `getMin`.

**Solution**:

Use two correlated stacks. One for the current value and one for the minimum value at that point in time. As we add values, we see if the new value is less than the current minimum value. If so, update the min stack to have the new value, otherwise, append the same minimum value.

```python
class MinStack(object):

    def __init__(self):
        self.stack = []
        self.min = []

    def push(self, val):
        """
        :type val: int
        :rtype: None
        """
        self.stack.append(val)
        val = min(val, self.min[-1] if self.min else val)    
        self.min.append(val)
        
    def pop(self):
        """
        :rtype: None
        """
        if self.stack: self.stack.pop()
        if self.min: self.min.pop()

    def top(self):
        """
        :rtype: int
        """
        return self.stack[-1] if self.stack else 0

    def getMin(self):
        """
        :rtype: int
        """
        return self.min[-1] if self.min else 0


# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(val)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()

# input1 = ["MinStack","push","push","push","getMin","pop","top","getMin"]
# input2 = [[],[-2],[0],[-3],[],[],[],[]]
ms = MinStack()
ms.push(-2)
ms.push(0)
ms.push(-3)
ms.getMin() # -3
ms.pop()
ms.top() # 0
ms.getMin() # -2
```

- **Time Complexity**: O(1) for each operation
- **Space Complexity**: O(n) (for each stack)
