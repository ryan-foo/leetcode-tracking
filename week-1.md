# Week 1
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

### 2. [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

Again, similar approach; generate a dictionary as we want the `O(1)` lookup property. We store the number as a key in the dictionary if its never been seen before, but if its been seen before,
we immediately return true (yes, there is a duplicate.)

An alternative approach may be to sort the list and naturally duplicates would be next to each other. However, a sort is `O(n logn)` in the worst case, and the approach described above with a dictionary is `O(n)`,
as the cost of creating the dictionary is `O(n)`, and the worst case runtime of looking up in a dictionary `n` times would be `O(n)`. Hence overall it is `O(n)`.

Memory complexity is `O(n)` in the worst case, where `n` is the length of the list.

```
from collections import defaultdict

class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        # Assume list of ints
        d = defaultdict(int)
        for num in nums:
            if num in d:
                return True
            else:
                d[num] += 1
        return False
```

### 3. [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

A brute force strategy would be `O(n)^2`, where `n` is the length of the list of prices. 

A better strategy, which runs in `O(n)` time, would involve maintaining the highest difference between buy and sell so far. As we loop through the list of prices, we check if the current price is greater than the minimum so far. If so, we take that difference (which is the profit), and update our highest price.

Order is important here: afterwards, if the `min_so_far` is greater than the price, then we can update `min_so_far`. This ensures that in the next iteration of the loop, the `highest_difference` will be updated using the new `min_so_far`.

This ensures that its not possible to have a situation where we update the `highest_difference` with a faulty `min_so_far` (i.e, a buy happening after a sell).

It also has a space complexity of `O(1)`, or more specifically, `O(2)` as we only maintain two variables (`min_so_far` and `highest_difference`).

```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) < 2:
            return 0
        
        min_so_far = prices[0]
        highest_difference = min(0, prices[1] - prices[0])
        
        for price in prices:
            highest_difference = max(price - min_so_far, highest_difference)
            if min_so_far > price:
                min_so_far = price

        return highest_difference
```

### 4. [Valid Anagram](https://leetcode.com/problems/valid-anagram/)

One way to do this might be to sort, and see if the sorted output is equal. This would have a time complexity of `O(n logn)` in the worst case, where `n` is the length of an input, (`s`). We assume that `s` and `t` have the same length, otherwise they would not be anagrams of each other.

A more efficient alternative would be to keep track of the number of letters, and then remove the letters accordingly.

We can then check if the count of all letters are zero, which means every letter in `s` corresponds to some letter in `t` uniquely. 

We can accomplish this by creating a dictionary from `s` where the keys are letters and the values are the count of the letters in `s`. This action is `O(n)`. Then, we can 

This has an overall time complexity of `O(n)`, as creation of a dictionary from a list has a time complexity of `O(n)`. Additionally, iterating through `t` is also O(n), and membership check is `O(1)` for each membership check in the loop, leading to an overall `O(n)` run time for the second for loop.

Finally, we check if after removing all letters that are part of `t` from the dictionary, the count of all letters is zero. If so, then the words are anagrams of each other!

```
Instead of initializing a dictionary with the keys being the letters and the values being the count,
A defaultdict abstracts this for us and provides us a helpful interface to count what we want to count.
A hashmap of letters.

from collections import defaultdict

class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        
        # Assume input only contains lowercase alphabets, but also works for unicode characters
        
        d = defaultdict(int)
        
        for char in s:
            d[char] += 1
            
        for char in t:
            if char in d:
                d[char] -= 1
                if d[char] < 0:
                    return False
            else:
                return False
        
        # get all values and ensure that it is equal to 0
        return len([x for x in d.values() if x != 0]) == 0
```

### 5. [Valid Parantheses](https://leetcode.com/problems/valid-parentheses)

So we could have used a counter based approach if the problem were a reduced version of itself (i.e, count the number of opening brackets as we move through the string, and subtract whenever we had a closing bracket. But this would not fulfil the second condition, which is for opening brackets to be closed in the correct order.

But this is not the problem we are facing. I did have the intuition on this to use a stack data structure, however I borrowed the optimized mapping of closing brackets to opening brackets from Leetcode's Solution as it was far more elegant, although less explicit than a character matching:  for example, checking `(if char == '{' and stack.pop() == '}')`.

The solution is detailed below: Whenever we encounter an open bracket, push it onto the stack.
Whenever we encounter a close bracket, we pop and check if the open bracket that has been popped is matching the current closing bracket we're looking at. If it does, continue.

If it doesn't, or the stack is empty, return false.

At the end of it all, the stack should be empty. If it is not empty, then we have too many opening brackets for the amount of closing brackets we were given, and therefore it is not well formed.

This problem: a throwback to Aquinas' Intro to CS class.

```
class Solution:
    def isValid(self, s: str) -> bool:
        # We can use a stack data structure
        # Whenever we encounter an open bracket, push it onto stack
        # Whenever we encounter a close bracket, pop
        # If, when we pop, the open bracket popped is NOT matching the close bracket, then return false (Example 4)
        
        # We can use a list for our stack
        stack = []
        
        mapping = {')': '(', '}': '{', ']': '['}
        
        for char in s:
            # It is a closing bracket
            if char in mapping:
                
                # Pop top most element from stack
                top_element = stack.pop() if stack else None
                
                if mapping[char] != top_element:
                    return False
            else:
                #We have an opening bracket
                stack.append(char)
                
        # If stack is empty...
        return not stack 
```

### 6. [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

Of course, the brute force solution would be to iterate through till i and collect the product, skip i, then collect the product of i till len(nums), and multiply those two numbers together, before inserting into the output array.

However, that's O(n)^2 in the worst case as we do n-1 multiplications, n times (for the length of the list).

The key insight came from the LeetCode Solutions.

We can create arrays that contain products of left and the right of the index for each given index i.
One array for left, right.

For each index i, we make use of the product of all the numbers to the left of it (which is contained in l at index i), and multiply by the product of all the numbers to the right (which is contained in r at index i).

This is O(n), without using division, and with `O(n)` space complexity for maintaining two arrays of length `n`, where `n` is the length of the input array.

```
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:

        
        l = [0] * len(nums)
        r = [0] * len(nums)
        answer = [0] * len(nums)
        
        l[0] = 1
        
        for i in range(1, len(nums)):
            l[i] = l[i-1] * nums[i-1]
        
        r[len(nums) - 1] = 1
        
        for i in reversed(range(len(nums) - 1)):
            r[i] = r[i+1] * nums[i+1]
        
        for i in range(len(nums)):
            answer[i] = l[i] * r[i]
            
        return answer
``` 


### 7. [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

The brute force solution is to enumerate all possible sub-arrays and store their sums, before taking the max of those sums. This would not be tenable and definitely would not be the optimal solution.

The problem statement itself suggests trying a divide-and-conquer strategy after having achieved an O(n) solution.