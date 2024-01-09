**706. Design HashMap**

**Easy**

[View on Leetcode](https://leetcode.com/problems/design-hashmap/)

Design a HashMap without using any built-in hash table libraries.

Implement the `MyHashMap` class:

- `MyHashMap()` initializes the object with an empty map.
- `void put(int key, int value)` inserts a (key, value) pair into the HashMap. If the key already exists in the map, update the corresponding value.
- `int get(int key)` returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key.
- `void remove(key)` removes the key and its corresponding value if the map contains the mapping for the key.

**Example 1**:

>
    Input
    ["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"]
    [[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]]
    Output
    [null, null, null, 1, -1, null, 1, null, -1]

    Explanation
    MyHashMap myHashMap = new MyHashMap();
    myHashMap.put(1, 1); // The map is now [[1,1]]
    myHashMap.put(2, 2); // The map is now [[1,1], [2,2]]
    myHashMap.get(1);    // return 1, The map is now [[1,1], [2,2]]
    myHashMap.get(3);    // return -1 (i.e., not found), The map is now [[1,1], [2,2]]
    myHashMap.put(2, 1); // The map is now [[1,1], [2,1]] (i.e., update the existing value)
    myHashMap.get(2);    // return 1, The map is now [[1,1], [2,1]]
    myHashMap.remove(2); // remove the mapping for 2, The map is now [[1,1]]
    myHashMap.get(2);    // return -1 (i.e., not found), The map is now [[1,1]]

**Constraints**:

- `0 <= key, value <= 10^6`
- At most `10^4` calls will be made to `put`, `get`, and `remove`.

**Solution**:

We can use a simple hashing function and an array holding linked lists for collisions. Because of the size constraints, we can use the easy path and just allocate the entire `10^4` slots (or in this case I just used `10^3`). We could use a load factor of 0.7 and resize + rehash too. We can also use a dummy node to make the pointer manipulation easier.

```python
class Node:
    
    def __init__(self, key=-1, val=-1, next=None):
        self.key = key
        self.val = val
        self.next = next

class MyHashMap:

    def __init__(self):
        # For this problem we know 10^4 is max inserts
        # To avoid rehashing for now, we'll just make it 10^3
        # Which will have some collisions, but should be performant enough
        self.size = 10 ** 3
        self.set = [Node() for _ in range(self.size)]

    def _hash(self, key):
        # Very simple hash function
        return key % self.size

    def _getNode(self, key):
        return self.set[self._hash(key)]
        
    def put(self, key: int, value: int) -> None:
        curr = self._getNode(key)

        while curr.next: # skip dummy
            # If it already exists, update it
            if curr.next.key == key:
                curr.next.val = value
                return
            curr = curr.next
        # Append the node to the previous node in that bucket
        curr.next = Node(key, value)

    def get(self, key: int) -> int:
        curr = self._getNode(key)
        
        while curr.next: # skip dummy
            # Found it
            if curr.next.key == key:
                return curr.next.val
            else:
                curr = curr.next
        
        # Didn't find it
        return -1

    def remove(self, key: int) -> None:
        curr = self._getNode(key)
        
        while curr.next: # skip dummy
            # Found it, so shift pointer past the one being deleted
            if curr.next.key == key:
                curr.next = curr.next.next
                return
            curr = curr.next

# test in REPL

myHashMap = MyHashMap()
myHashMap.put(1, 1)  # The map is now [[1,1]]
myHashMap.put(2, 2)  # The map is now [[1,1], [2,2]]
myHashMap.get(1)     # return 1, The map is now [[1,1], [2,2]]
myHashMap.get(3)     # return -1 (i.e., not found), The map is now [[1,1], [2,2]]
myHashMap.put(2, 1)  # The map is now [[1,1], [2,1]] (i.e., update the existing value)
myHashMap.get(2)     # return 1, The map is now [[1,1], [2,1]]
myHashMap.remove(2)  # remove the mapping for 2, The map is now [[1,1]]
myHashMap.get(2)     # return -1 (i.e., not found), The map is now [[1,1]]
```

**Time Complexity**:

- **Average Case**: `put(key, value)`, `remove(key)`, `get(key)` are all `O(1)`, assuming a good hash function and well-distributed keys.

- **Worst Case**: `put(key, value)`, `remove(key)`, `get(key)` can be `O(n)`, if all keys hash to the same bucket, leading to long linked lists.

**Space Complexity**:

- `O(n)`, where `n` is the maximum number of elements allowed in the hash set. This is due to the fixed-size array used to store the buckets.
