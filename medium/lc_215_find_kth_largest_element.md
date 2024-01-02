**215. Find Kth Largest Element in an Array**

**Medium**

[View on Leetcode](https://leetcode.com/problems/kth-largest-element-in-an-array/)

Given an integer array `nums` and an integer `k`, return the `kth` largest element in the array.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Can you solve it without sorting?

**Example 1**:

>
    Input: nums = [3,2,1,5,6,4], k = 2
    Output: 5

**Example 2**:

>
    Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
    Output: 4

**Constraints**:

- `1 <= k <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`

**Solution**:

```python
import heapq

class Solution(object):
    def findKthLargest(self, nums, k) -> int:
        p = []  # Initialize an empty min-heap

        # Iterate through the input list:
        for x in nums:
            heapq.heappush(p, x)        # Push element onto the heap
            if len(p) > k:              # If heap size exceeds k, 
                heapq.heappop(p)        # Remove the smallest element (at the top)
            #debug
            print(p)
        # Heap now contains the 'k' largest elements
        return p[0]  # Return the smallest element in the heap (kth largest)
    
sol = Solution()

assert sol.findKthLargest([3,2,1,5,6,4], 2) == 5
assert sol.findKthLargest([3,2,3,1,2,4,5,5,6], 4) == 4
```

Iteration | Heap `p`   | Explanation
----------|------------|-----------------
1         | [3]        | Push 3.
2         | [2, 3]     | Push 2.
3         | [2, 3]     | Push 1, pop 1 (heap size > k). 
4         | [3, 5]     | Push 5, pop 2 (heap size > k).
5         | [5, 6]     | Push 6, pop 3 (heap size > k).
6         | [5, 6]  | Push 4, pop 4 (heap size > k).

**Time Complexity**: 

- **Average**: `O(NlogK)`, where N is the number of elements in the input list and K is the value of k.
- **Worst Case**: `O(NlogN)`, if the input list is already sorted in ascending order (rare).

**Space Complexity**: 

- `O(K)`, where K is the value of k. This is because the heap only stores at most k elements at any given time.

