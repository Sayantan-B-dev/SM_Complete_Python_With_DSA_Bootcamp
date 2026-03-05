## Euclidean While Loop
```python
def gcd(n, m):
    while m != 0:                      # Continue until remainder becomes zero
        temp = m                         # Store current m
        m = n % m                        # Update m to remainder of n divided by m
        n = temp                          # Update n to old m
    return n                             # Return gcd (last non-zero remainder)

print(gcd(48, 18))  # Call with 48 and 18 - 6
# Special: Classic Euclidean algorithm using a temporary variable
```

## Recursive Euclidean
```python
def gcd(n, m):
    if m == 0:                           # Base case: if m is zero, gcd is n
        return n
    return gcd(m, n % m)                  # Recursive call with swapped arguments

print(gcd(48, 18))  # Call with 48 and 18 - 6
# Special: Elegant recursive version, directly implements Euclid's formula
```

## Built-in math.gcd
```python
import math

def gcd(n, m):
    return math.gcd(n, m)                 # Use Python's built-in gcd function

print(gcd(48, 18))  # Call with 48 and 18 - 6
# Special: Most efficient, implemented in C
```

## Extended Euclidean (gcd only)
```python
def gcd(n, m):
    # Extended Euclidean algorithm returning only gcd
    if m == 0:
        return n
    # Recursive call and then unwind (but we only need gcd)
    return gcd(m, n % m)

# Alternatively, implement iteratively with coefficients, but we simplify.
# Let's do iterative extended that returns just gcd:
def gcd_extended(n, m):
    old_r, r = n, m
    while r != 0:
        quotient = old_r // r
        old_r, r = r, old_r - quotient * r
    return old_r

print(gcd_extended(48, 18))  # Call with 48 and 18 - 6
# Special: Iterative extended Euclidean algorithm, tracks coefficients implicitly
```

## Binary GCD (Stein's Algorithm)
```python
def gcd(n, m):
    # Binary GCD algorithm (Stein's algorithm) - handles zeros and negatives
    if n == 0:
        return abs(m)
    if m == 0:
        return abs(n)
    # Remove common factors of 2
    shift = 0
    while ((n | m) & 1) == 0:           # Both even
        n >>= 1
        m >>= 1
        shift += 1
    # Make n odd
    while (n & 1) == 0:
        n >>= 1
    # Main loop
    while m != 0:
        while (m & 1) == 0:              # Remove factors of 2 from m
            m >>= 1
        if n > m:
            n, m = m, n                    # Ensure n <= m
        m = m - n                         # Subtract smaller from larger
        m >>= 1                            # Remove factor of 2 (since difference is even)
    return n << shift                      # Restore common factor of 2

print(gcd(48, 18))  # Call with 48 and 18 - 6
# Special: Uses bit operations, faster for large numbers with many common factors
```

## Subtraction Method (Euclid's original)
```python
def gcd(n, m):
    n, m = abs(n), abs(m)                 # Work with absolute values
    while n != m:                          # Continue until they become equal
        if n > m:
            n -= m                           # Subtract smaller from larger
        else:
            m -= n
    return n                               # When equal, that's the gcd

print(gcd(48, 18))  # Call with 48 and 18 - 6
# Special: Euclid's original subtraction method, simpler but slower
```

## Recursive Subtraction
```python
def gcd(n, m):
    n, m = abs(n), abs(m)                 # Ensure non-negative
    if n == m:                             # Base case: equal means gcd found
        return n
    if n > m:
        return gcd(n - m, m)                 # Subtract smaller from larger
    return gcd(n, m - n)

print(gcd(48, 18))  # Call with 48 and 18 - 6
# Special: Recursive version of subtraction method
```

## For Loop with Range (Inefficient)
```python
def gcd(n, m):
    # Find the greatest common divisor by checking from min down to 1
    smaller = min(abs(n), abs(m))
    for i in range(smaller, 0, -1):        # Iterate downward from smaller number
        if n % i == 0 and m % i == 0:
            return i
    return 1                               # Fallback (should not reach for positive numbers)

print(gcd(48, 18))  # Call with 48 and 18 - 6
# Special: Brute force, checks every possible divisor (O(min(n,m))), educational but slow
```

## While Loop with Tuple Unpacking
```python
def gcd(n, m):
    while m:                               # While m is non-zero (truthy)
        n, m = m, n % m                      # Simultaneous update using tuple unpacking
    return abs(n)                           # Return gcd (abs for negative handling)

print(gcd(48, 18))  # Call with 48 and 18 - 6
# Special: Pythonic one-liner within loop, no temporary variable
```

## Using Recursion and Modulo (Concise)
```python
def gcd(n, m):
    return n if m == 0 else gcd(m, n % m)   # Ternary conditional for base case

print(gcd(48, 18))  # Call with 48 and 18 - 6
# Special: Ultra-concise recursive version in one line
```