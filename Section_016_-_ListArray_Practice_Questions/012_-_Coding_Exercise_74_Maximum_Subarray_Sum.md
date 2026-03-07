## Maximum Subarray Sum (Kadane's Algorithm): Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
The maximum subarray sum problem asks for the contiguous subarray within a one-dimensional array of numbers that has the largest sum. Kadane's algorithm solves this efficiently in O(n) time by scanning the array and maintaining a running sum, resetting when it becomes negative.

**Approach:**  
- Initialize `current_sum` and `max_sum` with the first element.  
- Iterate through the rest of the array.  
- For each element, decide whether to extend the current subarray or start a new one: `current_sum = max(element, current_sum + element)`.  
- Update `max_sum` with the larger of itself and `current_sum`.  
- At the end, `max_sum` holds the maximum subarray sum.

**Algorithm:**  
1. If array is empty, return 0 (or appropriate sentinel).  
2. Set `current_sum = arr[0]`, `max_sum = arr[0]`.  
3. For `i = 1` to `len(arr)-1`:  
   - `current_sum = max(arr[i], current_sum + arr[i])`.  
   - `max_sum = max(max_sum, current_sum)`.  
4. Return `max_sum`.

**Pseudocode:**  
```
function maxSubarraySum(arr):
    if arr is empty: return 0
    current = arr[0]
    maxSum = arr[0]
    for i = 1 to len(arr)-1:
        current = max(arr[i], current + arr[i])
        maxSum = max(maxSum, current)
    return maxSum
```

---

## Basic Kadane's Algorithm (as given)

```python
def max_subarray_sum_kadane(arr):
    if len(arr) == 0:                     # handle empty array
        return 0
    current_sum = arr[0]                   # best sum ending at current position
    max_sum = arr[0]                        # overall best sum
    for i in range(1, len(arr)):
        # Main logic: decide to extend or start new subarray
        current_sum = max(arr[i], current_sum + arr[i])
        # Update global maximum
        max_sum = max(max_sum, current_sum)
    return max_sum

# Example call
arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
result = max_subarray_sum_kadane(arr)
print("Maximum subarray sum:", result)   # Output: Maximum subarray sum: 6
```

---

## Kadane's Algorithm Returning Indices

```python
def max_subarray_with_indices(arr):
    if not arr:
        return 0, -1, -1
    current_sum = arr[0]
    max_sum = arr[0]
    start = end = 0
    temp_start = 0
    for i in range(1, len(arr)):
        # Main logic: decide to extend or start new
        if arr[i] > current_sum + arr[i]:
            current_sum = arr[i]
            temp_start = i                    # start new subarray here
        else:
            current_sum += arr[i]
        # Update global max and indices
        if current_sum > max_sum:
            max_sum = current_sum
            start = temp_start
            end = i
    return max_sum, start, end

# Example call
arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
max_sum, s, e = max_subarray_with_indices(arr)
print(f"Max sum: {max_sum}, from index {s} to {e}")   # Output: Max sum: 6, from index 3 to 6
```

---

## Divide and Conquer Approach

```python
def max_crossing_sum(arr, left, mid, right):
    # Left side sum (including mid)
    left_sum = float('-inf')
    total = 0
    for i in range(mid, left - 1, -1):
        total += arr[i]
        left_sum = max(left_sum, total)
    # Right side sum (excluding mid)
    right_sum = float('-inf')
    total = 0
    for i in range(mid + 1, right + 1):
        total += arr[i]
        right_sum = max(right_sum, total)
    # Return combined crossing sum
    return left_sum + right_sum

def max_subarray_divide_conquer(arr, left, right):
    # Base case: single element
    if left == right:
        return arr[left]
    mid = (left + right) // 2
    # Recursively compute max in left, right, and crossing
    left_max = max_subarray_divide_conquer(arr, left, mid)
    right_max = max_subarray_divide_conquer(arr, mid + 1, right)
    cross_max = max_crossing_sum(arr, left, mid, right)
    # Return the maximum of the three
    return max(left_max, right_max, cross_max)

def max_subarray_sum_dc(arr):
    if not arr:
        return 0
    return max_subarray_divide_conquer(arr, 0, len(arr) - 1)

# Example call
arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
result = max_subarray_sum_dc(arr)
print("Maximum subarray sum (divide & conquer):", result)   # Output: 6
```

---

## Dynamic Programming Table

