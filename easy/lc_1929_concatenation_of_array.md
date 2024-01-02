**1929. Concatenation of Array**

**Easy**

[View on Leetcode](https://leetcode.com/problems/concatenation-of-array)

Given an integer array nums of length n, you want to create an array ans of length 2n where ans[i] == nums[i] and ans[i + n] == nums[i] for 0 <= i < n (0-indexed).

Specifically, ans is the concatenation of two nums arrays.

Return the array ans.

**Example 1**:

```text
Input: nums = [1,2,1]
Output: [1,2,1,1,2,1]
Explanation: The array ans is formed as follows:
- ans = [nums[0],nums[1],nums[2],nums[0],nums[1],nums[2]]
- ans = [1,2,1,1,2,1]
```

**Example 2**:

```text
Input: nums = [1,3,2,1]
Output: [1,3,2,1,1,3,2,1]
Explanation: The array ans is formed as follows:
- ans = [nums[0],nums[1],nums[2],nums[3],nums[0],nums[1],nums[2],nums[3]]
- ans = [1,3,2,1,1,3,2,1]
```

**Constraints**:

- n == nums.length
- 1 <= n <= 1000
- 1 <= nums[i] <= 1000

**Solution**:

In this case, loop twice (can allow for parameter of how many concatenations) and for each loop, append the value to an output array.

```python
class Solution(object):
    def getConcatenation(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        
        numConcats = 2
        ans = []
        
        for _ in range(numConcats):
            for j in range(len(nums)):
                ans.append(nums[j])
            
        print(ans) # debug
        return ans

sol = Solution()

nums1 = [1,2,1]
expected1 = [1,2,1,1,2,1]

nums2 = [1,3,2,1]
expected2 = [1,3,2,1,1,3,2,1]

assert sol.getConcatenation(nums1) == expected1
assert sol.getConcatenation(nums2) == expected2
```

- **Time Complexity**: O(n)
- **Space Complexity**: O(n)
