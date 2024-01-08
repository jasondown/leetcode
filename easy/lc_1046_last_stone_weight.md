**1046. Last Stone Weight**

**Easy**

[View on Leetcode](https://leetcode.com/problems/last-stone-weight/)

You are given an array of integers `stones` where `stones[i]` is the weight of the `ith` stone.

We are playing a game with the stones. On each turn, we choose the **heaviest two stones** and smash them together. Suppose the heaviest two stones have weights `x` and `y` with `x <= y`. The result of this smash is:

- If `x == y`, both stones are destroyed, and
- If `x != y`, the stone of weight `x` is destroyed, and the stone of weight `y` has new weight `y - x`.

At the end of the game, there is at most **one stone** left.

Return *the weight of the last remaining stone*. If there are no stones left, return `0`.

**Example 1**:

>
    Input: stones = [2,7,4,1,8,1]
    Output: 1
    Explanation: 
    We combine 7 and 8 to get 1 so the array converts to [2,4,1,1,1] then,
    we combine 2 and 4 to get 2 so the array converts to [2,1,1,1] then,
    we combine 2 and 1 to get 1 so the array converts to [1,1,1] then,
    we combine 1 and 1 to get 0 so the array converts to [1] then that's the value of the last stone.

**Example 2**:

>
    Input: stones = [1]
    Output: 1

**Constraints**:

- `1 <= stones.length <= 30`
- `1 <= stones[i] <= 1000`

**Solution**:

We can use a max heap to efficiently retrieve the two biggest stones. In Python, the min heap can be used, and the values can just be negated.

```python
from collections import heapq

class Solution:
    def lastStoneWeight(self, stones: List[int]) -> int:
        if len(stones) == 1:
            return stones[0]

        # NOTE: Max heap in python is min heap, but negate the value
        max_heap = []

        # Create the max heap
        for s in stones:
            heapq.heappush(max_heap, -s)

        while len(max_heap) > 1:
            y = -heapq.heappop(max_heap)
            x = -heapq.heappop(max_heap)

            if y - x > 0:
                heapq.heappush(max_heap, x - y) # same as -(y - x) to do max heap hack

        # If we have a stone left, return it.
        # Don't forget to reverse the sign
        return -max_heap[0] if max_heap else 0
    
# test in REPL
sol = Solution()

input1 = [2,7,4,1,8,1]
assert sol.lastStoneWeight(input1) == 1

input2 = [1]
assert sol.lastStoneWeight(input2) == 1

input3 = [2,2]
assert sol.lastStoneWeight(input3) == 0
```

**Time Complexity**:

- **Building the heap**: `O(n log n)`
    - Iterating through `stones` to push elements onto the heap takes `O(n)`.
    - Each `heappush` operation takes `O(log n)` time to maintain the heap property.
- **Heap operations within the loop**: `O((n - 1) log n)`
    - The loop runs at most `(n - 1)` times, as each iteration removes two elements.
    - Each `heappop` operation takes `O(log n)` time.
    - `heappush` also takes `O(log n)` time, but it's executed at most `(n - 2)` times, leading to an overall contribution of `O((n - 2) log n)` within the loop.
- **Total time complexity**: `O(n log n)`
    - Combining the above, the dominant term is `O(n log n)`, as it grows faster than `O(n)` for larger input sizes.

**Space Complexity**:

- **Heap storage**: `O(n)`
    - The heap `max_heap` stores up to `n` elements in the worst case.
- **Additional variables**: `O(1)`
    - The other variables used (e.g., x, y) occupy constant space.
- **Total space complexity**: `O(n)`
    - The heap dominates the space usage.

*NOTE: We could convert all the `stones` to negative values and then `heapify` the stones directly. This should be O(1) auxiliary space I think. And `heapify` might slightly be more efficient than manually calling `heappush` for each element.*
