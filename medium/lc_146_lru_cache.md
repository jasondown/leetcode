**146. LRU Cache**

**Medium**

[View on Leetcode](https://leetcode.com/problems/lru-cache/)

Design a data structure that follows the constraints of a **Least Recently Used (LRU) cache**.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
- `int get(int key)` Return the value of the `key` if the `key` exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, `evict` the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

**Example 1**:

>
    Input
    ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
    [[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
    Output
    [null, null, null, 1, null, -1, null, -1, 3, 4]
    
    Explanation
    LRUCache lRUCache = new LRUCache(2);
    lRUCache.put(1, 1); // cache is {1=1}
    lRUCache.put(2, 2); // cache is {1=1, 2=2}
    lRUCache.get(1);    // return 1
    lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
    lRUCache.get(2);    // returns -1 (not found)
    lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
    lRUCache.get(1);    // return -1 (not found)
    lRUCache.get(3);    // return 3
    lRUCache.get(4);    // return 4

**Constraints**:

- `1 <= capacity <= 3000`
- `0 <= key <= 10^4`
- `0 <= value <= 10^5`
- At most `2 * 10^5` calls will be made to `get` and `put`.

**Solution**:

We can use an ordered dictionary in python to allow `get` and `put` in `O(1)` average time complexity. We can also remove then add an item to have it go to the right (most recently used) when we get or put, or we can use the built-in `move_to_end`, which essentially does the same thing behind the scenes (I believe this is using a hasmap and linked list combination, so moving, removing or inserting is just prev/next pointer adjustments).

*Approach 1*:

This uses the OrderedDict and `move_to_end`.

```python:
from collections import OrderedDict

class LRUCache:

    def __init__(self, capacity: int):
        self.cache = OrderedDict()  # Use OrderedDict for O(1) moves
        self.capacity = capacity

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        else:
            self.cache.move_to_end(key)  # Move to end (most recently used)
            return self.cache[key]

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.cache.move_to_end(key)  # Move to end (most recently used)
        self.cache[key] = value
        if len(self.cache) > self.capacity:
            self.cache.popitem(last=False)  # Remove least recently used (first item)
        
# test in 
lRUCache = LRUCache(2)
lRUCache.put(1, 1)  # cache is {1=1}
lRUCache.put(2, 2)  # cache is {1=1, 2=2}
lRUCache.get(1)     # return 1
lRUCache.put(3, 3)  # LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2)     # returns -1 (not found)
lRUCache.put(4, 4)  # LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1)     # return -1 (not found)
lRUCache.get(3)     # return 3
lRUCache.get(4)     # return 4
lRUCache.put(4, 1)  # cache is {3=3, 4=1}
lRUCache.get(4)     # return 1
```

*Approach 2*:

This using an OrderedDict and manually doing a `pop` and then `add`.

```python
from collections import OrderedDict

class LRUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = OrderedDict()  # Employ OrderedDict for LRU order

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1

        # Move the key to the front, marking it as most recently used
        value = self.cache.pop(key)
        self.cache[key] = value
        return value

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.cache.pop(key)
        else:
            if len(self.cache) >= self.capacity:
                # Evict the least recently used key (last in the ordered dictionary)
                self.cache.popitem(last=False)

        self.cache[key] = value  # Add key-value pair

# Same test cases as above
```

*Solution 3*:

This is manually using a doubly-linked list and hashmap instead of an OrderedDict.

```python
class Node:
    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None

class LRUCache:

    def __init__(self, capacity: int):
        self.cache = {}  # HashMap for O(1) lookup
        self.head = Node(0, 0)  # Dummy head node
        self.tail = Node(0, 0)  # Dummy tail node
        self.head.next = self.tail
        self.tail.prev = self.head
        self.capacity = capacity

    def get(self, key: int) -> int:
        if key in self.cache:
            node = self.cache[key]
            self._remove_node(node)
            self._add_to_tail(node)
            return node.value
        else:
            return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            node = self.cache[key]
            node.value = value
            self._remove_node(node)
            self._add_to_tail(node)
        else:
            node = Node(key, value)
            self._add_to_tail(node)
            self.cache[key] = node
            if len(self.cache) > self.capacity:
                oldest = self.head.next
                self._remove_node(oldest)
                del self.cache[oldest.key]

    def _remove_node(self, node: Node) -> None:
        node.prev.next = node.next
        node.next.prev = node.prev

    def _add_to_tail(self, node: Node) -> None:
        prev_tail = self.tail.prev
        prev_tail.next = node
        node.prev = prev_tail
        node.next = self.tail
        self.tail.prev = node
```

*The time/space complexity is basically the same for all three solutions.*

**Time Complexity**:

- `O(1)` average for both `get` and `put`.

**Space Complexity**:

- `O(c)`, where `c` is the capacity sent to the constructor.
