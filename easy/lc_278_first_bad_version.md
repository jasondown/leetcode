**278. First Bad Version**

**Easy**

[View on Leetcode](https://leetcode.com/problems/first-bad-version/)

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which returns whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

**Example 1**:

>
    Input: n = 5, bad = 4
    Output: 4
    Explanation:
    call isBadVersion(3) -> false
    call isBadVersion(5) -> true
    call isBadVersion(4) -> true
    Then 4 is the first bad version.

**Example 2**:

>
    Input: n = 1, bad = 1
    Output: 1

**Constraints**:

- `1 <= bad <= n <= 2^31 - 1`

**Solution**:

Use binary search to converge on the first bad version.

```python
# The isBadVersion API is already defined for you.
# def isBadVersion(version: int) -> bool:

class Solution:
    def firstBadVersion(self, n: int) -> int:
        
        def getFirstBadVersion(first, last):

            while first < last:
                candidate = first + ((last - first) // 2)
                isBad = isBadVersion(candidate)
            
                if isBad:
                    last = candidate
                else:
                    first = candidate + 1

            return first

        return getFirstBadVersion(1, n)
```

**Time Complexity**:

- `O(log n)` - In each iteration, the search space is reduced by half. In the worst case, the algorithm will make approximately log(n) calls to `isBadVersion`.

**Space Complexity**:

- `O(1)` - The algorithm uses constant extra space to store the first, last, and candidate variables.
