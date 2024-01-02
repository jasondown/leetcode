**70. Climbing Stairs**

**Easy**

[View on Leetcode](https://leetcode.com/problems/climbing-stairs/)

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

**Example 1**:

>
    Input: n = 2
    Output: 2
    Explanation: There are two ways to climb to the top.
    1. 1 step + 1 step
    2. 2 steps

**Example 2**:

>
    Input: n = 3
    Output: 3
    Explanation: There are three ways to climb to the top.
    1. 1 step + 1 step + 1 step
    2. 1 step + 2 steps
    3. 2 steps + 1 step

**Constraints**:

- `1 <= n <= 45`

**Memoization Solution (Top Down Dynamic Programming)**:

```python
class Solution:

    def __init__(self):
        self.cache = {}

    def climbStairs(self, n: int) -> int:
        if n <= 3:
            return n

        if n in self.cache:
            return self.cache[n]
        else:
            res = self.climbStairs(n - 1) + self.climbStairs(n -2)

        self.cache[n] = res

        return res
    
sol = Solution()

input1 = 2
expected1 = 2

input2 = 3
expected2 = 3

assert sol.climbStairs(input1) == expected1
assert sol.climbStairs(input2) == expected2
```

- **Time Complexity**: O(n)
- **Space Complexity**: O(n)

**Iterative Solution (Bottom Up Dynamic Programming)**:

```python
class Solution:

    def climbStairs(self, n: int) -> int:
        # Starting from top step.
        # Top step and second last step have 1 way to get to the last step
        if n <= 2:
            return n
        
        a, b = 1, 1

        for step in range(n - 1):
            # Sub problem, just need value of previous two steps added together
            a, b = a + b, a

        return a

sol = Solution()

input1 = 2
expected1 = 2

input2 = 3
expected2 = 3

assert sol.climbStairs(input1) == expected1
assert sol.climbStairs(input2) == expected2
```

- **Time Complexity**: O(n)
- **Space Complexity**: O(1)