```python
def max_subarray_dp_table(arr):
    if not arr:
        return 0
    n = len(arr)
    dp = [0] * n                           # dp[i] = max sum ending at i
    dp[0] = arr[0]
    max_sum = arr[0]
    for i in range(1, n):
        # Main logic: extend previous or start new
        dp[i] = max(arr[i], dp[i - 1] + arr[i])
        max_sum = max(max_sum, dp[i])
    return max_sum

# Example call
arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
result = max_subarray_dp_table(arr)
print("Maximum subarray sum (DP table):", result)   # Output: 6
```

---

## Using Prefix Sums (Brute Force Optimized)

```python
def max_subarray_prefix(arr):
    if not arr:
        return 0
    n = len(arr)
    prefix = [0] * (n + 1)                 # prefix[0] = 0
    for i in range(n):
        prefix[i + 1] = prefix[i] + arr[i]
    max_sum = float('-inf')
    min_prefix = prefix[0]                  # smallest prefix seen so far
    for i in range(1, n + 1):
        # Main logic: max subarray sum ending at i-1 is prefix[i] - min_prefix
        max_sum = max(max_sum, prefix[i] - min_prefix)
        min_prefix = min(min_prefix, prefix[i])
    return max_sum

# Example call
arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
result = max_subarray_prefix(arr)
print("Maximum subarray sum (prefix sums):", result)   # Output: 6
```

---

## Using `itertools.accumulate` and Running Min

```python
from itertools import accumulate

def max_subarray_accumulate(arr):
    if not arr:
        return 0
    # Compute running sums
    running_sums = list(accumulate(arr))
    max_sum = arr[0]
    min_sum = 0                               # smallest prefix before current
    for s in running_sums:
        # Main logic: current max candidate = current sum - smallest previous sum
        max_sum = max(max_sum, s - min_sum)
        min_sum = min(min_sum, s)
    return max_sum

# Example call
arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
result = max_subarray_accumulate(arr)
print("Maximum subarray sum (accumulate):", result)   # Output: 6
```

---

## Recursive Kadane (Tail Recursion)

```python
def max_subarray_recursive(arr, i, current_sum, max_sum):
    if i == len(arr):
        return max_sum
    # Main logic: same as iterative but recursive
    current_sum = max(arr[i], current_sum + arr[i])
    max_sum = max(max_sum, current_sum)
    return max_subarray_recursive(arr, i + 1, current_sum, max_sum)

def max_subarray_rec(arr):
    if not arr:
        return 0
    return max_subarray_recursive(arr, 1, arr[0], arr[0])

# Example call
arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
result = max_subarray_rec(arr)
print("Maximum subarray sum (recursive):", result)   # Output: 6
```

---

## Maximum Circular Subarray (Kadane Variant)

```python
def max_circular_subarray(arr):
    if not arr:
        return 0
    # Case 1: normal Kadane
    def kadane(nums):
        cur = max_sum = nums[0]
        for x in nums[1:]:
            cur = max(x, cur + x)
            max_sum = max(max_sum, cur)
        return max_sum
    # Case 2: max subarray wraps around = total sum - min subarray sum
    total = sum(arr)
    # Invert signs to find minimum subarray sum via Kadane
    neg_arr = [-x for x in arr]
    min_subarray = -kadane(neg_arr)          # min subarray sum
    # If all numbers are negative, return normal Kadane (largest element)
    if min_subarray == total:                 # means all numbers negative
        return kadane(arr)
    return max(kadane(arr), total - min_subarray)

# Example call
arr = [8, -8, 9, -9, 10, -11, 12]
result = max_circular_subarray(arr)
print("Maximum circular subarray sum:", result)   # Output: 22 (12 + 8 -8 +9 -9 +10 = 22)
```

---

## Using `numpy` for Large Arrays

```python
import numpy as np

def max_subarray_numpy(arr):
    if len(arr) == 0:
        return 0
    # Convert to numpy array
    a = np.array(arr)
    # Compute maximum subarray sum using numpy's maximum.accumulate trick
    # This is a vectorized implementation of Kadane
    max_ending_here = np.maximum.accumulate(a)
    max_so_far = np.maximum.accumulate(max_ending_here)
    # But that's not exactly Kadane; better to use a custom loop or use np.max on prefix diff
    # Simpler: use standard Python for small, but for large, we can use:
    current = a[0]
    max_sum = a[0]
    for x in a[1:]:
        current = max(x, current + x)
        max_sum = max(max_sum, current)
    return max_sum

# Example call
arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
result = max_subarray_numpy(arr)
print("Maximum subarray sum (numpy):", result)   # Output: 6
```