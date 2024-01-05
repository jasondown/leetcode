**230. Kth Smallest Element in a BST**

**Medium**

[View on Leetcode](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

Given the `root` of a binary search tree, and an integer `k`, return *the `kth` smallest value (1-indexed) of all the values of the nodes in the tree*.

**Example 1**:

>
    Input: root = [3,1,4,null,2], k = 1
    Output: 1

**Example 2**:

>
    Input: root = [5,3,6,2,4,null,null,1], k = 3
    Output: 3

**Constraints**:

- The number of nodes in the tree is `n`.
- `1 <= k <= n <= 10^4`
- `0 <= Node.val <= 10^4`

**Follow up**: If the BST is modified often (i.e., we can do insert and delete operations) and you need to find the kth smallest frequently, how would you optimize?

**Iterative Solution**:

```python
from typing import List, Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        stack = []

        # In-order traversal with early termination
        while stack or root:
            while root:
                stack.append(root)
                root = root.left

            node = stack.pop()
            k -= 1  # Decrement count (k) first
            if k == 0:
                return node.val  # Found kth smallest
            root = node.right
    
# test in REPL
sol = Solution()

input1 = TreeNode(3)
input1.left = TreeNode(1)
input1.right = TreeNode(4)
input1.left.right = TreeNode(2)
k1 = 1
expected1 = 1
result1 = sol.kthSmallest(input1, k1)
print(result1)
assert result1 == expected1

input2 = TreeNode(5)
input2.left = TreeNode(3)
input2.right = TreeNode(6)
input2.left.left = TreeNode(2)
input2.left.right = TreeNode(4)
input2.left.left.left = TreeNode(1)
k2 = 3
expected2 = 3
result2 = sol.kthSmallest(input2, k2)
print(result2)
assert result2 == expected2
```

**Time Complexity**: `O(H + k)`, where `H` is the height of the tree. 
In the worst case, the algorithm visits all nodes on one side of the tree (`H` nodes). Then, it iterates up to k nodes before finding the target.

**Space Complexity**: `O(H)`, where `H` is the height of the tree. 
The space used is primarily for the stack to store nodes during in-order traversal. In the worst-case (skewed tree), the stack can store up to `H` nodes.

**Recursive Solution**:

```python
from typing import List, Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        result = None  # Stores the result

        def inorder(node):
            nonlocal k, result # allow access to outer k and result
            if node:
                inorder(node.left)

                # Decrement k and check
                k -= 1
                if k == 0:
                    result = node.val
                    return  # Early termination

                inorder(node.right)

        inorder(root)
        return result
    
# test in REPL
sol = Solution()

input1 = TreeNode(3)
input1.left = TreeNode(1)
input1.right = TreeNode(4)
input1.left.right = TreeNode(2)
k1 = 1
expected1 = 1
result1 = sol.kthSmallest(input1, k1)
print(result1)
assert result1 == expected1

input2 = TreeNode(5)
input2.left = TreeNode(3)
input2.right = TreeNode(6)
input2.left.left = TreeNode(2)
input2.left.right = TreeNode(4)
input2.left.left.left = TreeNode(1)
k2 = 3
expected2 = 3
result2 = sol.kthSmallest(input2, k2)
print(result2)
assert result2 == expected2
```

*NOTE: Same time and space complexity as iterative solution.*