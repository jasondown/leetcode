**695. Max Area of Island**

**Medium**

[View on Leetcode](https://leetcode.com/problems/max-area-of-island/)

You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected **4-directionally** (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The **area** of an island is the number of cells with a value `1` in the island.

Return *the maximum area of an island in* `grid`. If there is no island, return `0`.

**Example 1**:

>
    Input: grid = [
        [0,0,1,0,0,0,0,1,0,0,0,0,0],
        [0,0,0,0,0,0,0,1,1,1,0,0,0],
        [0,1,1,0,1,0,0,0,0,0,0,0,0],
        [0,1,0,0,1,1,0,0,1,0,1,0,0],
        [0,1,0,0,1,1,0,0,1,1,1,0,0],
        [0,0,0,0,0,0,0,0,0,0,1,0,0],
        [0,0,0,0,0,0,0,1,1,1,0,0,0],
        [0,0,0,0,0,0,0,1,1,0,0,0,0]
    ]
    Output: 6
    Explanation: The answer is not 11, because the island must be connected 4-directionally.

**Example 2**:

>
    Input: grid = [[0,0,0,0,0,0,0,0]]
Output: 0

**Constraints**:

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `grid[i][j]` is either `0` or `1`.

**Solution**:

We can use DFS or BFS as usual for grid problems. I will just do the DFS solution for now.

```python
from collections import deque
from typing import List

class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        # Bad input
        if not grid: return 0
        
        visited = set()
        rows, cols = len(grid), len(grid[0])
        max_area = 0
        
        def dfs(r, c):
            
            # check conditions to skip this cell
            if (
                # out of bounds
                r not in range(rows) or
                c not in range(cols) or
                # water
                grid[r][c] == 0 or
                # already visited
                (r, c) in visited
            ): return 0
            
            visited.add((r, c))
            # add 1 to our area, plus any adjacent cells that are land
            return (
                1 +
                dfs(r + 1, c) +
                dfs(r - 1, c) +
                dfs(r, c + 1) +
                dfs(r, c - 1))
            
        for r in range(rows):
            for c in range(cols):
                # don't even bother until we hit land
                if grid[r][c] == 1:
                    max_area = max(max_area, dfs(r, c))
                
        return max_area
    
# test in REPL
sol = Solution()

input1 = [
    [0,0,1,0,0,0,0,1,0,0,0,0,0],
    [0,0,0,0,0,0,0,1,1,1,0,0,0],
    [0,1,1,0,1,0,0,0,0,0,0,0,0],
    [0,1,0,0,1,1,0,0,1,0,1,0,0],
    [0,1,0,0,1,1,0,0,1,1,1,0,0],
    [0,0,0,0,0,0,0,0,0,0,1,0,0],
    [0,0,0,0,0,0,0,1,1,1,0,0,0],
    [0,0,0,0,0,0,0,1,1,0,0,0,0]
]
assert sol.maxAreaOfIsland(input1) == 6

input2 =[[0,0,0,0,0,0,0,0]]
assert sol.maxAreaOfIsland(input2) == 0
```

**Time Complexity**:

- `O(n*m)`, where `n` and `m` represent the number of rows and columns.

**Space Complexity**:

- `O(n*m)`, where `n` and `m` represent the number of rows and columns.
