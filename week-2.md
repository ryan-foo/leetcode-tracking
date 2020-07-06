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