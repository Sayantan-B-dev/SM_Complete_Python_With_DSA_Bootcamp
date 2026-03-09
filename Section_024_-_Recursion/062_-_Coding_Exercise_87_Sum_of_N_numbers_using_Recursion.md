## Iterative Approach using For Loop

### Explanation
This variant uses a simple `for` loop to iterate from 1 to n, accumulating the sum in a variable. It's straightforward and avoids recursion overhead, making it efficient for large n.

### Approach
- Initialize a variable `total` to 0.
- Loop through numbers 1 to n (inclusive).
- Add each number to `total`.
- Return `total`.

### Algorithm
1. Start with `total = 0`.
2. For `i` from 1 to `n`:
   - `total += i`
3. Return `total`.

### Pseudocode
```
function sum_of_natural_numbers(n):
    total = 0
    for i = 1 to n:
        total = total + i
    return total
```

### Code
```python
def sum_of_natural_numbers(n):
    total = 0                     # Initialize accumulator
    for i in range(1, n + 1):     # Iterate from 1 to n
        total += i                 # Add current number to total (main logic)
    return total                   # Return final sum

# Example call and output
print(sum_of_natural_numbers(5))   # Output: 15
```

---

## Mathematical Formula Approach

### Explanation
The sum of the first n natural numbers can be computed directly using the formula `n * (n + 1) // 2`. This is the most efficient method with O(1) time complexity.

### Approach
- Validate that n is non-negative (natural numbers).
- Compute using integer arithmetic to avoid floating-point issues.

### Algorithm
1. If `n < 0`, handle error (optional).
2. Return `n * (n + 1) // 2`.

### Pseudocode
```
function sum_of_natural_numbers(n):
    return n * (n + 1) // 2
```

### Code
```python
def sum_of_natural_numbers(n):
    # Direct formula: n*(n+1)/2, integer division ensures integer result
    return n * (n + 1) // 2        # Main logic: mathematical closed form

# Example call and output
print(sum_of_natural_numbers(5))   # Output: 15
```

---

## Built-in sum with range

### Explanation
Python's built-in `sum()` function can be combined with `range()` to compute the sum elegantly. This leverages Python's high-level abstractions.

### Approach
- Generate an iterable of numbers from 1 to n using `range(1, n+1)`.
- Pass it to `sum()` to compute the total.

### Algorithm
1. Create `range(1, n+1)`.
2. Pass to `sum()`.
3. Return the result.

### Pseudocode
```
function sum_of_natural_numbers(n):
    return sum(range(1, n+1))
```

### Code
```python
def sum_of_natural_numbers(n):
    # range generates numbers 1..n, sum adds them up (main logic)
    return sum(range(1, n + 1))    # Built-in sum over range

# Example call and output
print(sum_of_natural_numbers(5))   # Output: 15
```

---

## Using functools.reduce

### Explanation
`functools.reduce` applies a binary function cumulatively to the items of an iterable. Here we use it to accumulate the sum of numbers from 1 to n.

### Approach
- Import `reduce` from `functools`.
- Use a lambda that takes two arguments and returns their sum.
- Apply `reduce` over the range.

### Algorithm
1. Create an iterable of numbers 1..n.
2. Use `reduce(lambda x, y: x + y, iterable)`.
3. Return result.

### Pseudocode
```
import functools
function sum_of_natural_numbers(n):
    return functools.reduce(lambda a, b: a + b, range(1, n+1))
```

### Code
```python
from functools import reduce

def sum_of_natural_numbers(n):
    # reduce applies the lambda cumulatively: (((1+2)+3)+...)
    return reduce(lambda acc, val: acc + val, range(1, n + 1))  # Main logic: functional reduction

# Example call and output
print(sum_of_natural_numbers(5))   # Output: 15
```

---

## Tail Recursion with Accumulator

### Explanation
A recursive function is tail-recursive if the recursive call is the last operation. Here we use an accumulator parameter to carry the partial sum, allowing potential optimization (though Python does not optimize tail recursion).

### Approach
- Define an inner function or use a default parameter.
- Pass the current accumulated sum as an argument.
- Base case: when n == 0, return accumulator.
- Recursive case: call with n-1 and accumulator + n.

### Algorithm
1. If n == 0, return accumulator.
2. Else, return sum_of_natural_numbers(n-1, accumulator + n).

### Pseudocode
```
function sum_of_natural_numbers(n, acc=0):
    if n == 0:
        return acc
    else:
        return sum_of_natural_numbers(n-1, acc + n)
```

