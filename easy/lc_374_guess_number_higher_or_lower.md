**374. Guess Number Higher or Lower**

**Easy**

[View on Leetcode](https://leetcode.com/problems/guess-number-higher-or-lower/description/)

We are playing the Guess Game. The game is as follows:

I pick a number from `1` to `n`. You have to guess which number I picked.

Every time you guess wrong, I will tell you whether the number I picked is higher or lower than your guess.

You call a pre-defined API `int guess(int num)`, which returns three possible results:

- `-1`: Your guess is higher than the number I picked (i.e. `num > pick`).
- `1`: Your guess is lower than the number I picked (i.e. `num < pick`).
- `0`: your guess is equal to the number I picked (i.e. `num == pick`).

Return the number that I picked.

*Example 1*:

>
    Input: n = 10, pick = 6
    Output: 6

**Example 2**:

>
    Input: n = 1, pick = 1
    Output: 1

**Example 3**:

>
    Input: n = 2, pick = 1
    Output: 1

**Constraints**:

- `1 <= n <= 2^31 - 1`
- `1 <= pick <= n`

**Solution**:

A simple binary search can be used.

```python
# The guess API is already defined for you.
# @param num, your guess
# @return -1 if num is higher than the picked number
#          1 if num is lower than the picked number
#          otherwise return 0
def guess(num):
    # I have implemented this just for quick testing
    
    picked = 95 # change to something else
    if num > picked:
        return -1
    elif num < picked:
        return 1
    else:
        return 0

# This is the actual solution code.
class Solution(object):
    def guessNumber(self, n):
        
        # Use binary search
        low = 1
        high = n
        
        while low <= high:
            myGuess = low + ((high - low) // 2) # get mid and avoid overflow (not an issue in Python though)
            result = guess(myGuess)
            
            #debug
            print(f'My Guess: {myGuess}')

            if result == 0:
                return myGuess
            elif result == -1:
                high = myGuess - 1
            else:
                low = myGuess + 1
            
sol = Solution()

print(sol.guessNumber(100)) # testing between 1-100

```

**Time Complexity**: `O(logn)`, because the possible values are cut in half each iteration.

**Space Complexity**: `O(1)`, because there is no auxilary space used that depends on the size of the input.
