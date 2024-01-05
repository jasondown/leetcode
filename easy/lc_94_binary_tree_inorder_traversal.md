**94. Binary Tree Inorder Traversal**

**Easy**

[View on Leetcode](https://leetcode.com/problems/binary-tree-inorder-traversal/)

Given the `root` of a binary tree, *return the inorder traversal of its nodes' values*.

**Example 1**:

>
    Input: root = [1,null,2,3]
    Output: [1,3,2]

**Example 2**:

>
    Input: root = []
    Output: []

**Example 3**:

>
    Input: root = [1]
    Output: [1]

**Constraints**:

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

**Follow up**: Recursive solution is trivial, could you do it iteratively?

*NOTE: Inorder traversal means traverse entire left sub-tree, then process the node, the traverse entire right sub-tree.*

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
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        
        result = []

        def inorder(node):
            if not node:
                return

            # left sub-tree traversal, process node, right sub-tree traversal
            inorder(node.left)
            result.append(node.val)
            inorder(node.right)

        inorder(root)
        return result
    
# test in REPL
sol = Solution()

input1 = TreeNode(1)
input1.right = TreeNode(2)
input1.right.left = TreeNode (3)
expected1 = [1, 3, 2]
result1 = sol.inorderTraversal(input1)
print(result1)
assert result1 == expected1

input2 = None
expected2 = []
result2 = sol.inorderTraversal(input2)
print(result2)
assert result2 == expected2

input3 = TreeNode(1)
expected3 = [1]
result3 = sol.inorderTraversal(input3)
print(result3)
assert result3 == expected3
```

**Time Complexity**:

- `O(n)`, because we must traverse every node.

**Space Complexity**:

- `O(h)`, where `h` is the height of the tree, because of the recursion and call stack.
- `O(n)` in a worst case scenario where the tree is skewed/not balanced.

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
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        
        result = []
        stack = []
        current = root

        while stack or current:
            while current:  # Traverse left subtree and push nodes onto stack
                stack.append(current)
                current = current.left
            
            current = stack.pop()  # Visit the leftmost node
            result.append(current.val)
            current = current.right  # Move to the right subtree

        return result
    
# test in REPL
sol = Solution()

input1 = TreeNode(1)
input1.right = TreeNode(2)
input1.right.left = TreeNode (3)
expected1 = [1, 3, 2]
result1 = sol.inorderTraversal(input1)
print(result1)
assert result1 == expected1

input2 = None
expected2 = []
result2 = sol.inorderTraversal(input2)
print(result2)
assert result2 == expected2

input3 = TreeNode(1)
expected3 = [1]
result3 = sol.inorderTraversal(input3)
print(result3)
assert result3 == expected3
```

*NOTE: Same space and time complexity, except our stack is explicit instead of using the call stack.*
