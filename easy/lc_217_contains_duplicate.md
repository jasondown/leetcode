**217. Contains Duplicate**

**Easy**

[View on Leetcode](https://leetcode.com/problems/contains-duplicate/)

Given an integer array `nums`, return true if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**Example 1:**

>
    Input: nums = [1,2,3,1]
    Output: true

**Example 2**:

>
    Input: nums = [1,2,3,4]
    Output: false

**Example 3**:

>
    Input: nums = [1,1,1,3,3,4,3,2,4,2]
    Output: true

**Constraints**:

- `1 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`

**Solution**:

Use a hashset and early return as soon as a duplicate if found.

```python
from typing import List

class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        
        dups = set()

        for num in nums:
            if num in dups:
                return True

            dups.add(num)

        return False
    
# test in REPL
sol = Solution()

assert sol.containsDuplicate([1,2,3,1]) == True
assert sol.containsDuplicate([1,2,3,4]) == False
assert sol.containsDuplicate([1,1,1,3,3,4,3,2,4,2]) == True
```

**Time Complexity**:

- `O(n)` because at worst, we may have to scan the entire `nums` collection.

**Space Complexity**:

- `O(n)` because at worst, we may need to store the entire `nums` collection.
