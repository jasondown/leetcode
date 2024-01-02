**23. Merge K Sorted Lists**

**Hard**

[View on Leetcode](https://leetcode.com/problems/merge-k-sorted-lists)

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

*Merge all the linked-lists into one sorted linked-list and return it.*

**Example 1**:

>
    Input: lists = [[1,4,5],[1,3,4],[2,6]]
    Output: [1,1,2,3,4,4,5,6]
    Explanation: The linked-lists are:
    [
    1->4->5,
    1->3->4,
    2->6
    ]
    merging them into one sorted list:
    1->1->2->3->4->4->5->6

**Example 2**:

>
    Input: lists = []
    Output: []
    Example 3:

    Input: lists = [[]]
    Output: []

**Constraints**:

- `k == lists.length`
- `0 <= k <= 10^4`
- `0 <= lists[i].length <= 500`
- `-10^4 <= lists[i][j] <= 10^4`
- `lists[i]` is sorted in ascending order.
- The sum of `lists[i].length` will not exceed `10^4`.

**Solution**:

```python
# Definition for singly-linked list.
class ListNode(object):
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        def merge(list1, list2):
            dummy = ListNode()
            tail = dummy

            while list1 and list2:
                if list1.val <= list2.val:
                    tail.next = list1
                    list1 = list1.next
                else:
                    tail.next = list2
                    list2 = list2.next
                tail = tail.next
            
            # fill in remainder of whatever list has any left
            if list1:
                tail.next = list1
            elif list2:
                tail.next = list2

            return dummy.next

        if not lists or len(lists) == 0:
            return None 
        
        while len(lists) > 1:
            merged = []

            for i in range(0, len(lists), 2): # pairs of lists, so jump by 2s
                l1 = lists[i]
                l2 = lists[i + 1] if (i + 1 < len(lists)) else None # merge will handle a None list
                merged.append(merge(l1, l2))
            lists = merged
        return lists[0]

def test_equal(node_a: ListNode, node_e: ListNode) -> False:
    while node_a:
        if node_a.val != node_e.val:
            return False
        node_a = node_a.next
        node_e = node_e.next
    return True

sol = Solution()

input1 = [ListNode(1, ListNode(4, ListNode(5))),
          ListNode(1, ListNode(3, ListNode(4))),
          ListNode(2, ListNode(6))]
expected1 = ListNode(1, ListNode(1, ListNode(2, ListNode(3, 
             ListNode(4, ListNode(4, ListNode(5, ListNode(6))))))))
output1 = sol.mergeKLists(input1)

input2 = []
expected2 = []
output2 = sol.mergeKLists(input2)

print(test_equal(output1, expected1))
print(test_equal(output2, expected2))
```

**Time Complexity**:

- The `merge` function has O(N+M) complexity (linear to the sum of list lengths).
- In `mergeKLists`, the lists are repeatedly divided into pairs and merged. In each "level" of the process, the total work is O(N) (merging all nodes). 
- The number of "levels" is approximately log(k) (since we're roughly halving the number of lists each time).
- **Therefore, the total time complexity is O(N * log(k)), where N is the total number of nodes and k is the number of lists**.

**Space Complexity**:

- The `merge` function uses O(1) extra space for the dummy node and pointers.
- The recursive calls in `mergeKLists` lead to a call stack depth of O(log(k)).
- **Therefore, the space complexity is O(log(k))**, mainly due to the recursion depth.
