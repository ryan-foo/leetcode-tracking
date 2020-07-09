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

One possible solution would be to iterate through the list and keep track of the current sum. As we are iterating, whenever the sum falls below 0, we can just restart from the number we are on (effectively creating a new contiguous subarray). Otherwise, we continue adding the seen numbers to the subarray.

At any point, if the current sum is larger than the maximum sum we've seen so far, we replace `max_sum`. At the end of the for loop, we return `max_sum`.

This solution has `O(n)`, where `n` is the length of the list, time complexity as you iterate through the list once.

This problem is definitely similar to 3. Best Time to Buy and Sell Stock in terms of the intuition required.

It also has `O(1)` space complexity as you only maintain a `cur_sum` and `max_sum` variable.

```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        # Approach: iterate through the subarrays
        # Reset subarray to the current number if adding the previous number brought it to less than 0
        
        cur_sum, max_sum = 0, nums[0]
        
        for n in nums:
            if cur_sum < 0:
                cur_sum = n
                
            else:
                cur_sum += n
            
            if cur_sum > max_sum:
                max_sum = cur_sum
        
        return max_sum
```

### 8. [3 Sum](https://leetcode.com/problems/3sum/)

Idea from Leetcode solutions. ([christopherwu0529](https://leetcode.com/problems/3sum/discuss/232712/Best-Python-Solution-\(Explained\)), who apparently has 1008 stars. Go them!)

We want to find all combinations of `a, b, c` in the array such that `a + b + c = 0`. The idea is to sort the list and iterate through every combination of `a, b, c` by using pointers. We keep `a` fixed and then move pointers for `b` and `c` accordingly till we find a combination where `b + c = -a`.

We have two pointers: `l`, that represents `b` and starts from the left of the sorted list, and `r`, that represents `c` and starts from the right of the sorted list.

We start moving `i`, which represents `a` from `0` to `n-2`, where `n` is the length of the list. In each iteration, we consider `l` from `i+1` onwards, and `r` from the back of the list.

If the result of `nums[i] + nums[l] + nums[r]`, or `a+b+c`, is smaller than 0, the sum is too small and we should increment `l`. Conversely, if the result of `a+b+c` is smaller than 0, then our number is too big and we should decrement `r`.

We also should stop considering `nums[i]` when `a` is positive as there will be no combinations of `b` and `c` past `nums[i]` that will be negative (since we consider `b` and `c` past the positive number `a`, and the list is sorted!)

The runtime is `O(n^2)`. Sorting is worst case `O(nlogn)`. In the main loop (`i`), which runs `n` times, we have a while loop that, in the worst case, traverses the length of the list by moving the pointers up to `n` times. This nested loop leads to an `O(n^2)` runtime.

The space complexity is `O(1)` as we do not initialize any new data structures beyond the desired result array.

Optimization:
 We want to decrement `l` and `r` such that we skip to the next available number, since we don't want to perform a duplicate calculation.

```
class Solution:
    def threeSum(self, nums):
        res = []
        nums.sort()
        for i in range(len(nums)-2):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            if nums[i] > 0:
                break
            l, r = i+1, len(nums)-1
            while l < r:
                s = nums[i] + nums[l] + nums[r]
                if s < 0:
                    l +=1 
                elif s > 0:
                    r -= 1
                else:
                    res.append((nums[i], nums[l], nums[r]))
                    while l < r and nums[l] == nums[l+1]:
                        l += 1
                    while l < r and nums[r] == nums[r-1]:
                        r -= 1
                    l += 1; r -= 1
        return res
```

### 9. [Merge Intervals](https://leetcode.com/problems/merge-intervals)

Insights: You probably want to `append` where you can since its O(1), instead of modifying the input array. This keeps things functional instead of iterative, which makes your program cleaner to understand!

Python specific things to remember: `[-1]` to easily access last element in the array. `if not array` is the same as `if len(array) == 0`, but cool. Iterate with elements instead of indices where you can.

Sort `intervals` by its first digit. Iterate through intervals -- if you have no current interval in the result array, then add it to the result array. If the beginning of the new interval is larger than the previous interval, then append it. Otherwise, if it is equal to or greater than the previous interval, then merge it.

It is `O(nlogn)` runtime as that is the runtime of sorting, which dominates a simple walk of interval. Append is `O(1)` amortized and checks are also `O(1)`.

It is `O(1)` space complexity excluding the result array.

Final:

```
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        
        intervals = sorted(intervals, key = lambda interval: interval[0])
        
        merged = []
        
        for interval in intervals:
            if not merged or merged[-1][1] < interval[0]:
                merged.append(interval)
            
            if merged[-1][1] >= interval[0]:
                merged[-1][1] = max(merged[-1][1], interval[1])
        
        return merged
```

My initial attempt,
which fails on the `[0,4], [3,5]` case...

```
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        
        # Can we assume intervals are sorted? It looks like not, so we should sort them first.
        
        # Sort intervals using first number as a key
        
        intervals = sorted(intervals, key=lambda interval: interval[0])
        
        # Compare 2nd number of interval i [a,b] (b) with 1st number of interval i + 1 [c,d] (c)
        if len(intervals) == 2:
            if intervals[0][1] >= intervals[1][0]:
                intervals[0][1] = max(intervals[0][1], intervals[1][1])
                intervals.remove(intervals[1])
                merge(self, intervals)
        
        for i in range(len(intervals) - 2):
            if intervals[i][1] >= intervals[i+1][0]:
                # merge
                intervals[i][1] = max(intervals[i][1], intervals[i+1][1])
                intervals.remove(intervals[i+1])
                # after we've merged, we want to call the merge function again on the now modified array
                merge(self, intervals)
            
            # Repeat again for the next iteration
        
        # If b >= c, then call merge on the 1st interval
        
        # Merge should replace [a,b] with [a,c] and remove [c,d] from the array
        # If there are no more intervals (we hit the end of the list without detecting an interval, return the list)
        return intervals
```

### 10. [Group Anagrams](https://leetcode.com/problems/group-anagrams/)

To find if an anagram is a string of another, we can sort them and see if they are equal. We can use the sorted string in order to group up anagrams. We initialize a dictionary where the keys are the sorted strings and the values are the anagrams of the sorted string.

If its the first time we've seen it, we add it to our dictionary, with the original value as a string. If it is not the first time we've seen the sorted string, we can append that to the existing value associated with the key.

Afterwards, the values of our dictionary are precisely the grouped anagrams!

Very clean solution I'm happy with :)

Runtime: O(n * klogk) where `k` is the average length of the strings.
Space complexity: O(n * k) where `n` is the number of strings and `k` is the average length of a string.


```
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        # how to test if one is an anagram? sort and see if it is equal (nlogn)
        # key is sorted value, value is a list containing all anagrams of sorted value seen thus far
        
        d = defaultdict(str)
        
        for string in strs:
            if ''.join(sorted(string)) in d:
                d[''.join(sorted(string))].append(string)
            else:
                d[''.join(sorted(string))] = [string]
        
        return d.values()
```