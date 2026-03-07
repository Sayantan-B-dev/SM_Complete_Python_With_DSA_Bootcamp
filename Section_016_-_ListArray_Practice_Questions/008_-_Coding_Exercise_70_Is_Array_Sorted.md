## Check if List is Sorted (Non-Decreasing): Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
A list is considered sorted (non-decreasing) if every element is less than or equal to the next element. Checking this involves comparing each adjacent pair and ensuring the condition holds.

**Approach:**  
Several approaches exist: direct iteration, using built-in functions like `all()`, `zip`, `itertools.pairwise`, recursion, or even comparing with a sorted copy. The most efficient is a single pass O(n) comparison.

**Algorithm:**  
1. For `i = 0` to `len(arr)-2`:  
   - If `arr[i] > arr[i+1]`, return `False`.  
2. Return `True`.

**Pseudocode:**  
```
function isSorted(arr):
    for i = 0 to length(arr)-2:
        if arr[i] > arr[i+1]:
            return False
    return True
```

---

## Basic Iterative Check

```python
def is_sorted_iterative(arr):
    # Main logic: compare each adjacent pair
    for i in range(len(arr) - 1):
        if arr[i] > arr[i + 1]:          # if current > next, not sorted
            return False
    return True                           # all pairs satisfied

# Example call
test_list = [1, 2, 2, 3, 5]
result = is_sorted_iterative(test_list)
print("Is sorted:", result)   # Output: Is sorted: True
```

---

## Using `all()` with Generator

```python
def is_sorted_all(arr):
    # all() checks if all conditions are True; generator yields comparisons
    return all(arr[i] <= arr[i + 1] for i in range(len(arr) - 1))

# Example call
test_list = [1, 2, 2, 3, 5]
result = is_sorted_all(test_list)
print("Is sorted:", result)   # Output: Is sorted: True
```

---

## Using `zip` and `all`

```python
def is_sorted_zip(arr):
    # zip creates pairs (arr[i], arr[i+1]) for comparison
    return all(x <= y for x, y in zip(arr, arr[1:]))

# Example call
test_list = [1, 2, 2, 3, 5]
result = is_sorted_zip(test_list)
print("Is sorted:", result)   # Output: Is sorted: True
```

---

## Using `itertools.pairwise` (Python 3.10+)

```python
from itertools import pairwise

def is_sorted_pairwise(arr):
    # pairwise yields (arr[i], arr[i+1]) tuples
    return all(x <= y for x, y in pairwise(arr))

# Example call
test_list = [1, 2, 2, 3, 5]
result = is_sorted_pairwise(test_list)
print("Is sorted:", result)   # Output: Is sorted: True
```

---

## Recursive Check

```python
def is_sorted_recursive(arr):
    # Base case: single element or empty is sorted
    if len(arr) <= 1:
        return True
    # Check first pair and recurse on rest
    if arr[0] > arr[1]:
        return False
    return is_sorted_recursive(arr[1:])   # check tail

# Example call
test_list = [1, 2, 2, 3, 5]
result = is_sorted_recursive(test_list)
print("Is sorted:", result)   # Output: Is sorted: True
```

---

## Compare with Sorted Version (Inefficient)

```python
def is_sorted_sorted(arr):
    # Compare original list with its sorted version (creates new list)
    return arr == sorted(arr)

# Example call
test_list = [1, 2, 2, 3, 5]
result = is_sorted_sorted(test_list)
print("Is sorted:", result)   # Output: Is sorted: True
```

---

## Using `while` Loop with Index

```python
def is_sorted_while(arr):
    i = 0
    while i < len(arr) - 1:
        if arr[i] > arr[i + 1]:
            return False
        i += 1
    return True

# Example call
test_list = [1, 2, 2, 3, 5]
result = is_sorted_while(test_list)
print("Is sorted:", result)   # Output: Is sorted: True
```

---

## Using `numpy.diff()`

```python
import numpy as np

def is_sorted_numpy(arr):
    # Convert to numpy array and compute differences
    diff = np.diff(arr)
    # All differences should be >=0 for non-decreasing order
    return np.all(diff >= 0)

# Example call
test_list = [1, 2, 2, 3, 5]
result = is_sorted_numpy(test_list)
print("Is sorted:", result)   # Output: Is sorted: True
```

---

## Using `enumerate` and Checking Next Element

```python
def is_sorted_enumerate(arr):
    for i, val in enumerate(arr[:-1]):
        if val > arr[i + 1]:
            return False
    return True

# Example call
test_list = [1, 2, 2, 3, 5]
result = is_sorted_enumerate(test_list)
print("Is sorted:", result)   # Output: Is sorted: True
```

---

## Using `functools.reduce` with Early Exit Simulation

```python
from functools import reduce

def is_sorted_reduce(arr):
    # reduce accumulates but we can't break early; we use it to carry previous
    # and a flag. Here we use it to ensure all pairs satisfy.
    def check(prev, curr):
        if prev is None:
            return curr
        if prev > curr:
            # Force stop by raising exception? Not ideal. Instead, we'll accumulate a sentinel.
            # Alternative: use all() approach. This is just a demonstration.
            raise ValueError
        return curr
    try:
        reduce(check, arr, None)
        return True
    except ValueError:
        return False

# Example call
test_list = [1, 2, 2, 3, 5]
result = is_sorted_reduce(test_list)
print("Is sorted:", result)   # Output: Is sorted: True
```

---

## Strictly Increasing Variant (Optional)

```python
def is_strictly_increasing(arr):
    # Check if each element is strictly less than the next
    return all(arr[i] < arr[i + 1] for i in range(len(arr) - 1))

# Example call
test_list = [1, 2, 3, 4, 5]
result = is_strictly_increasing(test_list)
print("Is strictly increasing:", result)   # Output: Is strictly increasing: True
```