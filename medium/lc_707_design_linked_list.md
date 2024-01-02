**707. Design Linked List**

**Medium**

[View on Leetcode](https://leetcode.com/problems/design-linked-list/)

Design your implementation of the linked list. You can choose to use a singly or doubly linked list.
A node in a singly linked list should have two attributes: `val` and `next`. `val` is the value of the current node, and `next` is a pointer/reference to the next node.
If you want to use the doubly linked list, you will need one more attribute `prev` to indicate the previous node in the linked list. Assume all nodes in the linked list are 0-indexed.

Implement the `MyLinkedList` class:

- `MyLinkedList()` Initializes the `MyLinkedList` object.
- `int get(int index)` Get the value of the `indexth` node in the linked list. If the index is invalid, return `-1`.
- `void addAtHead(int val)` Add a node of value `val` before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
- `void addAtTail(int val)` Append a node of value `val` as the last element of the linked list.
- `void addAtIndex(int index, int val)` Add a node of value `val` before the `indexth` node in the linked list. If `index` equals the length of the linked list, the node will be appended to the end of the linked list. If `index` is greater than the length, the node **will not be inserted**.
- `void deleteAtIndex(int index)` Delete the `indexth` node in the linked list, if the index is valid.
 

**Example 1**:

>
    Input
    ["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"]
    [[], [1], [3], [1, 2], [1], [1], [1]]
    Output
    [null, null, null, null, 2, null, 3]

    Explanation
    MyLinkedList myLinkedList = new MyLinkedList();
    myLinkedList.addAtHead(1);
    myLinkedList.addAtTail(3);
    myLinkedList.addAtIndex(1, 2);    // linked list becomes 1->2->3
    myLinkedList.get(1);              // return 2
    myLinkedList.deleteAtIndex(1);    // now the linked list is 1->3
    myLinkedList.get(1);              // return 3

**Constraints**:

- `0 <= index, val <= 1000`
- Please do not use the built-in LinkedList library.
- At most `2000` calls will be made to `get`, `addAtHead`, `addAtTail`, `addAtIndex` and `deleteAtIndex`.

**Solution**:

```python
class ListNode:
    
    def __init__(self, val):
        self.val = val
        self.prev =None
        self.next = None

class MyLinkedList(object):

    def __init__(self):
        # dummy edge nodes to make edge cases easier
        self.left = ListNode(0)
        self.right = ListNode(0)
        self.left.next = self.right
        self.right.prev = self.left

    def _adjustNodes(self, new_node, prev_node, next_node):
        prev_node.next = new_node
        new_node.prev = prev_node
        new_node.next = next_node
        next_node.prev = new_node

    def get(self, index):
        """
        :type index: int
        :rtype: int
        """
        curr = self.left.next # start at our real first node
        while curr and index > 0:
            curr = curr.next
            index -= 1
        
        if (curr and # didn't go out of bound
            curr != self.right and # not at our dummy pointer
            index == 0 # we didn't break out of our loop early
        ):
            return curr.val
        
        return -1

    def addAtHead(self, val):
        """
        :type val: int
        :rtype: None
        """
        self._adjustNodes(ListNode(val), self.left, self.left.next)
        
    def addAtTail(self, val):
        """
        :type val: int
        :rtype: None
        """
        self._adjustNodes(ListNode(val), self.right.prev, self.right)
        
    def addAtIndex(self, index, val):
        """
        :type index: int
        :type val: int
        :rtype: None
        """
        curr = self.left.next
        while curr and index > 0:
            curr = curr.next
            index -= 1
        
        if curr and index == 0:
            self._adjustNodes(ListNode(val), curr.prev, curr)

    def deleteAtIndex(self, index):
        """
        :type index: int
        :rtype: None
        """
        curr = self.left.next
        while curr and index > 0:
            curr = curr.next
            index -= 1
            
        if (curr and # didn't go out of bound
            curr != self.right and # not at our dummy pointer
            index == 0 # we didn't break out of our loop early
        ):
            
            prev, next = curr.prev, curr.next
            prev.next = next
            next.prev = prev

# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)

myLinkedList = MyLinkedList()
myLinkedList.addAtHead(1)
myLinkedList.addAtTail(3)
myLinkedList.addAtIndex(1, 2);    # linked list becomes 1->2->3
myLinkedList.get(1);              # return 2
myLinkedList.deleteAtIndex(1);    # now the linked list is 1->3
myLinkedList.get(1);              # return 3
```
