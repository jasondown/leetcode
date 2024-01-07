**112. Path Sum**

**Easy**

[View on Leetcode](https://leetcode.com/problems/path-sum/)

Given the `root` of a binary tree and an integer `targetSum`, return `true` if the tree has a **root-to-leaf** path such that adding up all the values along the path equals `targetSum`.

A **leaf** is a node with no children.

**Example 1**:

>
    Input: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
    Output: true
    Explanation: The root-to-leaf path with the target sum is shown.

Example 2:

>
    Input: root = [1,2,3], targetSum = 5
    Output: false
    Explanation: There two root-to-leaf paths in the tree:
    (1 --> 2): The sum is 3.
    (1 --> 3): The sum is 4.
    There is no root-to-leaf path with sum = 5.

**Example 3**:

>
    Input: root = [], targetSum = 0
    Output: false
    Explanation: Since the tree is empty, there are no root-to-leaf paths.

**Constraints**:

- The number of nodes in the tree is in the range `[0, 5000]`.
- `-1000 <= Node.val <= 1000`
- `-1000 <= targetSum <= 1000`

**Solution**:

Use depth-first search (backtracking) and keep track of the sum. Check when we are at a leaf node if we hit the sum.

```python
from typing import Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        
        def isLeafNode(node):
            return not node.left and not node.right
        
        # Inorder depth-first search
        def dfs(node, currSum):
            
            # Base case: empty node
            if not node:
                return False
            
            currSum += node.val

            # If it's a leaf node, check if we've reached the targetSum
            if isLeafNode(node):
                return currSum == targetSum
            
            return (dfs(node.left, currSum) or dfs(node.right, currSum))
        
        return dfs(root, 0)

# test in REPL
sol = Solution()

input1 = TreeNode(5)
input1.left = TreeNode(4)
input1.right = TreeNode(8)
input1.left.left = TreeNode(11)
input1.right.left = TreeNode(13)
input1.right.right = TreeNode(4)
input1.left.left.left = TreeNode(7)
input1.left.left.right = TreeNode(2)
input1.right.right.right = TreeNode(1)
target1 = 22
expected1 = True
output1 = sol.hasPathSum(input1, target1)
assert expected1 == output1

input2 = TreeNode(1, TreeNode(2), TreeNode(3))
target2 = 5
expected2 = False
output2 = sol.hasPathSum(input2, target2)
assert expected2 == output2

input3 = None
target3 = 0
expected3 = False
output3 = sol.hasPathSum(input3, target3)
assert expected3 == output3
```

**Time Complexity**: `O(N)`, where `N` is the number of nodes.

**Space Complexity**: `O(H)`, where `H` is the height of the tree. The average case would be `H = logn` for a balanced tree, whereas a skewed tree could be `O(N)` (like a linked list).
