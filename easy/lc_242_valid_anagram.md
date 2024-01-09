**242. Valid Anagram**

**Easy**

[View on Leetcode](https://leetcode.com/problems/valid-anagram/)

Given two strings `s` and `t`, return *`true` if `t` is an anagram of `s`, and `false` otherwise*.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1**:

>
    Input: s = "anagram", t = "nagaram"
    Output: true

**Example 2**:

>
    Input: s = "rat", t = "car"
    Output: false

**Constraints**:

- `1 <= s.length, t.length <= 5 * 10^4`
- `s` and `t` consist of lowercase English letters.

**Follow up**: What if the inputs contain Unicode characters? How would you adapt your solution to such a case?

**Solution**:

There are many approaches that are valid:

1. Sorting both strings and comparing the results.
2. Storing both strings' characters in a hashmap (key=letter, value=count) and comparing the counts for each letter.
3. Using the Python Counter on both strings and comparing them (essentially the same as #2 with the Counter class handling the logic for you.)
4. Create a fixed array for all possible characters and increment the counts for each letter from the first string and decrement the counts for the second string. If the total == 0, you have an anagram.
5. Use a single hashmap for the first word with key=letter and value=count. Then for the second word, decrement the counts. Ensure the values are all 0 at the end.

All except the sorting solution have a **time complexity** of `O(n)` (sorting is `O(n logn)`). Most have a **space complexity** of `O(n)`, although a fixed array for character counting could be considered `O(1)` since it will not change with input size. Sorting may also be `O(1)` if the sorting algorithm is done in place.

*Here are examples of 1 and 3:*

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return false

        return sorted(s) == sorted(t)
```

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return false

        return Counter(s) == Counter(t)
```
