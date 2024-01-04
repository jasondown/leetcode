**704. Binary Search**

**Easy**

[View on Leetcode](https://leetcode.com/problems/binary-search/)

Given an array of integers `nums` which is sorted in ascending order, and an integer target, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1**:

>
    Input: nums = [-1,0,3,5,9,12], target = 9
    Output: 4
    Explanation: 9 exists in nums and its index is 4

**Example 2**:

>
    Input: nums = [-1,0,3,5,9,12], target = 2
    Output: -1
    Explanation: 2 does not exist in nums so return -1

**Constraints**:

- `1 <= nums.length <= 104`
- `-10^4 < nums[i], target < 10^4`
- All the integers in `nums` are unique.
- `nums` is sorted in ascending order.

**Solution**:

```python
class Solution(object):
    def search(self, nums, target):
        
        l = 0
        r = len(nums) - 1
        
        while l <= r:
            m = (l + r) // 2 # get mid-point
            # if int overflow is a concern, use:
            # l + ((r - l) // 2)
            
            # if value is higher than target, search left
            if nums[m] > target:
                r = m - 1
            
            #if value is lower than target, search right
            elif nums[m] < target:
                l = m + 1
            
            # hit our target
            else:
                return m
        
        # never hit our target
        return -1
        
sol = Solution()

input1 = [-1,0,3,5,9,12]
target1 = 9
expected1 = 4
assert sol.search(input1, target1) == expected1

input2 = [-1,0,3,5,9,12]
target2 = 2
expected2 = -1
assert sol.search(input2, target2) == expected2
```

**Time Complexity**: `O(logn)`, because input is split in half each iteration.

**Space Complexity**: `O(1)`, because no auxiliary space is used (aside from a few variables irrespective of the input size).
