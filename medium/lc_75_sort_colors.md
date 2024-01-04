**75. Sort Colors**

**Medium**

[View on Leetcode](https://leetcode.com/problems/sort-colors/description/)

Given an array `nums` with `n` objects colored red, white, or blue, sort them **in-place** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.


**Example 1**:

>
    Input: nums = [2,0,2,1,1,0]
    Output: [0,0,1,1,2,2]

**Example 2**:

>
    Input: nums = [2,0,1]
    Output: [0,1,2]

**Constraints**:

- `n == nums.length`
- `1 <= n <= 300`
- `nums[i]` is either `0`, `1`, or `2`.

**Two Pointer Solution**:

```python
class Solution:

    def sortColors(self, nums: List[int]) -> None:
        # one pass partition solution using two pointers 
        # (well three if you include the swap pointer)

        lp, rp, sp = 0, len(nums) - 1, 0 # left, right, swap pointers

        while sp <= rp:
            if nums[sp] == 0:
                nums[sp], nums[lp] = nums[lp], nums[sp]
                lp += 1
                sp += 1
            elif nums[sp] == 2:
                nums[sp], nums[rp] = nums[rp], nums[sp]
                rp -= 1
                # don't change swap pointer, because we may need to
                # swap again if a 0 moved from right to the swap pointer
            else:
                # We have a 1, so just leave it and move swap pointer
                sp += 1
        #debug
        print(nums)
        
sol = Solution()

input1 = [2,0,2,1,1,0]
expected1 = [0,0,1,1,2,2]
sol.sortColors(input1) # does in-place sort
assert input1 == expected1

input2 = [2,0,1]
expected2 = [0,1,2]
sol.sortColors(input2) # does in-place sort
assert input2 == expected2
```

**Explanation**:

Use a similar approach to the partitioning portion of a quick-sort algorithm, to allow a single pass.

- **Pointers**: The use of two pointers, `lp` and `rp`, efficiently partitions the array into three sections: red, white, and blue.
- **Single Pass**: The array is traversed only once, making it efficient in terms of time complexity.
- **Swapping**: Elements are swapped only when necessary, ensuring in-place sorting.
- **White Element Handling**: White elements are left in their correct positions as the pointers move towards each other.

**Space Complexity**: `O(1)`, as no additional data structures are used, apart from a few constant-space variables.

**Time Complexity**: `O(n)`, where `n` is the length of the array. This is due to the single pass through the array, and each element being processed at most twice (once for potentially swapping with a red element, and once for potentially swapping with a blue element).

**Bucket/Count Solution**:

```python
class Solution(object):
    
    def sortColors(self, nums):
        # Two pass solution couting frequencies

        counts = [0] * 3  # Counter for each color (0, 1, 2)
        for num in nums:
            counts[num] += 1

        i = 0
        for color in range(3):  # Place elements back into the array
            for _ in range(counts[color]):
                nums[i] = color
                i += 1
        
        #debug
        print(nums)
        
sol = Solution()

input1 = [2,0,2,1,1,0]
expected1 = [0,0,1,1,2,2]
sol.sortColors(input1) # does in-place sort
assert input1 == expected1

input2 = [2,0,1]
expected2 = [0,1,2]
sol.sortColors(input2) # does in-place sort
assert input2 == expected2
```

**Explanation**:

With a limited set of potential values, we can store the counts/frequency of each value in either a hashmap or array of size `colors`. Then we can just iterate through the origina array and replace the values based on the frequencies.

- **Count Frequencies**:
    - Create a `counts` list or hashmap to store the frequency of each color (0, 1, 2). In python we could also use the `Counter` class.
    - Iterate through `nums` and increment the corresponding count for each color.
- **Place Elements**:

    - Iterate through the colors (0, 1, 2).
    - For each color, iterate `counts[color]` times and place the color back into the original array nums at the current index i.

**Time Complexity**: `O(n)` The algorithm iterates through the array twice (once for counting and once for placement), maintaining linear time complexity.

**Space Complexity**: `O(1)` The additional space used is for the `counts` list, which has a size independent of the input array's length, resulting in constant extra space.