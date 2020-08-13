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