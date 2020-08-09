# Week 2
-------------------------

## `(ryan-foo.github.io)`
    
### 1. [Equal Sides of An Array](https://www.codewars.com/kata/5679aa472b8f57fb8c000047/train/python)

This one is off CodeWars. This was surprisingly similar to a problem from week 1, product of array except self, hence we can handle it in a similar fashion.

Build two arrays `left` and `right`, where left[i] is the sum of all that is to the left of [i] and right[i] is the sum of all that is to the right of [i].

Then, iterate through both and return the index when the sum of left[i] and right[i] is equal to 0.

The runtime complexity is `O(n)`, where `n` refers to the length of the input array `arr`, and the space complexity is `O(n)` (we have 2 arrays of length `n`).


```
def find_even_index(arr):
        
    left =  [0] * len(arr)
    right = [0] * len(arr)
    
    left[0] = 0
    right[len(arr) - 1] = 0
    
    for i in range(1, len(arr)):
        left[i] = left[i-1] + arr[i-1]
    
    for i in reversed(range(len(arr) - 1)):
        right[i] = right[i+1]  + arr[i+1]
    
    for i in range(len(arr)):
        if left[i] == right[i]:
            return i
    
    return -1
```

### 2. Path Sum III (https://leetcode.com/problems/path-sum-iii/) (Medium)

I tried this one with Alaukik on the first day of moving back to college. 9th August 2020.

The main takeaway points for Path Sum III for me was (1) writing a depth first search first, and then your `traverse` function. Your `traverse` function can then be responsible for the nested layer of DFS. (since we're crawling not just from the first node, but from its children, and its children's children, ad infinitum till it terminates at a leaf).

Another takeaway was initializing your global return variable (here, `numOfPaths`).

I was also reminded of pre-order, post-order and in-order traversals.

```
Pre-order:
Mid,
Left,
Right

In-order:
Left,
Mid,
Right


Post-order:
Left,
Right,
Mid
```

In this case, it didn't matter, but it was a good reminder.

The space complexity is `O(1)` as we are not storing any lists / trees etc. If we have `n` treenodes, the stack size (depth of the recursion) can range from `O(n)` for a single sided tree or `O(logn)` for a balanced tree.

As for time complexity: the 1st DFS traverses all nodes, so that's already `O(n).` The 2nd DFS (that happens when you call `traverse`) can take either `O(n)` at longest, for a single sided tree, or `O(logn)`.

The total time complexity is therefore `O(n)` * `O(n)` (`O(n^2)`), or `O(n logn)`.

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> int:
        self.numOfPaths = 0
        self.dfs(root, target)
        return self.numOfPaths
    
    def dfs(self, node, target):
        if node is None:
            return
        
        self.traverse(node,target)
        self.dfs(node.left, target)
        self.dfs(node.right, target)
        
    def traverse(self, node, target):
        if node is None:
            return
        if node.val == target:
            self.numOfPaths += 1
        
        self.traverse(node.left, target-node.val)
         self.traverse(node.right, target-node.val)
```

With credits to `wonderlives` for the brilliant and clearly explained solution.




