# Week 3
-------------------------

## `(ryan-foo.github.io)`

### 1. [Two Sum](https://leetcode.com/problems/two-sum/)

#### Notes

Nested for loops is `O(n)^2` time complexity, where `n` is the length of the list. Membership check is `O(n)` worst case for arrays.

Hence, we can consider using a dictionary, which is `O(n)` to create but has the property of `O(1)` lookup. 
Your overall time complexity would be `O(n)`, and a memory complexity of O(n) as you store all the numbers inside.

We use `defaultdict` for counting so a key error is not returned.

```
from collections import defaultdict

class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        dic = defaultdict(int)
        for i, num in enumerate(nums):
            if num in dic:
                return [dic[num], i]
            else:
                dic[target - num] = i
```

### 2. [Path Sum III](https://leetcode.com/problems/path-sum-iii/)

### 3. [Longest Substring](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

### 4. [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

If the node has one or more children, then we swap the position of its children, and recursively swap the position of its children's children. Then, we return the node.

If `node.right` and `node.left` are empty, i.e, `node` is a leaf, then we just return `node`, which is the leaf with no children swapped.

```
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        def traverse(node):
            if node:
                node.left, node.right = traverse(node.right), traverse(node.left)
            return node
        return traverse(root)
```

### 5. [Balanced Binary Trees](https://leetcode.com/problems/balanced-binary-tree/)

In traverse, we return two values: True, which states the subtree is balanced, and the height of the current subtree. We recursively call traverse on the left and right subchildren, which then return their values upwards. If they ever return a False, then the main tree is not balanced. (The insight being that a tree is not balanced if its sub-trees are imbalanced.)

We visit every node once, and at every node, we do a calculation `(abs(left[1] - right[1]) > 1)`, which is `O(1)`. Therefore it is `O(n)` where is `n` is the amount of nodes in the tree. The space complexity is `O(1)` as we are not creating any new trees or data structures.

```
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        def traverse(node):
            if not node:
                return (True, 0)
            
            left = traverse(node.left)
            right = traverse(node.right)
            if not right[0] or not left[0]:
                return (False, 0)
            
            if abs(left[1] - right[1]) > 1:
                return (False, 0)
            
            return True, max(left[1], right[1]) + 1
        
        return (traverse(root))[0]
```

Another solution:

```
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        balanced = [True]
        def traverse(node):
            if not node:
                return 0
            
            left = traverse(node.left)
            right = traverse(node.right)
            if abs(left - right) > 1:
                balanced[0] = False
            
            return max(left, right) + 1
        
        traverse(root)
        return balanced[0]
```

### 6. [Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)

This runs in `O(n)` run time, where `n` is the number of nodes in the linked list, as we traverse the linked list only once. It is also `O(1)` space complexity as we do it in place (i.e, we do not instantiate new lists to hold the nodes, nor do we create new linked lists beyond what is required for the solution.) 

The brute force approach would be to traverse the linked list, and alternately append the nodes to new lists (`evens` and `odds`). Afterwards, join up linked lists. This is order of `O(n)` runtime, but is slower than the ideal solution. Additionally, it has `O(n)` space complexity as it uses two new lists.

The final approach (code listed below) is to instantiate a new node to represent the head of those linked lists. Also, have a variable that keeps track of whether you are currently are on an odd or even numbered node.

Then, while traversing the list, alternate the variable and . You have a notion of the final element (tail) of your two linked lists (which begin with `evens_head` and `odds_head`). If we are in the odd case, we take the tail of the `odds_head` linked list and point it to the current node we are on (from our input). Likewise for our evens case. Then we flip the `odd` variable (or `is_odd`), and continue traversing.

At the end, we point the tail of `evens` to None, and the tail of odds to the head of `evens`, which is `even_heads.next`.

Then, return the head of `odds`, which is `odds_head.next`.

```
class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:

        odds, evens = ListNode(0), ListNode(0)
        evens_head = evens
        odds_head = odds
        odd = True
        
        while head:
            if odd:
                odds.next = head
                odds = odds.next
            else:
                evens.next = head
                evens = evens.next
            odd = not odd
            head = head.next
            
        evens.next = None
        odds.next = evens_head.next
        
        return odds_head.next
```

### 7. [Valid Parantheses](https://leetcode.com/problems/valid-parentheses/)

Big classic. `O(n)` runtime where `n` is the length of the string since we traverse it. `O(n)` space complexity because in the worst case we append an entire string before returning an error.

We build a mapping of openers `{[(` to closers `)]}`. We utilize a stack data structure and push if its an opener. If its a closer, we say 'hey mate, are you the closer we're looking for?' We do that by comparing the closer to the opener from the top of the stack.

Coffee is for closers, but only if they're well balanced. Make sure it doesn't overflow.

```
class Solution:
    def isValid(self, s: str) -> bool:
        corresponding_brackets = {'{':'}', '(':')', '[': ']'}
        stack = []
        for char in s:
            if char in corresponding_brackets.keys():
                stack.append(char)
            else:
                if len(stack) == 0 or char != corresponding_brackets[stack.pop()]:
                    return False
        return stack == []
            
        
# Stack to see if its valid
# Dictionary to map { to } and ( to ) and so on
```