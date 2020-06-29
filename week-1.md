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