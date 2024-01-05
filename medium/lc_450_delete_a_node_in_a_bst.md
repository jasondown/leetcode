**450. Delete a Node in a BST**

**Medium**

[View on Leetcode](https://leetcode.com/problems/delete-node-in-a-bst/)

Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return *the **root node reference** (possibly updated) of the BST*.

Basically, the deletion can be divided into two stages:

1. Search for a node to remove.
2. If the node is found, delete the node.

**Example 1**:

>
    Input: root = [5,3,6,2,4,null,7], key = 3
    Output: [5,4,6,2,null,null,7]
    Explanation: Given key to delete is 3. So we find the node with value 3 and delete it.
    One valid answer is [5,4,6,2,null,null,7], shown in the above BST.
    Please notice that another valid answer is [5,2,6,null,4,null,7] and it's also accepted.

**Example 2**:

>
    Input: root = [5,3,6,2,4,null,7], key = 0
    Output: [5,3,6,2,4,null,7]
    Explanation: The tree does not contain a node with value = 0.

**Example 3**:

>
    Input: root = [], key = 0
    Output: []

**Constraints**:

- The number of nodes in the tree is in the range `[0, 10^4]`.
- `-10^5 <= Node.val <= 10^5`
- Each node has a **unique** value.
- **root** is a valid binary search tree.
- `-10^5 <= key <= 10^5`

**Follow up**: Could you solve it with time complexity `O(height of tree)`?

**Solution**:

**Algorithm for Deleting a Node in a BST**:

1. **Locate the Node**:

- Start at the root of the tree.
- If the node to be deleted is not found, the tree remains unchanged (base case for recursion).
- If the key to be deleted is less than the root's key, recursively search the left subtree.
- If the key to be deleted is greater than the root's key, recursively search the right subtree.

2. **Node Deletion - Three Cases**:

- **Case 1**: Node has no children (Leaf Node):

    -Simply remove the node from the tree by setting the parent node's pointer (that points to this node) to null.

- **Case 2**: Node has one child:

    - Connect the parent of the node to be deleted directly with the child of that node. 

- **Case 3**: Node has two children:

    - Find either the inorder successor (the smallest node in the right subtree) or the inorder predecessor (the largest node in the left subtree).
    - Replace the value of the node to be deleted with the value from the inorder successor (or predecessor).
    - Recursively delete the inorder successor (or predecessor), which is guaranteed to fall into Case 1 or Case 2 (since it will have at most one child).

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        
        def findMin(curr):
            while curr.left:
                curr = curr.left
            return curr

        def delete(curr, val):
            if not curr:
                return curr

            if val > curr.val:
                curr.right = delete(curr.right, val)
            elif val < curr.val:
                curr.left = delete(curr.left, val)
            else:
                # Node to delete found
                # Cases 1 and 2
                if not curr.left:
                    return curr.right
                elif not curr.right:
                    return curr.left
                else:
                    # Case 3: Two children
                    min_node = findMin(curr.right)  # Find the minimum in the right subtree
                    curr.val = min_node.val       # Replace the value
                    curr.right = delete(curr.right, min_node.val)  # Recursively delete the duplicate
            return curr

        # Crucial: Return the updated root
        return delete(root, key) 
```

*NOTE: See lc_701 solution for testing and printing the BST.*

**Time Complexity**:

- **Finding the node**: `O(h)`, where `h` is the height of the tree. This involves searching for the node to be deleted, potentially traversing a path from the root to a leaf.
- **Deletion**:
    - Cases 1 and 2 (no child or one child) are `O(1)`, as they only involve updating pointers.
    - **Case 3 (two children)**:
        - **Finding the inorder successor**: `O(h)`
        - **Recursively deleting the successor**: `O(h)` since it falls into Case 1 or 2.

Therefore, the overall time complexity of deleting a node from a BST is `O(h)`, which is generally considered `O(log n)` for relatively balanced trees, where `n` is the number of nodes.


**Space Complexity**:

- **Auxiliary Space**: `O(h)` for the recursion stack, as it might need to store information for up to `h` recursive calls in the worst case (a skewed tree).
- **Space for Node Re-arrangement**: `O(1)`, as only a few pointers are modified for node deletion and replacement.
