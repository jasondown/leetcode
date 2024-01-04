**875. Koko Eating Bananas**

**Medium**

[View on Leetcode](https://leetcode.com/problems/koko-eating-bananas/)

Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return the minimum integer `k` such that she can eat all the bananas within `h` hours.

**Example 1**:

>
    Input: piles = [3,6,7,11], h = 8
    Output: 4

**Example 2**:

>
    Input: piles = [30,11,23,4,20], h = 5
    Output: 30

**Example 3**:

>
    Input: piles = [30,11,23,4,20], h = 6
    Output: 23

**Constraints**:

- `1 <= piles.length <= 10^4`
- `piles.length <= h <= 10^9`
- `1 <= piles[i] <= 10^9`

**Solution**:

```python
class Solution(object):

    def minEatingSpeed(self, piles, h):
        def canEatAll(k):
            return sum((pile + k - 1) // k for pile in piles) <= h

        left, right = 1, max(piles)
        while left <= right:
            k = (left + right) // 2
            if canEatAll(k):
                right = k - 1
            else:
                left = k + 1
        return left

sol = Solution()

piles1 = [3,6,7,11]
hours1 = 8
expected1 = 4
assert sol.minEatingSpeed(piles1, hours1) == expected1

piles2 = [30,11,23,4,20]
hours2 = 5
expected2 = 30
assert sol.minEatingSpeed(piles2, hours2) == expected2

piles3 = [30,11,23,4,20]
hours3 = 6
expected3 = 23
assert sol.minEatingSpeed(piles3, hours3) == expected3
```

**Explanation**:

- **Binary Search**: The problem has a clear search space for `k` (possible eating speeds) ranging from `1` to the maximum number of bananas in a pile. Binary search is well-suited for efficiently finding the minimum `k` that meets the condition.

- **`canEatAll` Function**: This function checks if Koko can eat all bananas within `h` hours at a given eating speed `k`. It calculates the total time needed using ceiling division ((pile + K - 1) // K), which ensures Koko eats a whole pile even if it has fewer bananas than K.

- **Searching for `k`**: The main algorithm performs binary search to find the minimum `k`. It iteratively narrows down the search space based on whether `canEatAll` returns `True` or `False` for a given k.

**Time Complexity**:

- `canEatAll` takes `O(n)` time due to the loop over piles.
- Binary search takes `O(log(max(piles)))` iterations.
- Overall complexity: `O(n * log(max(piles)))`.

**Space Complexity**:

- `O(1)` for a constant amount of extra space.

**More Explicit Solution**:

Here is slightly more explicit version that includes some intermediate varaibles for easier code understanding:

```python
import math

def minEatingSpeed(self, piles, h):
    # Define a helper function to check if you can eat all piles within 'h' hours with a given speed 'k'.
    def canEatAll(k):
        # Calculate the total hours required to eat all piles with speed 'k'.
        # For each pile, calculate how many hours it takes to eat it completely by dividing its size by 'k' and rounding up.
        total_hours = sum(math.ceil(pile / k) for pile in piles)
        return total_hours <= h

    # Initialize the search space and the variable to store the minimum speed found so far.
    left, right = 1, max(piles)
    min_speed = float('inf')  # Initialize with infinity as a placeholder.

    # Use binary search to find the minimum speed 'k' to eat all piles within 'h' hours.
    while left <= right:
        # Calculate the middle speed.
        k = (left + right) // 2
        
        # If you can eat all piles with speed 'k', update the minimum speed and search in the left half.
        if canEatAll(k):
            min_speed = min(min_speed, k)  # Update the minimum speed.
            right = k - 1
        # If you can't eat all piles with speed 'k', search in the right half.
        else:
            left = k + 1

    # Return the minimum speed found.
    return min_speed
```
