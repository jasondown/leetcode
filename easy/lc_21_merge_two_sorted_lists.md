**21. Merge Two Sorted Lists**

**Easy**

[View on Leetcode](https://leetcode.com/problems/merge-two-sorted-lists)

You are given the heads of two sorted linked lists list1 and list2.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

**Example 1**:

>
    Input: list1 = [1,2,4], list2 = [1,3,4]
    Output: [1,1,2,3,4,4]

**Example 2**:

>
    Input: list1 = [], list2 = []
    Output: []

**Example 3**:

>
    Input: list1 = [], list2 = [0]
    Output: [0]

**Constraints**:

- The number of nodes in both lists is in the range `[0, 50]`.
- `-100 <= Node.val <= 100`
- Both `list1` and `list2` are sorted in **non-decreasing** order.

**Solution**:

Create a dummy node to remove an edge case where there is no list. Then use two pointers to compare values from each list. Take the lower value and append it to the new list. Once one of the lists runs out, append the remainder of the other list. Then return the dummy node's next pointer.

```python
# Definition for singly-linked list.
class ListNode(object):
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
        
class Solution(object):
    def mergeTwoLists(self, list1, list2):
        """
        :type list1: Optional[ListNode]
        :type list2: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        if not list1 and not list2:
            return None
        
        dummy = ListNode()
        tail = dummy
        
        while list1 and list2:
            if list1.val < list2.val:
                tail.next = list1
                list1 = list1.next
            else:
                tail.next = list2
                list2 = list2.next
            
            tail = tail.next
            
        if list1:
            tail.next = list1
            
        if list2:
            tail.next = list2
            
        return dummy.next
    
def test_equal(node_a: ListNode, node_e: ListNode) -> False:
    while node_a:
        if node_a.val != node_e.val:
            return False
        node_a = node_a.next
        node_e = node_e.next
    return True

sol = Solution()

input1a = ListNode(1, ListNode(2, ListNode(4)))
input1b = ListNode(1, ListNode(3, ListNode(4)))
output1 = sol.mergeTwoLists(input1a, input1b)
expected1 = ListNode(1, ListNode(1, ListNode(2, ListNode(3, ListNode(4, ListNode(4))))))
print(test_equal(output1, expected1))

input2a, input2b, expected2 = ListNode(), ListNode(), ListNode()
output2 = sol.mergeTwoLists(input2a, input2b)
print(test_equal(output2, expected2))

input3a, input3b, expected3 = ListNode(), ListNode(0), ListNode(0)
output3 = sol.mergeTwoLists(input3a, input3b)
print(test_equal(output3, expected3))
```

- **Time Complexity**: O(n)
- **Space Complexity**: O(1)
