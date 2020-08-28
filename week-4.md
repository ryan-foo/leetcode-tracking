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