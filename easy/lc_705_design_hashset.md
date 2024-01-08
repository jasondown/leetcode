**705. Design HashSet**

**Easy**

[View on Leetcode](https://leetcode.com/problems/design-hashset/)

Design a HashSet without using any built-in hash table libraries.

Implement `MyHashSet` class:

- `void add(key)` Inserts the value `key` into the HashSet.
- `bool contains(key)` Returns whether the value `key` exists in the HashSet or not.
- `void remove(key)` Removes the value `key` in the HashSet. If `key` does not exist in the HashSet, do nothing.

**Example 1**:

>
    Input
    ["MyHashSet", "add", "add", "contains", "contains", "add", "contains", "remove", "contains"]
    [[], [1], [2], [1], [3], [2], [2], [2], [2]]
    Output
    [null, null, null, true, false, null, true, null, false]

    Explanation
    MyHashSet myHashSet = new MyHashSet();
    myHashSet.add(1);      // set = [1]
    myHashSet.add(2);      // set = [1, 2]
    myHashSet.contains(1); // return True
    myHashSet.contains(3); // return False, (not found)
    myHashSet.add(2);      // set = [1, 2]
    myHashSet.contains(2); // return True
    myHashSet.remove(2);   // set = [1]
    myHashSet.contains(2); // return False, (already removed)

**Constraints**:

- `0 <= key <= 10^6`
- At most `10^4` calls will be made to `add`, `remove`, and `contains`.

**Solution**:

We can use a simple hashing function and an array holding linked lists for collisions. Because of the size constraints, we can use the easy path and just allocate the entire `10^4` slots. We can also use a dummy node to make the pointer manipulation easier.

```python
class Node:
    
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class MyHashSet:

    def __init__(self):
        # For this problem we know 10K is max inserts
        # To avoid rehashing for now, we'll just make it full size
        self.size = 10 ** 4
        self.set = [Node() for _ in range(self.size)]

    def _hash(self, val):
        # Very simple hash function
        return val % self.size

    def _getNode(self, key):
        return self.set[self._hash(key)]
        
    def add(self, key: int) -> None:
        curr = self._getNode(key)

        while curr.next: # skip dummy
            # Don't add duplicate
            if curr.next.val == key:
                return
            curr = curr.next
        # Append the node to the previous node in that bucket
        curr.next = Node(key)

    def remove(self, key: int) -> None:
        curr = self._getNode(key)
        
        while curr.next: # skip dummy
            # Found it, so shift pointer past the one being deleted
            if curr.next.val == key:
                curr.next = curr.next.next
                return
            curr = curr.next
    
    def contains(self, key: int) -> bool:
        curr = self._getNode(key)
        
        while curr.next: # skip dummy
            # Found it
            if curr.next.val == key:
                return True
            else:
                curr = curr.next
        
        # Didn't find it
        return False

# test in REPL

myHashSet = MyHashSet()
myHashSet.add(1)            # set = [1]
myHashSet.add(2)            # set = [1, 2]
myHashSet.contains(1)       # return True
myHashSet.contains(3)       # return False, (not found)
myHashSet.add(2)            # set = [1, 2]
myHashSet.contains(2)       # return True
myHashSet.remove(2)         # set = [1]
myHashSet.contains(2)       # return False, (already removed)
myHashSet.add(10001)        # Collision with 1; set = [[1->10001]]
myHashSet.contains(1)       # return True
myHashSet.contains(10001)   # return True
myHashSet.remove(1)         # set = [10001]
myHashSet.contains(1)       # return False
myHashSet.contains(10001)   # return True
```

**Time Complexity**:

- **Average Case**: `add(key)`, `remove(key)`, `contains(key)` are all `O(1)`, assuming a good hash function and well-distributed keys.

- **Worst Case**: `add(key)`, `remove(key)`, `contains(key)` can be `O(n)`, if all keys hash to the same bucket, leading to long linked lists.

**Space Complexity**:

- `O(n)`, where `n` is the maximum number of elements allowed in the hash set. This is due to the fixed-size array used to store the buckets.
