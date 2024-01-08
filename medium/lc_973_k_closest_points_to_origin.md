**973. K Closest Points to Origin**

**Medium**

[View on Leetcode](https://leetcode.com/problems/k-closest-points-to-origin/)

Given an array of `points` where `points[i] = [xi, yi]` represents a point on the **X-Y** plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the **X-Y** plane is the Euclidean distance (i.e., ``√(x1 - x2)^2 + (y1 - y2)^2)`.

You may return the answer in **any order**. The answer is **guaranteed** to be **unique** (except for the order that it is in).

**Example 1**:

>
    Input: points = [[1,3],[-2,2]], k = 1
    Output: [[-2,2]]
    Explanation:
    The distance between (1, 3) and the origin is sqrt(10).
    The distance between (-2, 2) and the origin is sqrt(8).
    Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
    We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].

**Example 2**:

>
    Input: points = [[3,3],[5,-1],[-2,4]], k = 2
    Output: [[3,3],[-2,4]]
    Explanation: The answer [[-2,4],[3,3]] would also be accepted.

**Constraints**:

- `1 <= k <= points.length <= 10^4`
- `-10^4 <= xi, yi <= 10^4`

**Solution**:

We can use a min heap and use the distance as the sort value for the heap. We can also include the points with the distance (as a tuple of (distance, point)).

```python
from collections import heapq
from typing import List
import math

class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        
        result = []
        
        def distance(point):
            # since it's always distance from the origin of [0, 0]
            # we can skip subtracting 0 from x and y and just use
            # sqrt(point[0]^2 + point[1]^2)
            x = math.pow(point[0], 2)
            y = math.pow(point[1], 2)
            return math.sqrt(x + y)
        
        # Min heap using (distance, point)
        dist_and_points = [(distance(p), p) for p in points]
        heapq.heapify(dist_and_points)
        
        for _ in range(k):
            result.append(heapq.heappop(dist_and_points)[1])

        return result
    
# test in REPL
sol = Solution()

input1 = [[1,3],[-2,2]]
k1 = 1
assert sol.kClosest(input1, k1) == [[-2,2]]

input2 = [[3,3],[5,-1],[-2,4]]
k2 = 2
assert sol.kClosest(input2, k2) == [[3,3],[-2,4]]
```

 **Time complexity**: `O(n log k)`

- **Creating dist_and_points list**: `O(n)` time
- **Building the heap**: `O(n)` time using `heapify`
- **K extractions from the heap**: `O(log k)` each, for a total of `O(k log k)`

**Space complexity**: `O(n)`

- **result list**: occupies `O(k)` space
- **dist_and_points list**: can hold up to `n` elements in the worst case
- **Heap**: can hold up to `k` elements, but this is still considered `O(n)`
