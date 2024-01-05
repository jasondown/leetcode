**102. Binary Tree Level Order Traversal**

**Medium**

[View on Leetcode](https://leetcode.com/problems/binary-tree-level-order-traversal/)

Given the `root` of a binary tree, return *the level order traversal of its nodes' values*. (i.e., from left to right, level by level).

**Example 1**:


>
    Input: root = [3,9,20,null,null,15,7]
    Output: [[3],[9,20],[15,7]]

**Example 2**:

>
    Input: root = [1]
    Output: [[1]]

**Example 3**:

>
    Input: root = []
    Output: []

**Constraints**:

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-1000 <= Node.val <= 1000`

**Solution**:

The main idea is to use a queue to iteratively process nodes, doing a breadth-first search (bfs), or level order traversal.

1. **Initialize**: Start with the root node in the queue.
2. **Iterate**: While the queue is not empty:
    - Create a list (currLevel) to store node values for the current level.
        - Process all the nodes in the queue (which are all at the same level):
        - Remove a node from the queue.
        - Add its children (left and right) to the queue for processing in the next level.
    - Append the node's value to the currLevel list.
    - Add the completed currLevel to the result list.

```python
import collections
from typing import List, Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        
        result = []
        queue = collections.deque()
        queue.append(root)

        while queue:
            currLevel = []
            lenLevel = len(queue)

            # Iterate through all nodes at the current level
            for _ in range(lenLevel):
                node = queue.popleft()
                
                if node:
                    # Add node value to current level list
                    currLevel.append(node.val)

                    # Add child to the queue (for next level)
                    queue.append(node.left)
                    queue.append(node.right)
                    
            # Ensure we have nodes on the current level and add them to the result
            if currLevel:
                result.append(currLevel)

        return result
    
# test in REPL
sol = Solution()

input1 = TreeNode(3)
input1.left = TreeNode(9)
input1.right = TreeNode(20)
input1.right.left = TreeNode(15)
input1.right.right = TreeNode(7)
expected1 = [[3],[9,20],[15,7]]
result1 = sol.levelOrder(input1)
assert expected1 == result1

input2 = TreeNode(1)
expected2 = [[1]]
result2 = sol.levelOrder(input2)
assert expected2 == result2

input3 = None
expected3 = []
result3 = sol.levelOrder(input3)
assert expected3 == result3
```

**Time Complexity**: 

- `O(N)`, where `N` is the number of nodes in the tree. This is because the algorithm visits each node exactly once.

**Space Complexity**: 

- `O(N)` in the worst case, where the tree is a completely unbalanced tree (essentially a linked list). This is because the maximum size of the queue can grow up to the number of nodes in the last level. 
- `O(log(N))` in the best case of a perfectly balanced tree, as the space will be dominated by the last level, which has about half of the total nodes.
