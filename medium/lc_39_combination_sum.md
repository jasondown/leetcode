**39. Combination Sum**

**Medium**

[View on Leetcode](https://leetcode.com/problems/combination-sum/)

Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all unique combinations of* `candidates` *where the chosen numbers sum to* `target`. You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1**:

    >
    Input: candidates = [2,3,6,7], target = 7
    Output: [[2,2,3],[7]]
    Explanation:
    2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
    7 is a candidate, and 7 = 7.
    These are the only two combinations.

**Example 2**:

>
    Input: candidates = [2,3,5], target = 8
    Output: [[2,2,2,2],[2,3,3],[3,5]]

**Example 3**:

>
    Input: candidates = [2], target = 1
    Output: []

**Constraints**:

- `1 <= candidates.length <= 30`
- `2 <= candidates[i] <= 40`
- All elements of `candidates` are distinct.
- `1 <= target <= 40`

**Solution**:

We can use recursive backtracking via depth-first search and use a decision tree that explores combinations with and then without the current candidate (this allows resusing the same candidate more than once and ensures we do not get duplicate combinations that are just permutations of each other).

```python
from typing import List

class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []

        def dfs(index, sum, curr_subset):
            
            # This combination adds to target
            if sum == target:
                result.append(curr_subset.copy())
                return

            # Total is too high or ran out of candidates
            if sum > target or index >= len(candidates):
                return

            # Continue with this candidate because we can use it more than once
            curr_subset.append(candidates[index])
            dfs(index, sum + candidates[index], curr_subset)

            # Continue without this candidate
            curr_subset.pop()
            dfs(index + 1, sum, curr_subset)

        dfs(0, 0, [])
        return result

# test in REPL
sol = Solution()

input1 = [2,3,6,7]
target1 = 7
expected1 = [[2,2,3],[7]]
output1 = sol.combinationSum(input1, target1)
assert expected1 == output1

input2 = [2,3,5]
target2 = 8
expected2 = [[2,2,2,2],[2,3,3],[3,5]]
output2 = sol.combinationSum(input2, target2)
assert expected2 == output2

input3 = [2]
target3 = 1
expected3 = []
output3 = sol.combinationSum(input3, target3)
assert expected3 == output3
```

**Time Complexity**:

- **Worst Case**: `O(n^t)`, where `n` is the average length of combinations that add up to target `t` and where `t` is the target.

**Space Complexity**:

- **Worst Case**: `O(n * t)`, where `n` is the length of the candidates list and `t` is the target sum.
    - The result list can store up to `n * t` combinations in the worst case.
    - The maximum depth of the recursion stack is bounded by target (as each level adds a number to the sum).
    - **Average Case**: Can be significantly lower depending on the distribution of candidates and target.

*NOTE: A dynamic programming approach could be used to reduce time complexity at the cost of extra space complexity.*
