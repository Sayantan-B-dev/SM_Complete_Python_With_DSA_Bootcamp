## Slicing Rotation
```python
def rotate_list(lst, k):
    if len(lst) != 0:                     # Handle empty list case
        k = k % len(lst)                    # Normalize k to effective rotation amount
    new_list_left = lst[-k:]               # Slice from -k to end (elements that move to front)
    new_list_right = lst[:-k]              # Slice from start to -k (elements that move to back)
    return new_list_left + new_list_right  # Concatenate rotated parts

print(rotate_list([1, 2, 3, 4, 5], 2))    # Call with list and rotation - [4, 5, 1, 2, 3]
# Special: Clean slicing approach with modulo for k > length
```

## Deque Rotate
```python
from collections import deque

def rotate_list(lst, k):
    if not lst:                           # Check for empty list
        return lst
    dq = deque(lst)                        # Convert to deque for efficient rotation
    dq.rotate(k)                           # Deque's built-in rotate (positive k rotates right)
    return list(dq)                         # Convert back to list

print(rotate_list([1, 2, 3, 4, 5], 2))    # Call with list and rotation - [4, 5, 1, 2, 3]
# Special: Uses collections.deque's optimized rotate method
```

## Pop and Insert Loop
```python
def rotate_list(lst, k):
    if not lst:                            # Handle empty list
        return lst
    k = k % len(lst)                        # Normalize rotation amount
    result = lst.copy()                      # Create a copy to avoid modifying original
    for _ in range(k):                       # Perform k rotations
        result.insert(0, result.pop())         # Pop from end, insert at beginning
    return result                            # Return rotated list

print(rotate_list([1, 2, 3, 4, 5], 2))    # Call with list and rotation - [4, 5, 1, 2, 3]
# Special: Simulates rotation one step at a time
```

## Two-Pass Reversal
```python
def rotate_list(lst, k):
    if not lst:                            # Handle empty list
        return lst
    n = len(lst)                            # Get list length
    k = k % n                               # Normalize k
    
    def reverse_slice(arr, start, end):     # Helper to reverse portion in-place
        while start < end:                   # Two-pointer reversal
            arr[start], arr[end] = arr[end], arr[start]  # Swap elements
            start += 1                         # Move start forward
            end -= 1                            # Move end backward
    
    result = lst.copy()                      # Work on a copy
    reverse_slice(result, 0, n-1)             # Reverse entire list
    reverse_slice(result, 0, k-1)             # Reverse first k elements
    reverse_slice(result, k, n-1)             # Reverse remaining elements
    return result                            # Return rotated list

print(rotate_list([1, 2, 3, 4, 5], 2))    # Call with list and rotation - [4, 5, 1, 2, 3]
# Special: O(n) time, O(1) extra space using reversal algorithm
```

## List Comprehension with Index Mapping
```python
def rotate_list(lst, k):
    if not lst:                            # Handle empty list
        return lst
    n = len(lst)                            # Get list length
    k = k % n                               # Normalize k
    return [lst[(i - k) % n] for i in range(n)]  # Map each new index to old index

print(rotate_list([1, 2, 3, 4, 5], 2))    # Call with list and rotation - [4, 5, 1, 2, 3]
# Special: Mathematical index mapping with modulo arithmetic
```

## Numpy Roll
```python
import numpy as np

def rotate_list(lst, k):
    if not lst:                            # Handle empty list
        return lst
    arr = np.array(lst)                     # Convert to numpy array
    rotated = np.roll(arr, k)                # NumPy's roll function for rotation
    return rotated.tolist()                  # Convert back to list

print(rotate_list([1, 2, 3, 4, 5], 2))    # Call with list and rotation - [4, 5, 1, 2, 3]
# Special: NumPy's optimized roll function for array rotation
```

## Recursive Rotation
```python
def rotate_list(lst, k):
    if not lst or k == 0:                   # Base cases: empty list or no rotation
        return lst
    n = len(lst)                            # Get list length
    k = k % n                               # Normalize k
    if k == 0:                              # If k is 0 after normalization
        return lst
    
    def rotate_one(arr):                     # Helper for single rotation
        if not arr:
            return arr
        return [arr[-1]] + arr[:-1]           # Move last to front
    
    if k > 0:                                # Positive rotation
        rotated_once = rotate_one(lst)         # Rotate by one
        return rotate_list(rotated_once, k-1)  # Recursively rotate k-1 more times

print(rotate_list([1, 2, 3, 4, 5], 2))    # Call with list and rotation - [4, 5, 1, 2, 3]
# Special: Recursive approach rotating one step at a time
```

## Concatenate with Modulo Indexing
```python
def rotate_list(lst, k):
    if not lst:                            # Handle empty list
        return lst
    n = len(lst)                            # Get list length
    k = k % n                               # Normalize k
    return lst[n-k:] + lst[:n-k]             # Slice and concatenate directly

print(rotate_list([1, 2, 3, 4, 5], 2))    # Call with list and rotation - [4, 5, 1, 2, 3]
# Special: Direct formula using list slicing without temporary variables
```

## For Loop with New List
```python
def rotate_list(lst, k):
    if not lst:                            # Handle empty list
        return lst
    n = len(lst)                            # Get list length
    k = k % n                               # Normalize k
    result = [0] * n                         # Initialize result list with placeholders
    
    for i in range(n):                       # Iterate through each position
        result[(i + k) % n] = lst[i]          # Place each element at rotated position
    
    return result                            # Return rotated list

print(rotate_list([1, 2, 3, 4, 5], 2))    # Call with list and rotation - [4, 5, 1, 2, 3]
# Special: Explicit mapping using modulo arithmetic in loop
```

## Extended Slices with Step
```python
def rotate_list(lst, k):
    if not lst:                            # Handle empty list
        return lst
    n = len(lst)                            # Get list length
    k = k % n                               # Normalize k
    
    if k == 0:                              # No rotation needed
        return lst.copy()
    
    # Create a new list by taking elements in rotated order using extended slices
    return lst[-k:][::1] + lst[:-k][::1]     # Explicit slices (redundant but clear)

print(rotate_list([1, 2, 3, 4, 5], 2))    # Call with list and rotation - [4, 5, 1, 2, 3]
# Special: Demonstrates explicit step parameter in slices
```