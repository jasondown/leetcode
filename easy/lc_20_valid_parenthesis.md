**20. Valid Parenthesis**

**Easy**

[View on Leetcode](https://leetcode.com/problems/valid-parentheses)

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

**Example 1**:

>
    Input: s = "()"
    Output: true

**Example 2**:

>
    Input: s = "()[]{}"
    Output: true

**Example 3**:

>
    Input: s = "(]"
    Output: false

**Constraints**:

- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.

**Solution**

Can store closing to to opening parenthesis pairs as a hashmap. Can use a stack to store the inputs. If we have a closing paranthesis, check if the stack is the matching opening one. If not, return False. If true, pop the stack. Else, if we have an opening parenthesis, append it to the stack.

At the end, if our stack is empty, return true.

```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        map = {
            ']': '[',
            ')': '(',
            '}': '{'
        }
        stack = []
        
        for c in s:
            if c in map:
                if stack[-1] == map[c]:
                    stack.pop()
                else:
                    return False
            else:
                stack.append(c)
        
        print(stack) #debug
        return not stack # not stack = it is empty

sol = Solution()

input1 = "()"
expected1 = True

input2 = "()[]{}"
expected2 = True

input3 = "(["
expected3 = 0

assert sol.isValid(input1) == expected1
assert sol.isValid(input2) == expected2
assert sol.isValid(input3) == expected3
```

- **Time Complexity**: O(n)
- **Space Complexity**: O(n)
