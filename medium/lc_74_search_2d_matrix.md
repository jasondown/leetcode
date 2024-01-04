**74. Search a 2D Matrix**

**Medium**

[View on Leetcode](https://leetcode.com/problems/search-a-2d-matrix/)

You are given an `m x n` integer matrix `matrix` with the following two properties:

- Each row is sorted in non-decreasing order.
- The first integer of each row is greater than the last integer of the previous row.

Given an integer `target`, return *`true` if `target` is in `matrix` or `false` otherwise*.

You must write a solution in `O(log(m * n))` time complexity.

**Example 1**:

>
    Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
    Output: true

**Example 2**:

>
    Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
    Output: false

**Constraints**:

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 100`
- `-10^4 <= matrix[i][j], target <= 10^4`

**Solution**:

```python
class Solution(object):
    def searchMatrix(self, matrix, target):

        rows = len(matrix)
        cols = len(matrix[0])

        # Start search from the top-right corner
        row, col = 0, cols - 1

        while row < rows and col >= 0:
            if matrix[row][col] == target:
                return True
            elif matrix[row][col] > target:
                col -= 1  # Move left in the current row
            else:
                row += 1  # Move down to the next row

        return False  # Target not found

sol = Solution()

input1 = [[1,3,5,7],[10,11,16,20],[23,30,34,60]]
target1 = 3
expected1 = True
assert sol.searchMatrix(input1, target1) == expected1

input2 = [[1,3,5,7],[10,11,16,20],[23,30,34,60]]
target2 = 13
expected2 = False
assert sol.searchMatrix(input2, target2) == expected2
```

**Explanation**:

1. **Start from Top-Right**: The algorithm begins at the top-right corner of the matrix. This is efficient because:
    - If the current element is greater than the target, we know all elements to the right in the current row are also greater (since the row is sorted). So, we can eliminate that column.
    - If the current element is less than the target, we know all elements above in the current column are also less (since the first element in each row is greater than the last of the previous row). So, we can eliminate that row.
2. **Binary Search-like Elimination**: In each iteration:
    - If the current element is equal to the target, we have found it.
    - If the current element is greater than the target, we move left (reduce `col`), effectively eliminating the entire column.
    - If the current element is less than the target, we move down (increase `row`), effectively eliminating the entire row.

**Time Complexity**:

- `O(log(m * n))`: In the worst case, we might traverse a diagonal path from the top-right to the bottom-left (or vice-versa), reducing either the number of rows or columns by approximately half in each step. This is akin to binary search on a flattened view of the matrix, hence the logarithmic time complexity.

**Space Complexity**:

- `O(1)`: Both solutions use only a constant amount of extra space for the variables `row`, `col`, and the function return value.

**Alternative Solution**:

We can divide the search into a row binary search, followed by a column binary search. The resulting time and space complexity would be the same.

```python
class Solution(object):
    def searchMatrix(self, matrix, target):

        # Initialize variables for dimensions
        ROWS, COLS = len(matrix), len(matrix[0])

        # First Phase: Binary search on first column to find potential row
        top, bot = 0, ROWS - 1
        while top <= bot:
            row = (top + bot) // 2
            if target > matrix[row][-1]:  # Target might be in a lower row
                top = row + 1
            elif target < matrix[row][0]:  # Target might be in an upper row
                bot = row - 1
            else:  # Target is in this row (if it exists)
                break

        # Check if target was not found in first phase 
        if not (top <= bot):
            return False

        # Second Phase: Binary search on the selected row
        row = (top + bot) // 2  # Use the potentially correct row
        l, r = 0, COLS - 1
        while l <= r:
            m = (l + r) // 2
            if target > matrix[row][m]:
                l = m + 1  # Target might be on the right side
            elif target < matrix[row][m]:
                r = m - 1  # Target might be on the left side
            else:
                return True # Target found
        return False # Target not found

sol = Solution()

input1 = [[1,3,5,7],[10,11,16,20],[23,30,34,60]]
target1 = 3
expected1 = True
assert sol.searchMatrix(input1, target1) == expected1

input2 = [[1,3,5,7],[10,11,16,20],[23,30,34,60]]
target2 = 13
expected2 = False
assert sol.searchMatrix(input2, target2) == expected2
```

*NOTE: Technically this is `O(log(n) + log(m))`, but that is ~ `O(log(m*n))`.*
