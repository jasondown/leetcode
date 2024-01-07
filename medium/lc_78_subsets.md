**78. Subsets**

**Medium**

[View on Leetcode](https://leetcode.com/problems/subsets/)

Given an integer array `nums` of **unique** elements, return *all possible 
subsets (the power set)*.

The solution set** must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1**:

>
    Input: nums = [1,2,3]
    Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

**Example 2**:

>
    Input: nums = [0]
    Output: [[],[0]]

**Constraints**:

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- All the numbers of `nums` are **unique**.

**Iterative Solution**:

```python
class Solution:
    def subsets(self, nums):

        result = [[]]  # Start with the empty subset

        for num in nums:
            # Generate new subsets by adding the current element to existing subsets
            new_subsets = [subset + [num] for subset in result]
            print(new_subsets)
            result.extend(new_subsets)

        return result

# test in REPL
sol = Solution()

input1 = [1,2,3]
exptected1 = [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
output1 = sol.subsets(input1)
assert exptected1 == output1

input2 = [0]
exptected2 = [[],[0]]
output2 = sol.subsets(input2)
assert exptected2 == output2
```

**Time Complexity**:

- `O(2^N)` (exponential), where `N` is the length of the input array nums. 
- In each iteration of the loop, the number of subsets doubles, resulting in `2^N` possible subsets in total.

**Space Complexity**:

- `O(2^N)` (exponential) due to storing the resulting subsets.
- There are `2^N` possible subsets, and each subset can have at most N elements.

***Explanation from Example 1***:

**Iterations**:

- **Iteration 1 (`num=1`)**:

    - `new_subsets: [subset + [1] for subset in result]` becomes `[[] + [1]]`, which is `[[1]]`.
    - `result.extend(new_subsets)`: The result list becomes `[[], [1]]`.

- **Iteration 2 (`num=2`)**:

    - `new_subsets: [[], [1]] + [2]` becomes `[[2], [1, 2]]`.
    - `result.extend(new_subsets)`: The result list becomes `[[], [1], [2], [1, 2]]`.

- **Iteration 3 (`num=3`)**:

    - `new_subsets: [[], [1], [2], [1, 2]] + [3]` becomes `[[3], [1, 3], [2, 3], [1, 2, 3]]`.
    - `result.extend(new_subsets)`: The result list becomes `[[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]`.

**Appending with `extend`**:

- `result.extend(new_subsets)` is a crucial step in each iteration.
- `extend` does more than just appending; it essentially adds each element of `new_subsets` individually to `result`. Think of it as unwrapping `new_subsets` and adding each of its inner lists to `result` (flattening the list).
- This means that we are not adding the `new_subsets` list as a whole to `result`; instead, we are adding its elements separately, resulting in a combined list of subsets.

**Final Result**:

- After all iterations, the `result` list contains all the possible subsets:


`[[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]`

**Depth-First Search (DFS/Backtracking) Solution**

Use a decision tree to include or not include the next element.

```python
class Solution:
    def subsets(self, nums):

        result = []

        def dfs(index, current_subset):
            # Base case: Add the current subset when we reach the end
            if index == len(nums):
                result.append(current_subset.copy())  # Add a copy to avoid modifying later
                return

            # Decision 1: Include the current element
            current_subset.append(nums[index])
            dfs(index + 1, current_subset)

            # Decision 2: Exclude the current element
            current_subset.pop()  # Backtrack 
            dfs(index + 1, current_subset)

        dfs(0, [])  # Start the DFS from index 0 with an empty subset
        return result

# test in REPL
sol = Solution()

input1 = [1,2,3]
exptected1 = [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3], [2], [3], []]
output1 = sol.subsets(input1)
assert exptected1 == output1

input2 = [0]
exptected2 = [[0],[]]
output2 = sol.subsets(input2)
assert exptected2 == output2
```

**Time Complexity**:

- `O(2^N)`, where `N` is the length of nums. Each element has two choices (include or exclude), leading to `2^N` possible paths.

**Space Complexity**:

- `O(2^N)` in the worst case to store the subsets in result. 
- `O(N)` for the recursive call stack depth (if we don't consider the output space).
