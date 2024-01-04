**700. Binary Search Tree**

**Easy**

[View on Leetcode](https://leetcode.com/problems/search-in-a-binary-search-tree/)

You are given the root of a binary search tree (BST) and an integer `val`.

Find the node in the BST that the node's value equals `val` and return the subtree rooted with that node. If such a node does not exist, return `null`.

**Example 1**:

>
    Input: root = [4,2,7,1,3], val = 2
    Output: [2,1,3]

**Example 2**:

>
    Input: root = [4,2,7,1,3], val = 5
    Output: []

**Constraints**:

- The number of nodes in the tree is in the range `[1, 5000]`.
- `1 <= Node.val <= 10^7`
- `root` is a binary search tree.
- `1 <= val <= 10^7`

**Solution**:

*NOTE: Some helper code added for testing in a Python REPL.*

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

# ACTUAL SOLUTION CODE
class Solution:
    def searchBST(self, root, val):
        if not root:
            return None

        if root.val > val:
            return self.searchBST(root.left, val)
        elif root.val < val:
            return self.searchBST(root.right, val)
        else:
            return root
# END OF ACTUAL SOLUTION

class Helper:
    def subtree(self, root):
        # Helper function to return the subtree rooted at a given node.
        if not root:
            return []
        return [root.val] + self.subtree(root.left) + self.subtree(root.right)

# Test
solution = Solution()
helper = Helper()

root1 = TreeNode(4)
root1.left = TreeNode(2)
root1.right = TreeNode(7)
root1.left.left = TreeNode(1)
root1.left.right = TreeNode(3)
val1 = 2
answer = solution.searchBST(root1, val1)
print(f'Original Tree: {helper.subtree(root1)}')
print(f'Subtree containing {val1}: {helper.subtree(answer)}')
assert helper.subtree(answer) == [2, 1, 3]

root2 = TreeNode(4)
root2.left = TreeNode(2)
root2.right = TreeNode(7)
root2.left.left = TreeNode(1)
root2.left.right = TreeNode(3)
val2 = 5
answer2 = solution.searchBST(root2, val2)
print(f'Original Tree: {helper.subtree(root2)}')
print(f'Subtree containing {val2}: {helper.subtree(answer2)}')
assert helper.subtree(answer2) == []
```

**Time Complexity**:

The time complexity of the `searchBST` function is `O(h)`, where `h` is the height of the BST. In the worst-case scenario, the height of a BST can be `O(n)` (where `n` is the number of `nodes`), especially if the tree becomes skewed (i.e., all `nodes` are arranged in a single branch). However, on average, the height of a BST for a randomly built tree would be approximately `O(log n))`.

**Space Complexity**:

The space complexity is determined by the recursive calls made during the traversal of the BST. In the worst-case scenario, the space complexity can be `(O(n))` due to the recursion stack, especially if the tree is skewed.

However, on average, for a balanced BST, the space complexity would be `O(log n)`, considering the depth of the recursion stack is `O(log n)` for a tree with `n` `nodes`.

In summary:

- **Time Complexity**: `O(h)` in the worst case, `O(log n)` on average for a balanced BST.
- **Space Complexity**: `O(n)` in the worst case, `O(log n)` on average for a balanced BST.
