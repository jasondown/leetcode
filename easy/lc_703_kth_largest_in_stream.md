**703. Kth Largest Element in a Stream**

**Easy**

[View on Leetcode](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

Design a class to find the `kth` largest element in a stream. Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Implement `KthLargest` class:

- `KthLargest(int k, int[] nums)` Initializes the object with the integer `k` and the stream of integers `nums`.
- `int add(int val)` Appends the integer `val` to the stream and returns the element representing the `kth` largest element in the stream.

**Example 1**:

>
    Input
    ["KthLargest", "add", "add", "add", "add", "add"]
    [[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]
    Output
    [null, 4, 5, 5, 8, 8]

    Explanation
    KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);
    kthLargest.add(3);   // return 4
    kthLargest.add(5);   // return 5
    kthLargest.add(10);  // return 5
    kthLargest.add(9);   // return 8
    kthLargest.add(4);   // return 8

**Constraints**:

- `1 <= k <= 10^4`
- `0 <= nums.length <= 10^4`
- `-10^4 <= nums[i] <= 10^4`
- `-10^4 <= val <= 10^4`
- At most `10^4` calls will be made to `add`.
- It is guaranteed that there will be at least `k` elements in the array when you search for the `kth` element.

**Solution**:

We can use a min heap (priority queue) of size `k` to always have the most `kth` largest element at the root of the heap.

```python
from typing import List
import heapq

class KthLargest:

    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.min_heap = []
        
        # Could also do self.min_heap = nums
        # heapq.heapify(self.min_heap), then pop while len > k
        for x in nums:
            self.add(x)

    def add(self, val: int) -> int:
        heapq.heappush(self.min_heap, val)
        if len(self.min_heap) > self.k:
            heapq.heappop(self.min_heap)
        return self.min_heap[0]

# Test in REPL
kthLargest = KthLargest(3, [4, 5, 8, 2])
assert kthLargest.add(3) == 4
assert kthLargest.add(5) == 5
assert kthLargest.add(10) ==  5
assert kthLargest.add(9) ==  8
assert kthLargest.add(4) ==  8
```

**Time Complexity**:

- `O(logk n)` for initializing the heap.
- `O(logk)` for adding (push and pop each take `logk`)

**Space Complexity**:

- `O(k)` to store `k` values on the heap.
