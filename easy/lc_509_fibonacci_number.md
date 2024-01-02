**509. Fibonacci Number**

**Easy**

[View on Leetcode](https://leetcode.com/problems/fibonacci-number/)

The **Fibonacci numbers**, commonly denoted `F(n)` form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from `0` and `1`. That is,
>
    F(0) = 0, F(1) = 1
    F(n) = F(n - 1) + F(n - 2), for n > 1.

Given `n`, calculate `F(n)`.

**Example 1**:

>
    Input: n = 2
    Output: 1
    Explanation: F(2) = F(1) + F(0) = 1 + 0 = 1.

**Example 2**:

>
    Input: n = 3
    Output: 2
    Explanation: F(3) = F(2) + F(1) = 1 + 1 = 2.

**Example 3**:

>
    Input: n = 4
    Output: 3
    Explanation: F(4) = F(3) + F(2) = 2 + 1 = 3.

**Constraints**:
- `0 <= n <= 30`

**Recursive Solution**:

```python
class Solution:
    def fib(self, n: int) -> int:
        if n <= 1:
            return n

        return self.fib(n-1) + self.fib(n-2)
    
sol = Solution()

input1 = 2
expected1 = 1

input2 = 3
expected2 = 2

input3 = 4
expected3 = 3

input4 = 8
expected4 = 21

assert sol.fib(input1) == expected1
assert sol.fib(input2) == expected2
assert sol.fib(input3) == expected3
assert sol.fib(input4) == expected4
```

- **Time Complexity**: O(2^n)
- **Space Complexity**: O(n)

**Memoization Solution (Top Down Dynamic Programming)**:

```python
class Solution:

    def __init__(self):
        # initialize memo
        self.cache = {}

    def fib(self, n: int) -> int:
        if n < 2:
            return n

        # check if fib(n) was calculated already and cached
        if n in self.cache:
            return self.cache[n]
        else:
            res = self.fib(n - 1) + self.fib(n - 2)

        # cache fib(n) result
        self.cache[n] = res
   
        return res
    
sol = Solution()

input1 = 2
expected1 = 1

input2 = 3
expected2 = 2

input3 = 4
expected3 = 3

input4 = 8
expected4 = 21

assert sol.fib(input1) == expected1
assert sol.fib(input2) == expected2
assert sol.fib(input3) == expected3
assert sol.fib(input4) == expected4
```

- **Time Complexity**: O(n)
- **Space Complexity**: O(n)

**Iterative Solution (Bottom Up Dynamic Programming)**:

```python
class Solution:
    def fib(self, n: int) -> int:
        a, b = 0, 1
        for i in range(0, n):
            a, b = b, a + b
        return a
    
sol = Solution()

input1 = 2
expected1 = 1

input2 = 3
expected2 = 2

input3 = 4
expected3 = 3

input4 = 8
expected4 = 21

assert sol.fib(input1) == expected1
assert sol.fib(input2) == expected2
assert sol.fib(input3) == expected3
assert sol.fib(input4) == expected4
```

- **Time Complexity**: O(n)
- **Space Complexity**: O(1)

Alternatively, could use an array to store the values like this (same time and space complexity):

```python
class Solution:
    def fib(self, n):
        if n < 2:
            return n

        arr = [0, 1]
        i = 2
        while i <= n:
            tmp = arr[1]
            arr[1] = arr[0] + arr[1]
            arr[0] = tmp
            i += 1
        return arr[1]
    
sol = Solution()

input1 = 2
expected1 = 1

input2 = 3
expected2 = 2

input3 = 4
expected3 = 3

input4 = 8
expected4 = 21

assert sol.fib(input1) == expected1
assert sol.fib(input2) == expected2
assert sol.fib(input3) == expected3
assert sol.fib(input4) == expected4
```

