**200. Number of Islands**

**Medium**

[View on Leetcode](https://leetcode.com/problems/number-of-islands/)

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return *the number of islands*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1**:

>
    Input: grid = [
        ["1","1","1","1","0"],
        ["1","1","0","1","0"],
        ["1","1","0","0","0"],
        ["0","0","0","0","0"]
    ]
    Output: 1

**Example 2**:

>
    Input: grid = [
        ["1","1","0","0","0"],
        ["1","1","0","0","0"],
        ["0","0","1","0","0"],
        ["0","0","0","1","1"]
    ]
    Output: 3

**Constraints**:

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

**Solution**:

We can use either breadth-first search or depth-first search to calculate all the adjacent `'1'`s to track islands.

*DFS Approach*:

```python
from typing import List

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        # Protect against bad input
        if not grid: return 0
        
        # get rows and cols for easier use later
        rows, cols = len(grid), len(grid[0])
        visited = set()
        islands = 0
        
        # left, right, up, down deltas
        directions = [[0, -1], [0, 1], [-1, 0], [1, 0]]
        
        def dfs(r, c):
            
            # conditions for not exploring this cell
            if (
                # out of bounds
                r not in range(rows) or
                c not in range(cols) or
                # water
                grid[r][c] == '0' or
                # already visited
                (r, c) in visited
            ): return
            
            visited.add((r, c))
            
            for row_delta, col_delta in directions:
                # visit the adjacent cells
                dfs(r + row_delta, c + col_delta)
        
        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == '1' and (r, c) not in visited:
                    # We are exploring a new island
                    islands += 1
                    
                    # Now get all parts of the island
                    dfs(r, c)
                    
        return islands
    
# test in REPL
sol = Solution()

input1 = [
    ["1","1","1","1","0"],
    ["1","1","0","1","0"],
    ["1","1","0","0","0"],
    ["0","0","0","0","0"]
]
assert sol.numIslands(input1) == 1

input2 = [
    ["1","1","0","0","0"],
    ["1","1","0","0","0"],
    ["0","0","1","0","0"],
    ["0","0","0","1","1"]
]
assert sol.numIslands(input2) == 3
```

*BFS Approach*:

```python
from collections import deque
from typing import List

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        # Protect against bad input
        if not grid: return 0
        
        # get rows and cols for easier use later
        rows, cols = len(grid), len(grid[0])
        visited = set()
        islands = 0
        
        # left, right, up, down deltas
        directions = [[0, -1], [0, 1], [-1, 0], [1, 0]]
        
        def bfs(r, c):
            q = deque()
            visited.add((r, c))
            q.append((r, c))
            
            while q:
                row, col = q.popleft()
                
                for row_delta, col_delta in directions:
                    r, c = row + row_delta, col + col_delta
                    # conditions required to actually explore the cell
                    if (
                        # in bounds
                        r in range(rows) and
                        c in range(cols) and
                        # island
                        grid[r][c] == '1' and
                        # not already visited
                        (r, c) not in visited
                    ):
                        q.append((r, c))
                        visited.add((r, c))
        
        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == '1' and (r, c) not in visited:
                    # Now get all parts of the island
                    bfs(r, c)
                    islands += 1
                    
        return islands
    
# test in REPL
sol = Solution()

input1 = [
    ["1","1","1","1","0"],
    ["1","1","0","1","0"],
    ["1","1","0","0","0"],
    ["0","0","0","0","0"]
]
assert sol.numIslands(input1) == 1

input2 = [
    ["1","1","0","0","0"],
    ["1","1","0","0","0"],
    ["0","0","1","0","0"],
    ["0","0","0","1","1"]
]
assert sol.numIslands(input2) == 3
```

**Time Complexity**:

- `O(n*m)`, where `n` and `m` represent the number of rows and columns.

**Space Complexity**:

- `O(n*m)`, where `n` and `m` represent the number of rows and columns.

*NOTE: We could change the grid in place instead, to save memory, but the overall space complexity would still be considered linear.*
