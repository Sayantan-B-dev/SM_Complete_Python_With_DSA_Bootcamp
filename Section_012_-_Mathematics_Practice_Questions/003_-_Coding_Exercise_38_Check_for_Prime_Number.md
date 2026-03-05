## Basic Trial Division
```python
import math

def is_prime(n):
    if n <= 1:                           # Numbers less than 2 are not prime
        return False
    for i in range(2, int(math.sqrt(n)) + 1):  # Check divisors up to sqrt(n)
        if n % i == 0:                     # If divisible, not prime
            return False
    return True                            # No divisors found, prime

print(is_prime(17))  # True
print(is_prime(15))  # False
# Special: Classic algorithm, checks all numbers up to sqrt(n)
```

## Optimized Trial Division
```python
import math

def is_prime(n):
    if n <= 1:
        return False
    if n == 2:                             # 2 is the only even prime
        return True
    if n % 2 == 0:                          # Other evens are composite
        return False
    # Check only odd divisors up to sqrt(n)
    for i in range(3, int(math.sqrt(n)) + 1, 2):
        if n % i == 0:
            return False
    return True

print(is_prime(17))  # True
print(is_prime(15))  # False
# Special: Skips even numbers after handling 2, about twice as fast
```

## While Loop Implementation
```python
import math

def is_prime(n):
    if n <= 1:
        return False
    i = 2
    while i <= int(math.sqrt(n)):           # While loop instead of for
        if n % i == 0:
            return False
        i += 1
    return True

print(is_prime(17))  # True
print(is_prime(15))  # False
# Special: Uses while loop with explicit index variable
```

## All Function with Generator
```python
import math

def is_prime(n):
    if n <= 1:
        return False
    # all() checks that every divisor up to sqrt(n) leaves remainder
    return all(n % i != 0 for i in range(2, int(math.sqrt(n)) + 1))

print(is_prime(17))  # True
print(is_prime(15))  # False
# Special: Functional style using all() and generator expression
```

## Recursive Divisor Check
```python
import math

def is_prime(n, divisor=2):
    if n <= 1:
        return False
    if divisor > int(math.sqrt(n)):         # Base case: checked all divisors
        return True
    if n % divisor == 0:                     # Found divisor, not prime
        return False
    return is_prime(n, divisor + 1)          # Check next divisor

print(is_prime(17))  # True
print(is_prime(15))  # False
# Special: Recursive approach testing divisors sequentially
```

## Using math.isqrt (Python 3.8+)
```python
import math

def is_prime(n):
    if n <= 1:
        return False
    # math.isqrt returns integer floor of square root
    for i in range(2, math.isqrt(n) + 1):
        if n % i == 0:
            return False
    return True

print(is_prime(17))  # True
print(is_prime(15))  # False
# Special: Uses math.isqrt for exact integer sqrt, avoids floating point
```

## Sieve of Eratosthenes (for a single number)
```python
def is_prime(n):
    if n <= 1:
        return False
    # Create a boolean sieve up to n
    sieve = [True] * (n + 1)
    sieve[0] = sieve[1] = False
    for i in range(2, int(n**0.5) + 1):
        if sieve[i]:
            for j in range(i*i, n+1, i):
                sieve[j] = False
    return sieve[n]

print(is_prime(17))  # True
print(is_prime(15))  # False
# Special: Builds full sieve up to n, then checks; overkill for single number but demonstrates concept
```

## Fermat Primality Test (probabilistic)
```python
import random

def is_prime(n, k=5):                      # k is number of test rounds
    if n <= 1:
        return False
    if n <= 3:
        return True
    for _ in range(k):
        a = random.randint(2, n - 2)        # Choose random base
        if pow(a, n-1, n) != 1:              # Fermat's little theorem check
            return False
    return True                              # Probably prime

print(is_prime(17))  # True (almost certainly)
print(is_prime(15))  # False
# Special: Probabilistic test using Fermat's little theorem; may give false positives (Carmichael numbers)
```

## Miller-Rabin Deterministic (for 32-bit integers)
```python
def is_prime(n):
    if n < 2:
        return False
    # Small primes
    small_primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
    if n in small_primes:
        return True
    for p in small_primes:
        if n % p == 0:
            return False
    
    # Write n-1 as d*2^s
    d = n - 1
    s = 0
    while d % 2 == 0:
        d //= 2
        s += 1
    
    # Bases sufficient for all n < 2^32
    bases = [2, 7, 61] if n < 2**32 else [2, 3, 5, 7, 11]
    
    for a in bases:
        if a >= n:
            continue
        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue
        for _ in range(s - 1):
            x = pow(x, 2, n)
            if x == n - 1:
                break
        else:
            return False
    return True

print(is_prime(17))  # True
print(is_prime(15))  # False
# Special: Deterministic Miller-Rabin for 32-bit numbers; fast and accurate
```

## SymPy Library
```python
from sympy import isprime

def is_prime(n):
    return isprime(n)                       # SymPy's built-in prime checker

print(is_prime(17))  # True
print(is_prime(15))  # False
# Special: Uses external library with optimized primality testing
```