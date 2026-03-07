## Find Maximum Consecutive Ones: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Given a binary array (containing only 0s and 1s), find the maximum number of consecutive 1s. The goal is to efficiently traverse the array, track the current streak of 1s, and update the maximum streak whenever a longer streak is found.

**Approach:**  
Several methods exist: sliding window (two pointers), simple counter resetting on zeros, splitting the array by zeros, using groupby, etc. The most efficient is a single pass O(n) with constant space.

**Algorithm (Two-Pointer):**  
1. Initialize `slow = 0`, `fast = 0`, `max_con = 0`.  
2. While `fast < len(nums)`:  
   - If `nums[fast] == 1`:  
     - Update `max_con = max(max_con, fast - slow + 1)`.  
     - Increment `fast`.  
   - Else (found 0):  
     - Increment `fast`, set `slow = fast`.  
3. Return `max_con`.

**Pseudocode:**  
```
function maxConsecutiveOnes(nums):
    slow = 0
    fast = 0
    maxLen = 0
    while fast < length(nums):
        if nums[fast] == 1:
            maxLen = max(maxLen, fast - slow + 1)
            fast = fast + 1
        else:
            fast = fast + 1
            slow = fast
    return maxLen
```

---

## Original Two-Pointer Implementation

```python
def max_consecutive_ones_two_pointer(nums):
    slow = 0
    fast = 0
    max_con = 0
    # Main logic: expand window while seeing 1s, reset on 0
    while fast < len(nums):
        if nums[fast] == 1:
            # Update maximum length for current streak
            max_con = max(max_con, fast - slow + 1)
            fast += 1
        else:
            # Zero encountered, move both pointers past it
            fast += 1
            slow = fast
    return max_con

# Example call
nums = [1, 1, 0, 1, 1, 1]
result = max_consecutive_ones_two_pointer(nums)
print("Max consecutive ones:", result)   # Output: Max consecutive ones: 3
```

---

## Simple Counter Reset on Zero

```python
def max_consecutive_ones_counter(nums):
    max_con = 0
    current = 0
    # Main logic: increment counter for 1, reset on 0, track max
    for num in nums:
        if num == 1:
            current += 1
            max_con = max(max_con, current)   # update max
        else:
            current = 0                         # reset streak
    return max_con

# Example call
nums = [1, 1, 0, 1, 1, 1]
result = max_consecutive_ones_counter(nums)
print("Max consecutive ones:", result)   # Output: Max consecutive ones: 3
```

---

## Using String Split

Convert array to string, split on zeros, find longest substring of ones.

```python
def max_consecutive_ones_split(nums):
    # Convert list to string without spaces
    s = ''.join(str(x) for x in nums)
    # Split on '0' to get segments of consecutive 1s
    segments = s.split('0')
    # Find the longest segment and return its length
    return max(len(seg) for seg in segments)

# Example call
nums = [1, 1, 0, 1, 1, 1]
result = max_consecutive_ones_split(nums)
print("Max consecutive ones:", result)   # Output: Max consecutive ones: 3
```

---

## Using `itertools.groupby`

Group consecutive equal elements and compute max length for groups of 1s.

```python
from itertools import groupby

def max_consecutive_ones_groupby(nums):
    max_con = 0
    # groupby returns (key, group_iterator)
    for key, group in groupby(nums):
        if key == 1:
            length = len(list(group))          # count ones in this group
            max_con = max(max_con, length)
    return max_con

# Example call
nums = [1, 1, 0, 1, 1, 1]
result = max_consecutive_ones_groupby(nums)
print("Max consecutive ones:", result)   # Output: Max consecutive ones: 3
```

---

## Dynamic Programming Style (Running Sum Reset)

Treat current streak as a running sum that resets on zero.

```python
def max_consecutive_ones_dp(nums):
    max_con = 0
    current = 0
    # Main logic: current = current + 1 if 1 else 0
    for num in nums:
        current = (current + 1) if num == 1 else 0
        max_con = max(max_con, current)
    return max_con

# Example call
nums = [1, 1, 0, 1, 1, 1]
result = max_consecutive_ones_dp(nums)
print("Max consecutive ones:", result)   # Output: Max consecutive ones: 3
```

