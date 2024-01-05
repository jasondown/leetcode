**701. Insert into a Binary Search Tree**

**Medium**

[View on Leetcode](https://leetcode.com/problems/insert-into-a-binary-search-tree/description/)

You are given the `root` node of a binary search tree (BST) and a `value` to insert into the tree. Return *the root node of the BST after the insertion*. It is **guaranteed** that the new value does not exist in the original BST.

**Notice** that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return **any of them**.

**Example 1**:

>
    Input: root = [4,2,7,1,3], val = 5
    Output: [4,2,7,1,3,5]
    Explanation: Another accepted tree is:

**Example 2**:

>
    Input: root = [40,20,60,10,30,50,70], val = 25
    Output: [40,20,60,10,30,50,70,null,null,25]

**Example 3**:

>
    Input: root = [4,2,7,1,3,null,null,null,null,null,null], val = 5
    Output: [4,2,7,1,3,5]

**Constraints**:

- The number of nodes in the tree will be in the range `[0, 10^4]`.
- `-10^8 <= Node.val <= 10^8`
- All the values Node.val are unique.
- `-10^8 <= val <= 10^8`
- It's **guaranteed** that `val` does not exist in the original BST.

**Solution**:

```python
from collections import deque
from typing import Optional

class TreeNode:
    # Definition for a binary tree node already created for you
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:

        if not root:  # Empty tree, create a new node
            return TreeNode(val)

        if root.val > val:
            root.left = self.insertIntoBST(root.left, val)  # Insert in left subtree
        else:
            root.right = self.insertIntoBST(root.right, val)  # Insert in right subtree

        return root  # Return the updated root
            
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
        
# test

root = TreeNode(4)
root.left = TreeNode(2)
root.right = TreeNode(7)
root.left.left = TreeNode(1)
root.left.right = TreeNode(3)

expected = TreeNode(4)
expected.left = TreeNode(2)
expected.right = TreeNode(7)
expected.left.left = TreeNode(1)
expected.left.right = TreeNode(3)
expected.right.left = TreeNode(5)

# Insert a new node
sol = Solution()
root = sol.insertIntoBST(root, 5)
print_tree(root)
print_tree(expected)
assert root.right.left.val == expected.right.left.val == 5
```

**Explanation:**

1. **Base Case:** If the tree is empty, create a new node with the value and return it as the root.
2. **Recursive Traversal:** Recursively traverse the tree:
   - If the value to insert is less than the current node's value, go left.
   - Otherwise, go right.
3. **Insertion:** Insert the value into the appropriate subtree using a recursive call.
4. **Return Updated Root:** Return the updated root node to propagate changes up the tree.

**Time Complexity**:

- `O(log n)` on average for a balanced BST, `O(n)` in the worst case (skewed tree). This is due to the recursive calls potentially traversing a path of logarithmic or linear length.

**Space Complexity**:

- `O(log n)` on average due to the recursive call stack, `O(n)` in the worst case for a linear tree shape.
