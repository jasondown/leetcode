**105. Construct Binary Tree from Preorder and Inorder Traversal**

**Medium**

[View on Leetcode](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and inorder is the `inorder` traversal of the same tree, construct and return *the binary tree*.

**Example 1**:

>
    Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
    Output: [3,9,20,null,null,15,7]

**Example 2**:

>
    Input: preorder = [-1], inorder = [-1]
    Output: [-1]

**Constraints**:

- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- `-3000 <= preorder[i], inorder[i] <= 3000`
- `preorder` and `inorder` consist of unique values.
- Each value of `inorder` also appears in `preorder`.
- `preorder` is guaranteed to be the `preorder` traversal of the tree.
- `inorder` is guaranteed to be the `inorder` traversal of the tree.

**Example walkthrough**:

```text
For left recursion:
    preorder = preorder[1: mid + 1]
    inorder = inorder[: mid]
For right recursion:
    preorder = preorder[mid + 1: ]
    inorder = inorder[mid + 1: ]

----------
ROOT (3)

Preorder [3, 9, 20, 15, 7]
Inorder  [9, 3, 15, 20, 7]
root = preorder[0] = 3
mid = indorder.index(preorder[0]) = 1

----------
LEFT of 3 (9)

Preorder [9]
Inorder  [9]
root = preorder[0] = 9
mid = inorder.index(preorder[0]) = 0

----------
LEFT AND RIGHT of 9 (null)
Return to 9, then to 3

----------
RIGHT of 3 (20)

Preorder [20, 15, 7]
Inorder  [15, 20, 7]
root = preorder[0] = 20
mid = inorder.index(preorder[0]) = 1

----------
LEFT of 20 (15)

Preorder [15]
Inorder  [15]
root = preorder[0] = 15
mid = inorder.index(preorder[0]) = 0

----------
LEFT AND RIGHT of 15 (null)
Return to 15, then to 20

----------
RIGHT of 20 (7)

Preorder [7]
Inorder  [7]
root = preorder[0] = 7
mid = inorder.index(preorder[0]) = 0

----------
LEFT AND RIGHT OF 7 (null)
Return to 7, then to 20, then to 3
```

```python
from typing import List, Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder or not inorder:
            return None
        
        # current root is always head of preorder
        root = TreeNode(preorder[0])
        
        # partition inorder at current root
        mid = inorder.index(preorder[0])
        
        # Elements from index 1 to mid in the preorder list are the left subtree
        # Elements before mid in the inorder list are the left subtree
        root.left = self.buildTree(preorder[1:mid+1], inorder[:mid])
        
        # Elements after mid in the preorder list are the right subtree
        # Elements after mid in the inorder list are the right subtree
        root.right = self.buildTree(preorder[mid+1:], inorder[mid+1:])
        
        return root

# helper for printing
def print_tree(root: Optional[TreeNode]):
    if not root:
        return  # Empty tree

    queue = deque([root])  # Use a queue for level-order traversal
    while queue:
        level_nodes = len(queue)  # Nodes in the current level
        for _ in range(level_nodes):
            node = queue.popleft()
            print(node.val, end=" ")  # Print node value
            if node.left:
                queue.append(node.left)  # Add left child to the queue
            if node.right:
                queue.append(node.right)  # Add right child to the queue
        print()  # Move to the next level   
    
# test in REPL
sol = Solution()

preorder1 = [3,9,20,15,7]
inorder1 = [9,3,15,20,7]
expected1 = TreeNode(3)
expected1.left = TreeNode(9)
expected1.right = TreeNode(20)
expected1.right.left = TreeNode(15)
expected1.right.right = TreeNode(7)
result1 = sol.buildTree(preorder1, inorder1)
print_tree(expected1)
print_tree(result1)

preorder2 = [-1]
inorder2 = [-1]
expected2 = TreeNode(-1)
result2 = sol.buildTree(preorder2, inorder2)
print_tree(expected2)
print_tree(result2)
```

**Time Complexity**:

- **Overall**: `O(n^2)`, where `n` is the number of nodes.
- **Breakdown**:
    - Each node is visited once for tree construction.
    - The index method in inorder scan takes `O(n)` in the worst case (skewed tree). This is amortized across calls, but in the worst case it still works out to ~`O(n^2)`. A hashmap could be used to store the indices, reducing the time complexity to `O(n)` at the cost of extra space.

**Space Complexity**:

- **Overall**: `O(h)`, where h is the height of the tree.
- **Breakdown**:
    - The recursion stack for building the tree can be up to the height of the tree.

**Solution using a hashmap**:

```python
from typing import List, Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        # Create hashmap to store inorder element indices for O(1) lookups
        inorder_map = {val: idx for idx, val in enumerate(inorder)}

        def helper(pre_start, pre_end, in_start, in_end):
            if pre_start > pre_end or in_start > in_end:
                return None

            root_val = preorder[pre_start]  # Root is always the first in preorder
            root = TreeNode(root_val)

            # Efficiently find the index in inorder using hashmap
            root_idx = inorder_map[root_val]

            # Num of elements in left subtree (for slicing later)
            num_left = root_idx - in_start

            # Build left and right subtrees recursively
            root.left = helper(pre_start + 1, pre_start + num_left, in_start, root_idx - 1)
            root.right = helper(pre_start + num_left + 1, pre_end, root_idx + 1, in_end)

            return root

        # Call helper with the full ranges of the lists
        return helper(0, len(preorder) - 1, 0, len(inorder) - 1)

# helper for printing
def print_tree(root: Optional[TreeNode]):
    if not root:
        return  # Empty tree

    queue = deque([root])  # Use a queue for level-order traversal
    while queue:
        level_nodes = len(queue)  # Nodes in the current level
        for _ in range(level_nodes):
            node = queue.popleft()
            print(node.val, end=" ")  # Print node value
            if node.left:
                queue.append(node.left)  # Add left child to the queue
            if node.right:
                queue.append(node.right)  # Add right child to the queue
        print()  # Move to the next level   
    
# test in REPL
sol = Solution()

preorder1 = [3,9,20,15,7]
inorder1 = [9,3,15,20,7]
expected1 = TreeNode(3)
expected1.left = TreeNode(9)
expected1.right = TreeNode(20)
expected1.right.left = TreeNode(15)
expected1.right.right = TreeNode(7)
result1 = sol.buildTree(preorder1, inorder1)
print_tree(expected1)
print_tree(result1)

preorder2 = [-1]
inorder2 = [-1]
expected2 = TreeNode(-1)
result2 = sol.buildTree(preorder2, inorder2)
print_tree(expected2)
print_tree(result2)
```

