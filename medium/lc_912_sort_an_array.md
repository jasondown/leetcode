**912. Sort an Array**

** Medium**

[View on Leetcode](https://leetcode.com/problems/sort-an-array/)

Given an array of integers `nums`, sort the array in ascending order and return it.

You must solve the problem **without using any built-in** functions in `O(nlog(n))` time complexity and with the smallest space complexity possible.

**Example 1**:

>
    Input: nums = [5,2,3,1]
    Output: [1,2,3,5]
    Explanation: After sorting the array, the positions of some numbers are not changed (for example, 2 and 3), while the positions of other numbers are changed (for example, 1 and 5).

**Example 2**:

>
    Input: nums = [5,1,1,2,0,0]
    Output: [0,0,1,1,2,5]
    Explanation: Note that the values of nums are not necessairly unique.

**Constraints**:

- `1 <= nums.length <= 5 * 104`
- `-5 * 104 <= nums[i] <= 5 * 104`

**Merge Sort Solution**:

```python
class Solution(object):
    def sortArray(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        
        def merge(arr, left, mid, right):
            leftHalf = arr[left : mid + 1]
            rightHalf = arr[mid + 1: right + 1]
            
            lp, rp, ip = 0, 0, left
            
            while lp < len(leftHalf) and rp < len(rightHalf):
                if leftHalf[lp] <= rightHalf[rp]:
                    arr[ip] = leftHalf[lp]
                    lp += 1
                else:
                    arr[ip] = rightHalf[rp]
                    rp += 1
                ip += 1
                
            # One of the arrays will still have values
            while lp < len(leftHalf):
                arr[ip] = leftHalf[lp]
                lp += 1
                ip += 1
                
            while rp < len(rightHalf):
                arr[ip] = rightHalf[rp]
                rp += 1
                ip += 1
        

        def mergeSort(arr, l, r):
            
            if l == r: # array is of size 1
                return arr
            
            m = (l + r) // 2 # get mid point
            
            mergeSort(arr, l, m)        # left half
            mergeSort(arr, m + 1, r)    # right half
            merge(arr, l, m, r)
            
            print(arr) #debug
            return arr

        return mergeSort(nums, 0, len(nums) - 1)
    
sol = Solution()

input1 = [5,2,3,1]
expected1 = [1,2,3,5]

input2 = [5,1,1,2,0,0]
expected2 = [0,0,1,1,2,5]

input3 = [2]
expected3 = [2]

assert sol.sortArray(input1) == expected1
assert sol.sortArray(input2) == expected2
assert sol.sortArray(input3) == expected3
```

- **Time Complexity**: O(n logn)
- **Space Complexity**: O(n)