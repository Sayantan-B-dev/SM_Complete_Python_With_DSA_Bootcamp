Basic Recursion
```python
def sum_list(numbers):
    if len(numbers)==0:          # Base case: empty list
        return 0                  # Return 0 for empty list
    return numbers[0] + sum_list(numbers[1:])  # Add first element to sum of rest

print(sum_list([1, 2, 3, 4, 5]))  # Call function with sample list
# Special: Classic recursion breaking problem into smaller pieces
```

Tail Recursion
```python
def sum_list(numbers, accumulator=0):
    if len(numbers)==0:            # Base case: empty list
        return accumulator          # Return accumulated sum
    return sum_list(numbers[1:], accumulator + numbers[0])  # Tail recursive call

print(sum_list([1, 2, 3, 4, 5]))  # Call with initial accumulator=0
# Special: Accumulator enables tail call optimization
```

Built-in Sum
```python
def sum_list(numbers):
    return sum(numbers)             # Python's built-in sum function

print(sum_list([1, 2, 3, 4, 5]))  # Call function
# Special: Most efficient, implemented in C
```

For Loop
```python
def sum_list(numbers):
    total = 0                       # Initialize accumulator
    for num in numbers:             # Iterate through each element
        total += num                 # Add current number to total
    return total                     # Return final sum

print(sum_list([1, 2, 3, 4, 5]))  # Call function
# Special: Clear iterative logic
```

While Loop
```python
def sum_list(numbers):
    total = 0                       # Initialize sum
    i = 0                           # Initialize index
    while i < len(numbers):          # Loop until end of list
        total += numbers[i]           # Add element at index i
        i += 1                        # Increment index
    return total                     # Return final sum

print(sum_list([1, 2, 3, 4, 5]))  # Call function
# Special: Manual index control
```

Reduce Function
```python
from functools import reduce

def sum_list(numbers):
    return reduce(lambda x, y: x + y, numbers, 0)  # Reduce with addition lambda

print(sum_list([1, 2, 3, 4, 5]))  # Call function
# Special: Functional programming approach
```

List Comprehension
```python
def sum_list(numbers):
    return sum([num for num in numbers])  # List comprehension feeding into sum

print(sum_list([1, 2, 3, 4, 5]))  # Call function
# Special: Creates intermediate list then sums
```

Generator Expression
```python
def sum_list(numbers):
    return sum(num for num in numbers)  # Generator expression (no intermediate list)

print(sum_list([1, 2, 3, 4, 5]))  # Call function
# Special: Memory efficient, lazy evaluation
```

Math.fsum
```python
import math

def sum_list(numbers):
    return math.fsum(numbers)       # High precision floating point sum

print(sum_list([1, 2, 3, 4, 5]))  # Call function
# Special: Tracks partial sums to minimize floating point error
```

Numpy Sum
```python
import numpy as np

def sum_list(numbers):
    return np.sum(numbers)           # NumPy's optimized array sum

print(sum_list([1, 2, 3, 4, 5]))  # Call function
# Special: Optimized for numerical arrays, vectorized operations
```