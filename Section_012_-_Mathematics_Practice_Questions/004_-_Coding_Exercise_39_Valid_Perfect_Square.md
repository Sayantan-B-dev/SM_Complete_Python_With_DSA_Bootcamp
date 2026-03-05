## Integer Square Root Squaring
```python
def is_perfect_square(num):
    if num < 0:                          # Negative numbers cannot be perfect squares
        return False
    root = int(num ** 0.5)                # Calculate integer part of square root
    return root * root == num              # Check if squaring gives original number

print(is_perfect_square(16))  # True
print(is_perfect_square(14))  # False
print(is_perfect_square(0))   # True
# Special: Uses integer floor of sqrt and compares square
```

## Math.isqrt (Python 3.8+)
```python
import math

def is_perfect_square(num):
    if num < 0:
        return False
    root = math.isqrt(num)                # Exact integer square root (floor)
    return root * root == num              # Verify by squaring

print(is_perfect_square(16))  # True
print(is_perfect_square(14))  # False
# Special: Uses math.isqrt for exact integer sqrt, avoids floating point
```

## Binary Search for Square Root
```python
def is_perfect_square(num):
    if num < 0:
        return False
    if num < 2:                           # 0 and 1 are perfect squares
        return True
    low, high = 1, num // 2                # Square root is at most num/2 for num>1
    while low <= high:
        mid = (low + high) // 2
        square = mid * mid
        if square == num:
            return True
        elif square < num:
            low = mid + 1
        else:
            high = mid - 1
    return False

print(is_perfect_square(16))  # True
print(is_perfect_square(14))  # False
# Special: Binary search for integer root, O(log n) time
```

## Newton's Method (Integer Version)
```python
def is_perfect_square(num):
    if num < 0:
        return False
    if num < 2:
        return True
    x = num // 2                           # Initial guess
    while True:
        y = (x + num // x) // 2             # Newton's iteration for integer sqrt
        if y >= x:                           # Converged when no improvement
            break
        x = y
    return x * x == num

print(is_perfect_square(16))  # True
print(is_perfect_square(14))  # False
# Special: Uses Newton-Raphson method for integer square root
```

## Brute Force Loop
```python
def is_perfect_square(num):
    if num < 0:
        return False
    for i in range(num + 1):                # Check every integer up to num
        if i * i == num:
            return True
        if i * i > num:                      # Stop early if square exceeds
            break
    return False

print(is_perfect_square(16))  # True
print(is_perfect_square(14))  # False
# Special: Simple but inefficient O(sqrt(n)) loop
```

## Float Square Root with Int Check
```python
def is_perfect_square(num):
    if num < 0:
        return False
    root = num ** 0.5                       # Floating point square root
    return root.is_integer()                 # Check if it's an integer

print(is_perfect_square(16))  # True
print(is_perfect_square(14))  # False
# Special: Original method, but may have precision issues for large numbers
```

## Decimal Module for High Precision
```python
from decimal import Decimal, getcontext

def is_perfect_square(num):
    if num < 0:
        return False
    getcontext().prec = 50                   # Set high precision
    root = Decimal(num).sqrt()                # High-precision square root
    return root == root.to_integral()         # Check if integer

print(is_perfect_square(16))  # True
print(is_perfect_square(14))  # False
# Special: Uses Decimal for exact square root, avoids floating point errors
```

## Modulo Pre-check Optimization
```python
def is_perfect_square(num):
    if num < 0:
        return False
    # Perfect squares modulo 16 are in {0,1,4,9}
    if num % 16 not in (0, 1, 4, 9):
        return False
    # Additional modulo checks can be added (e.g., modulo 7, 9, etc.)
    root = int(num ** 0.5)
    return root * root == num

print(is_perfect_square(16))  # True
print(is_perfect_square(14))  # False (fails modulo check)
print(is_perfect_square(25))  # True (passes modulo check)
# Special: Uses modular arithmetic to quickly eliminate non-squares
```

## While Loop Generating Squares
```python
def is_perfect_square(num):
    if num < 0:
        return False
    i = 0
    while i * i <= num:                     # Generate squares until >= num
        if i * i == num:
            return True
        i += 1
    return False

print(is_perfect_square(16))  # True
print(is_perfect_square(14))  # False
# Special: Generates squares sequentially, similar to brute force but with while
```

## Using Exponentiation and Floor
```python
def is_perfect_square(num):
    if num < 0:
        return False
    root = int(num ** 0.5)                   # Floor of square root
    return root ** 2 == num                   # Check if square equals num

print(is_perfect_square(16))  # True
print(is_perfect_square(14))  # False
# Special: Same as variant 1, but explicitly using exponentiation
```