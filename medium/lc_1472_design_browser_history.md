**1472. Dsign Browser History**

**Medium**

[View on Leetcode](https://leetcode.com/problems/design-browser-history)

You have a **browser** of one tab where you start on the `homepage` and you can visit another `url`, get back in the history number of `steps` or move forward in the history number of `steps`.

Implement the `BrowserHistory` class:

- `BrowserHistory(string homepage)` Initializes the object with the `homepage` of the browser.
- `void visit(string url)` Visits `url` from the current page. It clears up all the forward history.
- `string back(int steps)` Move `steps` back in history. If you can only return `x` steps in the history and `steps > x`, you will return only `x` steps. Return the current url after moving back in history at most `steps`.
- `string forward(int steps)` Move steps forward in history. If you can only forward `x` steps in the history and `steps > x`, you will forward only `x` steps. Return the current url after forwarding in history at most `steps`.
 

**Example**:

>
    Input:
    ["BrowserHistory","visit","visit","visit","back","back","forward","visit","forward","back","back"]
    [["leetcode.com"],["google.com"],["facebook.com"],["youtube.com"],[1],[1],[1],["linkedin.com"],[2],[2],[7]]
    Output:
    [null,null,null,null,"facebook.com","google.com","facebook.com",null,"linkedin.com","google.com","leetcode.com"]

    Explanation:
    BrowserHistory browserHistory = new BrowserHistory("leetcode.com");
    browserHistory.visit("google.com");       // You are in "leetcode.com". Visit "google.com"
    browserHistory.visit("facebook.com");     // You are in "google.com". Visit "facebook.com"
    browserHistory.visit("youtube.com");      // You are in "facebook.com". Visit "youtube.com"
    browserHistory.back(1);                   // You are in "youtube.com", move back to "facebook.com" return "facebook.com"
    browserHistory.back(1);                   // You are in "facebook.com", move back to "google.com" return "google.com"
    browserHistory.forward(1);                // You are in "google.com", move forward to "facebook.com" return "facebook.com"
    browserHistory.visit("linkedin.com");     // You are in "facebook.com". Visit "linkedin.com"
    browserHistory.forward(2);                // You are in "linkedin.com", you cannot move forward any steps.
    browserHistory.back(2);                   // You are in "linkedin.com", move back two steps to "facebook.com" then to "google.com". return "google.com"
    browserHistory.back(7);                   // You are in "google.com", you can move back only one step to "leetcode.com". return "leetcode.com"

**Constraints**:

- `1 <= homepage.length <= 20`
- `1 <= url.length <= 20`
- `1 <= steps <= 100`
- `homepage` and `url` consist of  '.' or lower case English letters.
- At most `5000` calls will be made to `visit`, `back`, and `forward`.

**Doubly Linked List Solution**:

```python
class ListNode():
    
    def __init__(self, val, prev=None, next=None):
        self.val = val
        self.prev = prev
        self.next = next

class BrowserHistory(object):

    def __init__(self, homepage):
        """
        :type homepage: str
        """
        self.current = ListNode(homepage)
        
    def visit(self, url):
        """
        :type url: str
        :rtype: None
        """
        self.current.next = ListNode(url, self.current)
        self.current = self.current.next

    def back(self, steps):
        """
        :type steps: int
        :rtype: str
        """
        while self.current.prev and steps > 0:
            self.current = self.current.prev
            steps -= 1
        
        return self.current.val

    def forward(self, steps):
        """
        :type steps: int
        :rtype: str
        """
        while self.current.next and steps > 0:
            self.current = self.current.next
            steps -= 1
        
        return self.current.val


# Your BrowserHistory object will be instantiated and called as such:
# obj = BrowserHistory(homepage)
# obj.visit(url)
# param_2 = obj.back(steps)
# param_3 = obj.forward(steps)

browserHistory = BrowserHistory("leetcode.com")
browserHistory.visit("google.com");       # You are in "leetcode.com". Visit "google.com"
browserHistory.visit("facebook.com");     # You are in "google.com". Visit "facebook.com"
browserHistory.visit("youtube.com");      # You are in "facebook.com". Visit "youtube.com"
browserHistory.back(1);                   # You are in "youtube.com", move back to "facebook.com" return "facebook.com"
browserHistory.back(1);                   # You are in "facebook.com", move back to "google.com" return "google.com"
browserHistory.forward(1);                # You are in "google.com", move forward to "facebook.com" return "facebook.com"
browserHistory.visit("linkedin.com");     # You are in "facebook.com". Visit "linkedin.com"
browserHistory.forward(2);                # You are in "linkedin.com", you cannot move forward any steps.
browserHistory.back(2);                   # You are in "linkedin.com", move back two steps to "facebook.com" then to "google.com". return "google.com"
browserHistory.back(7);                   # You are in "google.com", you can move back only one step to "leetcode.com". return "leetcode.com"
```

- **Time Complexity**: O(1) (Visit), O(n) (Forward/Back)
- **Space Complexity**: O(n)

**Dynamic Array Solution**:

```python
class BrowserHistory:

    def __init__(self, homepage: str):
        self.curr = 0
        self.history = [homepage]
        self.size = 1
        
    def visit(self, url: str) -> None:
        # curr is 0 based and len is 1 based
        # so compare curr + 1 for offset diff and + another 1 for next cell
        # if len < curr + 2, we have no space and must resize the array via append
        if len(self.history) < self.curr + 2:
            self.history.append(url)
        else:
            self.history[self.curr + 1] = url
        
        self.curr += 1
        # soft delete by changing size pointer instead of popping all forward cells
        # which would make it worst case O(n) instead of O(1)
        self.size = self.curr + 1 

    def back(self, steps: int) -> str:
        candidate = self.curr - steps
        self.curr = candidate if candidate > 0 else 0
        return self.history[self.curr]

    def forward(self, steps: int) -> str:
        candidate = self.curr + steps
        self.curr = candidate if candidate < self.size else (self.size - 1)
        return self.history[self.curr]


# Your BrowserHistory object will be instantiated and called as such:
# obj = BrowserHistory(homepage)
# obj.visit(url)
# param_2 = obj.back(steps)
# param_3 = obj.forward(steps)

browserHistory = BrowserHistory("leetcode.com")
browserHistory.visit("google.com");       # You are in "leetcode.com". Visit "google.com"
browserHistory.visit("facebook.com");     # You are in "google.com". Visit "facebook.com"
browserHistory.visit("youtube.com");      # You are in "facebook.com". Visit "youtube.com"
browserHistory.back(1);                   # You are in "youtube.com", move back to "facebook.com" return "facebook.com"
browserHistory.back(1);                   # You are in "facebook.com", move back to "google.com" return "google.com"
browserHistory.forward(1);                # You are in "google.com", move forward to "facebook.com" return "facebook.com"
browserHistory.visit("linkedin.com");     # You are in "facebook.com". Visit "linkedin.com"
browserHistory.forward(2);                # You are in "linkedin.com", you cannot move forward any steps.
browserHistory.back(2);                   # You are in "linkedin.com", move back two steps to "facebook.com" then to "google.com". return "google.com"
browserHistory.back(7);                   # You are in "google.com", you can move back only one step to "leetcode.com". return "leetcode.com"
```

- **Time Complexity**: O(1) for all operations
- **Space Complexity**: O(n)
