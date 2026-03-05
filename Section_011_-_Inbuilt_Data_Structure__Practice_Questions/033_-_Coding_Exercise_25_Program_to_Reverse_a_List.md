## Slicing Reverse
```python
def reverse_list(lst):
    return lst[::-1]                  # Extended slicing with step -1 creates reversed copy

print(reverse_list([1, 2, 3, 4, 5]))  # Call with sample list - [5, 4, 3, 2, 1]
# Special: Most Pythonic and concise way using slice notation
```

## Built-in Reversed
```python
def reverse_list(lst):
    return list(reversed(lst))         # reversed() returns iterator, convert to list

print(reverse_list([1, 2, 3, 4, 5]))  # Call with sample list - [5, 4, 3, 2, 1]
# Special: Uses Python's built-in reversed function
```

## For Loop Reverse
```python
def reverse_list(lst):
    result = []                        # Initialize empty result list
    for i in range(len(lst)-1, -1, -1): # Iterate from last index to first
        result.append(lst[i])            # Append each element in reverse order
    return result                       # Return reversed list

print(reverse_list([1, 2, 3, 4, 5]))  # Call with sample list - [5, 4, 3, 2, 1]
# Special: Manual index control with descending range
```

## While Loop Two Pointer
```python
def reverse_list(lst):
    left = 0                           # Initialize left pointer at start
    right = len(lst) - 1                # Initialize right pointer at end
    while left < right:                  # Continue until pointers meet
        lst[left], lst[right] = lst[right], lst[left]  # Swap elements
        left += 1                         # Move left pointer right
        right -= 1                         # Move right pointer left
    return lst                           # Return reversed list (in-place)

print(reverse_list([1, 2, 3, 4, 5]))  # Call with sample list - [5, 4, 3, 2, 1]
# Special: In-place reversal with two pointers, O(1) extra space
```

## List Comprehension
```python
def reverse_list(lst):
    return [lst[i] for i in range(len(lst)-1, -1, -1)]  # List comprehension with reversed indices

print(reverse_list([1, 2, 3, 4, 5]))  # Call with sample list - [5, 4, 3, 2, 1]
# Special: Functional style using list comprehension
```

## Pop and Insert
```python
def reverse_list(lst):
    result = []                        # Initialize empty result
    for _ in range(len(lst)):           # Loop for each element
        result.append(lst.pop())         # Pop from end and append to result
    return result                       # Return reversed list

print(reverse_list([1, 2, 3, 4, 5]))  # Call with sample list - [5, 4, 3, 2, 1]
# Special: Destructive reversal using pop()
```

## Recursive Reverse
```python
def reverse_list(lst):
    if len(lst) <= 1:                  # Base case: empty or single element
        return lst                       # Return as-is
    return reverse_list(lst[1:]) + [lst[0]]  # Recursively reverse rest and append first

print(reverse_list([1, 2, 3, 4, 5]))  # Call with sample list - [5, 4, 3, 2, 1]
# Special: Recursive approach breaking into smaller pieces
```

## Deque Reverse
```python
from collections import deque

def reverse_list(lst):
    dq = deque(lst)                    # Convert list to deque
    result = []                         # Initialize result list
    while dq:                            # While deque not empty
        result.append(dq.pop())           # Pop from right and append
    return result                        # Return reversed list

print(reverse_list([1, 2, 3, 4, 5]))  # Call with sample list - [5, 4, 3, 2, 1]
# Special: Uses deque's efficient pop operations
```

## Numpy Flip
```python
import numpy as np

def reverse_list(lst):
    return np.flip(lst).tolist()        # NumPy flip reverses array, convert back to list

print(reverse_list([1, 2, 3, 4, 5]))  # Call with sample list - [5, 4, 3, 2, 1]
# Special: NumPy optimized for numerical arrays
```

## Reduce Reverse
```python
from functools import reduce

def reverse_list(lst):
    return reduce(lambda acc, x: [x] + acc, lst, [])  # Accumulator building reversed list

print(reverse_list([1, 2, 3, 4, 5]))  # Call with sample list - [5, 4, 3, 2, 1]
# Special: Functional programming with reduce
```