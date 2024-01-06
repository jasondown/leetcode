**199. Binary Tree Right Side View**

**Medium**

[View on Leetcode](https://leetcode.com/problems/binary-tree-right-side-view/description/)

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return (the values of the nodes you can see ordered from top to bottom).

**Example 1**:

>
    Input: root = [1,2,3,null,5,null,4]
    Output: [1,3,4]

**Example 2**:

>
    Input: root = [1,null,3]
    Output: [1,3]

**Example 3**:

>
    Input: root = []
    Output: []

**Constraints**:

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

**Solution**:

We can use a level order traversal (breadth-first search) and track the rightmost node on each level.

```python
from collections import deque
from typing import List, Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []

        result = []
        queue = deque([root])

        while queue:
            level_size = len(queue)

            # Keep track of the rightmost node for the current level
            rightmost_node = None

            for _ in range(level_size):
                node = queue.popleft()
                rightmost_node = node  # Update the rightmost node

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

            # Only append the rightmost node of the level
            if rightmost_node:
                result.append(rightmost_node.val)

        return result

# test in REPL
sol = Solution()

input1 = TreeNode(1)
input1.left = TreeNode(2)
input1.right = TreeNode(3)
input1.left.right = TreeNode(5)
input1.right.right = TreeNode(4)
expected1 = [1,3,4]
result1 = sol.rightSideView(input1)
assert expected1 == result1

input2 = TreeNode(1)
input2.right = TreeNode(3)
expected2 = [1,3]
result2 = sol.rightSideView(input2)
assert expected2 == result2

input3 = None
expected3 = []
result3 = sol.rightSideView(input3)
assert expected3 == result3
```

**Space Complexity**: `O(N)`, where `N` is the number of nodes in the tree. The space used primarily for the queue and result list scales with the number of nodes.

**Time Complexity**: `O(N)`, as each node is visited exactly once during the level-order traversal.