### Code
```python
def sum_of_natural_numbers(n, accumulator=0):   # accumulator carries partial sum
    if n == 0:                                   # Base case: no more numbers
        return accumulator                       # Return accumulated result
    # Recursive call with updated accumulator (tail recursion)
    return sum_of_natural_numbers(n - 1, accumulator + n)  # Main logic: tail call

# Example call and output
print(sum_of_natural_numbers(5))   # Output: 15
```

---

## Generator Expression with sum

### Explanation
A generator expression produces values on the fly without creating a list. Combined with `sum()`, it's memory-efficient for large n.

### Approach
- Create a generator expression `(i for i in range(1, n+1))`.
- Pass it to `sum()`.

### Algorithm
1. Generate values from 1 to n using a generator.
2. Sum them with `sum()`.

### Pseudocode
```
function sum_of_natural_numbers(n):
    return sum(i for i in range(1, n+1))
```

### Code
```python
def sum_of_natural_numbers(n):
    # Generator expression yields each i one by one, sum consumes it
    return sum(i for i in range(1, n + 1))   # Main logic: generator + sum

# Example call and output
print(sum_of_natural_numbers(5))   # Output: 15
```

---

## While Loop Approach

### Explanation
This variant uses a `while` loop to decrement n and accumulate the sum. It's similar to the iterative for loop but uses a different control structure.

### Approach
- Initialize `total` to 0.
- While n > 0, add n to total and decrement n.
- Return total.

### Algorithm
1. Set `total = 0`.
2. While `n > 0`:
   - `total += n`
   - `n -= 1`
3. Return `total`.

### Pseudocode
```
function sum_of_natural_numbers(n):
    total = 0
    while n > 0:
        total = total + n
        n = n - 1
    return total
```

### Code
```python
def sum_of_natural_numbers(n):
    total = 0                     # Initialize accumulator
    while n > 0:                   # Loop until n becomes 0
        total += n                  # Add current n to total (main logic)
        n -= 1                       # Decrement n
    return total                   # Return final sum

# Example call and output
print(sum_of_natural_numbers(5))   # Output: 15
```

---

## Recursion with Ternary Operator (One-liner)

### Explanation
This is a compact recursive version using Python's ternary conditional expression. It's concise but may be less readable.

### Approach
- Use a conditional expression: `n + sum_of_natural_numbers(n-1) if n > 0 else 0`.
- Base case returns 0 when n == 0.

### Algorithm
1. If n == 0, return 0.
2. Else, return n + sum_of_natural_numbers(n-1).

### Pseudocode
```
function sum_of_natural_numbers(n):
    return n + sum_of_natural_numbers(n-1) if n > 0 else 0
```

### Code
```python
def sum_of_natural_numbers(n):
    # Ternary operator: if n > 0, do recursive addition, else base case 0
    return n + sum_of_natural_numbers(n - 1) if n > 0 else 0   # Main logic: one-liner recursion

# Example call and output
print(sum_of_natural_numbers(5))   # Output: 15
```

---

## Using Lambda and Recursion (Self-referential)

### Explanation
A lambda function can be assigned to a variable and then used recursively. This demonstrates that functions are first-class objects in Python.

### Approach
- Assign a lambda to `sum_func`.
- The lambda takes n and calls itself via the variable name.
- Base case handled with conditional.

### Algorithm
1. Define `sum_func = lambda n: n + sum_func(n-1) if n > 0 else 0`.
2. Call `sum_func(n)`.

### Pseudocode
```
sum_func = lambda n: n + sum_func(n-1) if n > 0 else 0
function sum_of_natural_numbers(n):
    return sum_func(n)
```

### Code
```python
# Define a recursive lambda (requires assignment to refer to itself)
sum_func = lambda n: n + sum_func(n - 1) if n > 0 else 0   # Main logic: lambda recursion

def sum_of_natural_numbers(n):
    return sum_func(n)               # Call the lambda

# Example call and output
print(sum_of_natural_numbers(5))   # Output: 15
```

---

## Using List Comprehension with sum

### Explanation
This builds a list of numbers from 1 to n using list comprehension and then sums them. It's less memory-efficient than generators but clear.

### Approach
- Create a list `[i for i in range(1, n+1)]`.
- Pass it to `sum()`.

### Algorithm
1. Generate list of numbers 1..n.
2. Sum the list.

### Pseudocode
```
function sum_of_natural_numbers(n):
    return sum([i for i in range(1, n+1)])
```

### Code
```python
def sum_of_natural_numbers(n):
    # List comprehension creates a list of all numbers, then sum adds them
    return sum([i for i in range(1, n + 1)])   # Main logic: list comprehension + sum

# Example call and output
print(sum_of_natural_numbers(5))   # Output: 15
```