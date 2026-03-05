## Lambda Filter Both
```python
def count_even_odd(lst):
    odd_count = len(list(filter(lambda x: x % 2 == 0, lst)))  # Filter evens, count length
    even_count = len(list(filter(lambda x: x % 2 != 0, lst)))  # Filter odds, count length
    return (odd_count, even_count)  # Return tuple with (even_count, odd_count)

print(count_even_odd([1, 2, 3, 4, 5, 6]))  # Call with sample list - (3, 3)
# Special: Uses lambda with filter for both counts separately
```

## For Loop Counters
```python
def count_even_odd(lst):
    even_count = 0                      # Initialize even counter
    odd_count = 0                       # Initialize odd counter
    for num in lst:                      # Iterate through each element
        if num % 2 == 0:                   # Check if number is even
            even_count += 1                  # Increment even counter
        else:                               # Number is odd
            odd_count += 1                    # Increment odd counter
    return (even_count, odd_count)        # Return tuple with both counts

print(count_even_odd([1, 2, 3, 4, 5, 6]))  # Call with sample list - (3, 3)
# Special: Single pass through list with two counters
```

## List Comprehension Length
```python
def count_even_odd(lst):
    even_count = len([num for num in lst if num % 2 == 0])  # List comp for evens, get length
    odd_count = len([num for num in lst if num % 2 != 0])   # List comp for odds, get length
    return (even_count, odd_count)  # Return tuple with counts

print(count_even_odd([1, 2, 3, 4, 5, 6]))  # Call with sample list - (3, 3)
# Special: Creates intermediate lists to count
```

## Sum of Booleans
```python
def count_even_odd(lst):
    even_count = sum(1 for num in lst if num % 2 == 0)  # Sum ones for even condition
    odd_count = sum(1 for num in lst if num % 2 != 0)   # Sum ones for odd condition
    return (even_count, odd_count)  # Return tuple with counts

print(count_even_odd([1, 2, 3, 4, 5, 6]))  # Call with sample list - (3, 3)
# Special: Generator expressions with sum, memory efficient
```

## Dictionary Count
```python
def count_even_odd(lst):
    count_dict = {'even': 0, 'odd': 0}   # Initialize dictionary for counts
    for num in lst:                      # Iterate through each element
        if num % 2 == 0:                   # Check if number is even
            count_dict['even'] += 1          # Increment even count in dict
        else:                               # Number is odd
            count_dict['odd'] += 1            # Increment odd count in dict
    return (count_dict['even'], count_dict['odd'])  # Return tuple from dict

print(count_even_odd([1, 2, 3, 4, 5, 6]))  # Call with sample list - (3, 3)
# Special: Uses dictionary to store counts by category
```

## Partition with Filter
```python
def count_even_odd(lst):
    evens = list(filter(lambda x: x % 2 == 0, lst))  # Get all even numbers
    odds = list(filter(lambda x: x % 2 != 0, lst))   # Get all odd numbers
    return (len(evens), len(odds))  # Return tuple with lengths

print(count_even_odd([1, 2, 3, 4, 5, 6]))  # Call with sample list - (3, 3)
# Special: Creates separate lists then counts them
```

## While Loop
```python
def count_even_odd(lst):
    even_count = 0                      # Initialize even counter
    odd_count = 0                       # Initialize odd counter
    i = 0                                # Initialize index
    while i < len(lst):                   # Loop until end of list
        if lst[i] % 2 == 0:                # Check if current element is even
            even_count += 1                  # Increment even counter
        else:                               # Current element is odd
            odd_count += 1                    # Increment odd counter
        i += 1                               # Increment index
    return (even_count, odd_count)        # Return tuple with counts

print(count_even_odd([1, 2, 3, 4, 5, 6]))  # Call with sample list - (3, 3)
# Special: Manual index control with while loop
```

## Numpy Where
```python
import numpy as np

def count_even_odd(lst):
    arr = np.array(lst)                  # Convert to numpy array
    even_count = np.sum(arr % 2 == 0)     # Count where remainder is 0 (even)
    odd_count = np.sum(arr % 2 != 0)      # Count where remainder not 0 (odd)
    return (int(even_count), int(odd_count))  # Return tuple with counts as ints

print(count_even_odd([1, 2, 3, 4, 5, 6]))  # Call with sample list - (3, 3)
# Special: Vectorized operations with numpy
```

## Collections Counter with Modulo
```python
from collections import Counter

def count_even_odd(lst):
    parity = [num % 2 for num in lst]     # Create list of remainders (0 for even, 1 for odd)
    counts = Counter(parity)               # Count occurrences of 0 and 1
    return (counts.get(0, 0), counts.get(1, 0))  # Return tuple with even(0) and odd(1) counts

print(count_even_odd([1, 2, 3, 4, 5, 6]))  # Call with sample list - (3, 3)
# Special: Uses Counter on parity values
```

## Recursive Count
```python
def count_even_odd(lst):
    if not lst:                          # Base case: empty list
        return (0, 0)                      # Return zero counts
    even_count, odd_count = count_even_odd(lst[1:])  # Recursive call on rest
    if lst[0] % 2 == 0:                   # Check if first element is even
        return (even_count + 1, odd_count)   # Increment even count
    else:                                  # First element is odd
        return (even_count, odd_count + 1)   # Increment odd count

print(count_even_odd([1, 2, 3, 4, 5, 6]))  # Call with sample list - (3, 3)
# Special: Recursive approach breaking down problem
```