---

## Using Enumerate to Track Start Index

Similar to two-pointer but uses index tracking with reset.

```python
def max_consecutive_ones_enumerate(nums):
    max_con = 0
    start = 0
    # enumerate gives index and value
    for i, val in enumerate(nums):
        if val == 0:
            start = i + 1                     # reset start after zero
        else:
            max_con = max(max_con, i - start + 1)
    return max_con

# Example call
nums = [1, 1, 0, 1, 1, 1]
result = max_consecutive_ones_enumerate(nums)
print("Max consecutive ones:", result)   # Output: Max consecutive ones: 3
```

---

## Using Recursion

Recursively process the array, returning the max of left, right, and crossing.

```python
def max_consecutive_ones_recursive(nums, left, right):
    # Base case: single element
    if left == right:
        return 1 if nums[left] == 1 else 0
    mid = (left + right) // 2
    # Recursively get max from left and right halves
    left_max = max_consecutive_ones_recursive(nums, left, mid)
    right_max = max_consecutive_ones_recursive(nums, mid + 1, right)
    # Calculate crossing max: extend from mid to left and right
    left_count = 0
    i = mid
    while i >= left and nums[i] == 1:
        left_count += 1
        i -= 1
    right_count = 0
    j = mid + 1
    while j <= right and nums[j] == 1:
        right_count += 1
        j += 1
    cross_max = left_count + right_count
    return max(left_max, right_max, cross_max)

def max_consecutive_ones_rec(nums):
    if not nums:
        return 0
    return max_consecutive_ones_recursive(nums, 0, len(nums) - 1)

# Example call
nums = [1, 1, 0, 1, 1, 1]
result = max_consecutive_ones_rec(nums)
print("Max consecutive ones:", result)   # Output: Max consecutive ones: 3
```

---

## Using `max` and `accumulate` from `itertools`

`accumulate` can carry a running count that resets on zero.

```python
from itertools import accumulate

def max_consecutive_ones_accumulate(nums):
    # Define a function to carry the count: carry = (carry + 1) if x==1 else 0
    counts = list(accumulate(nums, lambda carry, x: (carry + 1) if x == 1 else 0, initial=0))
    # counts[0] is initial 0, so ignore; max of rest gives max streak
    return max(counts[1:]) if nums else 0

# Example call
nums = [1, 1, 0, 1, 1, 1]
result = max_consecutive_ones_accumulate(nums)
print("Max consecutive ones:", result)   # Output: Max consecutive ones: 3
```

---

## Using `numpy` for Vectorized Operations

If numpy is available, we can use it to find consecutive ones efficiently.

```python
import numpy as np

def max_consecutive_ones_numpy(nums):
    arr = np.array(nums)
    # Find positions where value changes
    diff = np.diff(np.concatenate(([0], arr, [0])))
    starts = np.where(diff == 1)[0]      # indices where 1 starts
    ends = np.where(diff == -1)[0]       # indices where 1 ends
    if len(starts) == 0:
        return 0
    lengths = ends - starts
    return np.max(lengths)

# Example call
nums = [1, 1, 0, 1, 1, 1]
result = max_consecutive_ones_numpy(nums)
print("Max consecutive ones:", result)   # Output: Max consecutive ones: 3
```

---

## Using Regular Expressions

Convert to string and use regex to find all runs of 1s, then get max length.

```python
import re

def max_consecutive_ones_regex(nums):
    s = ''.join(str(x) for x in nums)
    # Find all sequences of '1'
    matches = re.findall(r'1+', s)
    # Get max length, default 0 if no matches
    return max((len(m) for m in matches), default=0)

# Example call
nums = [1, 1, 0, 1, 1, 1]
result = max_consecutive_ones_regex(nums)
print("Max consecutive ones:", result)   # Output: Max consecutive ones: 3
```