**206. Reverse Linked List**

**Easy**

[View on Leetcode](https://leetcode.com/problems/reverse-linked-list/)

Given the head of a singly linked list, reverse the list, and return the reversed list.

**Example 1**:

>
    Input: head = [1,2,3,4,5]
    Output: [5,4,3,2,1]

**Example 2**:

>
    Input: head = [1,2]
    Output: [2,1]

**Example 3**:

>
    Input: head = []
    Output: []

**Constraints**:

- The number of nodes in the list is the range `[0, 5000]`.
- `-5000 <= Node.val <= 5000`

**Follow up**: A linked list can be reversed either iteratively or recursively. Could you implement both?

**Iterative Solution**:

Two pointers, `prev`, initially set to `null` and `curr`, initially set to `head`. Save the value of `curr.next` in a temp variable, `next`. Then change `curr.next` to `prev`, `prev` to `curr`, `curr` to `next`. Once done iterating (`curr` is `null`), return `prev`, which will be the new head.

```python
# Definition for singly-linked list.
class ListNode(object):
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        prev = None
        curr = head
                
        while curr:
            next = curr.next
            curr.next = prev
            prev = curr
            curr = next
            
        return prev

sol = Solution()

input1 = ListNode(1, ListNode(2, ListNode(3, ListNode(4, ListNode(5)))))
expected1 = ListNode(5, ListNode(4, ListNode(3, ListNode(2, ListNode(1)))))

input2 = ListNode(1, ListNode(2))
expected2 = ListNode(2, ListNode(1))

input3 = ListNode()
expected3 = ListNode()

assert sol.reverseList(input1) == expected1
assert sol.reverseList(input2) == expected2
assert sol.reverseList(input3) == expected3
```

- **Time Complexity**: O(n)
- **Space Complexity**: O(1)

**Recursive Solution**:

```python
# Definition for singly-linked list.
class ListNode(object):
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if not head: # this is ONLY for if the original test input is empty, doesn't apply in all other cases
            return None

        oldTail = head # keep assigning to curr as we recurse down, then the last one will be the one we actually pass all the way back up
        if head.next:
            oldTail = self.reverseList(head.next) # stays the same as we recurse back up
            head.next.next = head # this is what actually points next back to curr as we recurse back up
        head.next = None # point to None in case we've recursed back up to the old head (new tail now)
        
        return oldTail # keep passing all the way up to eventually be the new head


sol = Solution()

input1 = ListNode(1, ListNode(2, ListNode(3, ListNode(4, ListNode(5)))))
expected1 = ListNode(5, ListNode(4, ListNode(3, ListNode(2, ListNode(1)))))

input2 = ListNode(1, ListNode(2))
expected2 = ListNode(2, ListNode(1))

input3 = ListNode()
expected3 = ListNode()

assert sol.reverseList(input1) == expected1
assert sol.reverseList(input2) == expected2
assert sol.reverseList(input3) == expected3
```

- **Time Complexity**: O(n)
- **Space Complexity**: O(n)