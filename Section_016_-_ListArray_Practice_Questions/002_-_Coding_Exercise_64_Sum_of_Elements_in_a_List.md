## Sum of Elements: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Summing the elements of a list is a fundamental aggregation operation. It involves iterating through all elements and accumulating their total. The result is a single numeric value representing the total sum.

**Approach:**  
- Initialize an accumulator variable to zero.  
- Traverse each element in the list, adding its value to the accumulator.  
- After processing all elements, return the accumulator.

**Algorithm:**  
1. Set `total = 0`.  
2. For each `element` in the list:  
   - Add `element` to `total`.  
3. Return `total`.

**Pseudocode:**  
```
function sumList(list):
    total = 0
    for each element in list:
        total = total + element
    return total
```

---

## Basic Iterative Sum (As Given)

```python
def sum_iterative(lst):
    s = 0                         # initialize accumulator
    for e in lst:                  # main logic: iterate through each element
        s += e                      # add current element to total
    return s                        # return final sum

# Example call
numbers = [1, 2, 3, 4, 5]
result = sum_iterative(numbers)
print("Sum:", result)   # Output: Sum: 15
```

---

## Using Python's Built-in `sum()`

```python
def sum_builtin(lst):
    # Built-in sum function – most Pythonic and efficient
    return sum(lst)                  # direct call to sum()

# Example call
numbers = [1, 2, 3, 4, 5]
result = sum_builtin(numbers)
print("Sum:", result)   # Output: Sum: 15
```

---

## Recursive Sum

```python
def sum_recursive(lst):
    # Base case: empty list sums to 0
    if not lst:
        return 0
    # Recursive case: first element + sum of rest
    return lst[0] + sum_recursive(lst[1:])   # main logic: head + tail sum

# Example call
numbers = [1, 2, 3, 4, 5]
result = sum_recursive(numbers)
print("Sum:", result)   # Output: Sum: 15
```

---

## Using a `while` Loop

```python
def sum_while(lst):
    total = 0
    i = 0
    while i < len(lst):               # main logic: loop with index
        total += lst[i]                 # add current element
        i += 1                          # move to next index
    return total

# Example call
numbers = [1, 2, 3, 4, 5]
result = sum_while(numbers)
print("Sum:", result)   # Output: Sum: 15
```

---

## Using `functools.reduce()`

```python
from functools import reduce

def sum_reduce(lst):
    # reduce applies a two-argument function cumulatively
    # lambda adds two numbers
    return reduce(lambda x, y: x + y, lst, 0)   # start with 0 for empty list safety

# Example call
numbers = [1, 2, 3, 4, 5]
result = sum_reduce(numbers)
print("Sum:", result)   # Output: Sum: 15
```

---

## Sum of Squares

```python
def sum_of_squares(lst):
    total = 0
    for e in lst:
        total += e * e                   # main logic: add square of each element
    return total

# Example call
numbers = [1, 2, 3, 4, 5]
result = sum_of_squares(numbers)
print("Sum of squares:", result)   # Output: Sum of squares: 55
```

---

## Conditional Sum (Sum Only Even Numbers)

```python
def sum_even(lst):
    total = 0
    for e in lst:
        if e % 2 == 0:                   # condition: only add even numbers
            total += e
    return total

# Example call
numbers = [1, 2, 3, 4, 5, 6]
result = sum_even(numbers)
print("Sum of evens:", result)   # Output: Sum of evens: 12
```

---

## Sum of Nested List (Flattened)

```python
def sum_nested(lst):
    total = 0
    for element in lst:
        if isinstance(element, list):     # if element is a list, recursively sum it
            total += sum_nested(element)
        else:
            total += element               # base case: add number
    return total

# Example call
nested = [1, [2, 3], [4, [5, 6]], 7]
result = sum_nested(nested)
print("Sum of nested list:", result)   # Output: Sum of nested list: 28
```

---

## Sum Using `enumerate()` (Demonstrative)

```python
def sum_enumerate(lst):
    total = 0
    for i, value in enumerate(lst):      # i is index, not used here but available
        total += value                     # main logic: add value
    return total

# Example call
numbers = [1, 2, 3, 4, 5]
result = sum_enumerate(numbers)
print("Sum:", result)   # Output: Sum: 15
```

---

## Sum with Initial Value (Custom Start)

```python
def sum_with_start(lst, start=0):
    total = start                         # start from custom initial value
    for e in lst:
        total += e
    return total

# Example call
numbers = [1, 2, 3, 4, 5]
result = sum_with_start(numbers, 10)      # start from 10
print("Sum with start 10:", result)   # Output: Sum with start 10: 25
```

---

## Sum Using Generator Expression

```python
def sum_generator(lst):
    # Generator expression yields each element lazily
    return sum(e for e in lst)             # built-in sum with generator

# Example call
numbers = [1, 2, 3, 4, 5]
result = sum_generator(numbers)
print("Sum:", result)   # Output: Sum: 15
```