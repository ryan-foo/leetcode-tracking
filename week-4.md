# Week 4
-------------------------

## `(ryan-foo.github.io)`

### 1. [Valid Perfect Square](https://leetcode.com/problems/valid-perfect-square/)

#### Notes

We aim to find if a number is a valid perfect square. A number is a valid perfect square if its square root is an integer.

Two ways to solve this.

(1) An algorithm that takes advantage of the property that the difference between perfect squares increases by 2 each time. (4-1=3, 9-4=5, 16-9=7, etc)

We iteratively subtract `i` from num.

This runs in `O(sqrt(n))` time complexity where `n` is the num.

We exit the while loop on two conditions: when num == 0, which means that it is a perfect square, or when num < 0, which means that it is NOT a perfect square. Either way, we are guaranteed to terminate because we continually increase `i` and our termination measure is `num`.

```
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        i = 1
        while num > 0:
            num -= i
            i += 2
        return num == 0

```

(2) Binary search for the correct square root. (which has the advantage of being able to give you the square root without the built in `sqrt` function.)

This runs in `O(logn)` time complexity where `n` is the num, as we reduce the search space in half each time.

```
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        if num == 1:
            return True
        if num == 2:
            return False
        
        left, right = (2, num)
        while left <= right:
            mid = (left + right) // 2
            if mid ** 2 == num:
                return True
            elif mid ** 2 > num:
                right = mid - 1
            else:
                left = mid + 1
        return False
```

### [2. Missing Number](https://leetcode.com/problems/missing-number/)

The intuition is that we can add up all the numbers, and also find the number we expect to see. Then the missing number is precisely the expected sum minus the total sum of the list.

We use O(1) space complexity and O(n) runtime. It is O(1) space as we only initialize a variable expected_sum and total_sum, and not a full list. It is O(n) time complexity with respect to the size of the list as we iterate through the list of nums in order to get the expected sum.

```
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        expected_sum = 0
        for i in range(len(nums) +1):
            expected_sum += i
        total_sum = sum(nums)
        return (expected_sum - total_sum)
```

### [3. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)

The proof here is in the pigeonhole principle -- if you have n holes, and n+1 pigeons, then necessarily 1 pigeon is a lonely boy.

If we sort, the duplicate pigeons will be next to each other. Squawk away friend, we caught you and we know who you are.

`O(nlogn)` time complexity. O(1) space complexity (as we sort in place.) Things I learnt: the sorting in place method for python (`nums.sort()`), as compared to sorting the list and storing it in a variable (`sorted_nums = sorted(nums)`).

```
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        # Pigeonhole principle
        nums.sort()
        for i in range(1, len(nums)):
            if nums[i-1] == nums[i]:
                return nums[i]
```

### [4. Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern)

This is a different approach, which takes the insight that the beginning of a substring is going to be repeated many times.

Additionally, if it is perfectly repeated and the substring is correct (i.e the answer is True), then dividing the length of the string by the length of the substring should give us a whole number (times to repeat).

We begin by iteratively adding to the substring from the left side.

Then we check whether repeating the substring that many times (times to repeat) gives us the original string. If so, return True.

If we've made it through half the list, then the substrings are not equal and we can just stop searching and return False.

The time complexity is `O(n^2)` where `n` is the length of the input string. This is because appending to a substring is `O(n)` and we repeat that operation `n/2` times in the worst case. Additionally, equality comparison is `O(n)` where `n` is the length of the substring being compared. 

The space complexity is `O(n/2)` as we maintain the current substring up to `n/2`.

```
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        curr_substring = ''
        for i in range(0, len(s) // 2):
            curr_substring += s[i]
            times_to_repeat = len(s) // len(curr_substring)
            if ((curr_substring * times_to_repeat) == s):
                return True
        return False
```
```
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        curr_substring = ''
        for i in range(0, len(s) // 2):
            curr_substring += s[i]
            times_to_repeat = len(s) // len(curr_substring)
            if ((curr_substring * times_to_repeat) == s):
                return True
        return False
```