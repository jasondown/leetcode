**1. Two Sum**

**Easy**

[View on Leetcode](https://leetcode.com/problems/two-sum/)

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have ***exactly*** **one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

**Example 1**:

>
    Input: nums = [2,7,11,15], target = 9
    Output: [0,1]
    Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

**Example 2**:

>
    Input: nums = [3,2,4], target = 6
    Output: [1,2]

**Example 3**:

>
    Input: nums = [3,3], target = 6
    Output: [0,1]

**Constraints**:

- `2 <= nums.length <= 10^4`
- `-10^9 <= nums[i] <= 10^9`
- `-10^9 <= target <= 10^9`
- **Only one valid answer exists**.

**Follow-up**: Can you come up with an algorithm that is less than `O(n2)` time complexity?

**Solution**:

We can iterate over the `nums` and store the values in a hashmap, using `key=num, val=array_index`. Rather than storing them all up front, as we iterate, we check if `target - current_num` is in the hashmap. If so, we return the current index and the hashmap value (index of the required num).

```python
from typing import List

class Solution(object):
    def twoSum(self, nums, target):
        
        visited = {}

        for i, n in enumerate(nums):
            new_target = target - n
            if new_target in visited:
                return [visited[new_target], i]
            visited[n] = i

    
# test in REPL
sol = Solution()

input1 = [2,7,11,15]
target1 = 9
expected1 = [0,1]
assert sol.twoSum(input1, target1) == expected1

input2 = [3,2,4]
target2 = 6
expected2 = [1,2]
assert sol.twoSum(input2, target2) == expected2

input3 = [3,3]
target3 = 6
expected3 = [0,1]
assert sol.twoSum(input3, target3) == expected3
```

**Time Complexity**:

- `O(n)`, because we may need to iterate over all values of `nums`.

**Space Complexity**:

- `O(n)`, because we may need to store all values of `nums` (except the last one) in the hashmap.
