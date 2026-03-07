## Find Missing Number: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Given an array `nums` containing `n` distinct numbers taken from the range `[0, n]`, find the one missing number. The array length is `n`, and it should contain all numbers from `0` to `n` inclusive except one. The task is to identify the missing number efficiently.

**Approach:**  
Several approaches exist: using the sum formula (Gauss), XOR bit manipulation, sorting and scanning, using sets, binary search (if sorted), or in-place index marking. Each has different time and space complexities.

**Algorithm (Sum Formula):**  
1. Let `n = len(nums)`.  
2. Compute `expected_sum = n * (n + 1) // 2`.  
3. Compute `actual_sum = sum(nums)`.  
4. Return `expected_sum - actual_sum`.

**Pseudocode:**  
```
function findMissing(nums):
    n = length(nums)
    expected = n * (n + 1) / 2
    actual = sum(nums)
    return expected - actual
```

---

## Sum Method (Original)

```python
def find_missing_sum(nums):
    n = len(nums)
    # Expected sum of 0..n (inclusive) using Gauss formula
    expected_sum = n * (n + 1) // 2
    # Actual sum of given numbers
    actual_sum = sum(nums)
    # Difference gives the missing number
    return expected_sum - actual_sum

# Example call
nums = [3, 0, 1]
result = find_missing_sum(nums)
print("Missing number:", result)   # Output: Missing number: 2
```

---

## XOR Method

Uses the property that XOR of a number with itself cancels out.

```python
def find_missing_xor(nums):
    missing = len(nums)                  # start with n (since range is 0..n)
    for i, num in enumerate(nums):
        # XOR with index and current number
        missing ^= i ^ num
    return missing

# Example call
nums = [9, 6, 4, 2, 3, 5, 7, 0, 1]
result = find_missing_xor(nums)
print("Missing number:", result)   # Output: Missing number: 8
```

---

## Sorting and Linear Scan

After sorting, find the first place where index and value don't match.

```python
def find_missing_sort(nums):
    nums.sort()
    # Check each index against its value
    for i, num in enumerate(nums):
        if i != num:
            return i
    # If all match, missing is n
    return len(nums)

# Example call
nums = [3, 0, 1]
result = find_missing_sort(nums)
print("Missing number:", result)   # Output: Missing number: 2
```

---

## Set Difference

Create a set of the range and subtract the given set.

```python
def find_missing_set(nums):
    n = len(nums)
    # Create set of all numbers from 0 to n
    full_set = set(range(n + 1))
    # Remove the given numbers
    missing = full_set - set(nums)
    # Pop the single element
    return missing.pop()

# Example call
nums = [9, 6, 4, 2, 3, 5, 7, 0, 1]
result = find_missing_set(nums)
print("Missing number:", result)   # Output: Missing number: 8
```

---

## Boolean Array (Hash Set)

Use a boolean array to mark seen numbers, then find the unmarked.

```python
def find_missing_boolean(nums):
    n = len(nums)
    seen = [False] * (n + 1)
    for num in nums:
        seen[num] = True
    for i in range(n + 1):
        if not seen[i]:
            return i

# Example call
nums = [3, 0, 1]
result = find_missing_boolean(nums)
print("Missing number:", result)   # Output: Missing number: 2
```

---

## Binary Search on Sorted Input

If the array is sorted (or we sort it), we can use binary search to find the missing number.

```python
def find_missing_binary_search(nums):
    nums.sort()
    left, right = 0, len(nums)
    while left < right:
        mid = (left + right) // 2
        if nums[mid] > mid:
            right = mid
        else:
            left = mid + 1
    return left

# Example call
nums = [0, 1, 3, 4]
result = find_missing_binary_search(nums)
print("Missing number:", result)   # Output: Missing number: 2
```

---

## Index Marking (Cyclic Sort)

Place each number at its correct index by swapping, then find the index that doesn't match.

```python
def find_missing_cyclic(nums):
    n = len(nums)
    i = 0
    while i < n:
        # If current number is within range and not at correct position, swap
        if nums[i] < n and nums[i] != nums[nums[i]]:
            nums[nums[i]], nums[i] = nums[i], nums[nums[i]]
        else:
            i += 1
    # After rearranging, find index where value != index
    for i in range(n):
        if i != nums[i]:
            return i
    return n

# Example call
nums = [3, 0, 1]
result = find_missing_cyclic(nums)
print("Missing number:", result)   # Output: Missing number: 2
```

---

## Using `enumerate` and Sum of Indices

Compute sum of indices and subtract sum of numbers to get missing.

```python
def find_missing_index_sum(nums):
    n = len(nums)
    # Sum of indices 0..n-1 plus n (since we need 0..n)
    index_sum = sum(range(n + 1))
    # Subtract sum of numbers
    return index_sum - sum(nums)

# Example call
nums = [9, 6, 4, 2, 3, 5, 7, 0, 1]
result = find_missing_index_sum(nums)
print("Missing number:", result)   # Output: Missing number: 8
```

---

## Using `collections.Counter`

Create a frequency counter and find the missing number.

```python
from collections import Counter

def find_missing_counter(nums):
    n = len(nums)
    freq = Counter(nums)
    for i in range(n + 1):
        if freq[i] == 0:
            return i

# Example call
nums = [3, 0, 1]
result = find_missing_counter(nums)
print("Missing number:", result)   # Output: Missing number: 2
```

---

## Using `reduce` from `functools` (XOR with reduce)

Use `reduce` to apply XOR across all numbers and indices.

```python
from functools import reduce

def find_missing_reduce(nums):
    n = len(nums)
    # XOR all indices from 0 to n
    xor_indices = reduce(lambda x, y: x ^ y, range(n + 1))
    # XOR all numbers in nums
    xor_nums = reduce(lambda x, y: x ^ y, nums)
    return xor_indices ^ xor_nums

# Example call
nums = [0, 1, 3]
result = find_missing_reduce(nums)
print("Missing number:", result)   # Output: Missing number: 2
```