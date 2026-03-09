# Factorial Function: Concepts and Variants

## Explanation

The factorial of a non‑negative integer `n`, denoted `n!`, is the product of all positive integers less than or equal to `n`.  
Mathematically:

- `0! = 1` (by convention)  
- `n! = n × (n-1) × (n-2) × ... × 1` for `n ≥ 1`

The factorial function grows rapidly and is fundamental in combinatorics, probability, and algorithm analysis.  
It exhibits a natural recursive structure: `n! = n × (n-1)!`, with the base case `0! = 1`.

## Approach

Several implementation strategies exist:

- **Recursive**: Directly translates the mathematical recurrence. Simple but limited by stack depth.  
- **Iterative**: Uses a loop to accumulate the product. Avoids recursion overhead and stack limits.  
- **Functional**: Employs higher‑order functions like `reduce` to express the product declaratively.  
- **Built‑in**: Leverages Python’s standard library (`math.factorial`) for production use.  
- **Memoized**: Caches results of recursive calls to avoid redundant computation (useful for repeated calls).  
- **Tail‑recursive**: Attempts to use an accumulator to enable potential tail‑call optimization (not supported in Python, but demonstrates the concept).  
- **Dynamic Programming**: Builds a table of factorial values bottom‑up.

## Algorithm (Standard Recursive)

1. If `n == 0`, return `1`.  
2. Otherwise, return `n × factorial(n-1)`.

## Pseudocode

```
FUNCTION factorial(n)
    IF n == 0 THEN
        RETURN 1
    ELSE
        RETURN n * factorial(n - 1)
    END IF
END FUNCTION
```

---

## 1. Basic Recursive Factorial

```python
def factorial(n: int) -> int:
    """
    Recursive implementation of factorial.
    Base case: n == 0 -> return 1
    Recursive case: n * factorial(n-1)
    """
    # Base case: smallest input, recursion terminates here
    if n == 0:
        return 1
    # Recursive call with reduced argument; current call waits for result
    return n * factorial(n - 1)

# Example call and output
print(factorial(5))  # 120
```

**Output:**  
```
120
```

---

## 2. Iterative Factorial Using For Loop

```python
def factorial_iterative(n: int) -> int:
    """
    Iterative factorial using a for loop.
    Accumulates product from 1 to n.
    """
    result = 1                     # Initialize accumulator (0! = 1)
    for i in range(1, n + 1):      # i takes values 1, 2, ..., n
        result *= i                 # Multiply each integer into result
    return result

print(factorial_iterative(5))       # 120
```

**Output:**  
```
120
```

---

## 3. Functional Factorial Using `functools.reduce`

```python
from functools import reduce
import operator

def factorial_reduce(n: int) -> int:
    """
    Factorial using reduce and operator.mul.
    reduce applies multiplication cumulatively to the sequence 1..n.
    """
    if n == 0:
        return 1
    # reduce(lambda x, y: x * y, range(1, n+1)) is equivalent
    return reduce(operator.mul, range(1, n + 1))

print(factorial_reduce(5))          # 120
```

**Output:**  
```
120
```

---

## 4. Built-in Factorial Using `math.factorial`

```python
import math

def factorial_builtin(n: int) -> int:
    """
    Delegates to the highly optimized math.factorial.
    Handles edge cases and raises ValueError for negative inputs.
    """
    return math.factorial(n)

print(factorial_builtin(5))         # 120
```

**Output:**  
```
120
```

---

## 5. Memoized Recursive Factorial (with LRU Cache)

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def factorial_memoized(n: int) -> int:
    """
    Recursive factorial with memoization.
    Results of previous calls are cached, avoiding recomputation.
    """
    if n == 0:
        return 1
    return n * factorial_memoized(n - 1)

print(factorial_memoized(5))        # 120
print(factorial_memoized(7))        # 5040 (reuses cached values for 5 and 6)
```

**Output:**  
```
120
5040
```

---

## 6. Tail Recursive Factorial (with Accumulator)

```python
def factorial_tail(n: int, accumulator: int = 1) -> int:
    """
    Tail‑recursive factorial using an accumulator.
    Python does not optimize tail calls, but the pattern is shown for completeness.
    """
    if n == 0:
        return accumulator           # Base case returns accumulated product
    # Recursive call with n-1 and updated accumulator
    return factorial_tail(n - 1, accumulator * n)

print(factorial_tail(5))             # 120
```

**Output:**  
```
120
```

---

## 7. One-Liner Recursive Factorial Using Ternary Operator

```python
def factorial_oneliner(n: int) -> int:
    """
    Recursive factorial expressed as a single line using the ternary operator.
    """
    return 1 if n == 0 else n * factorial_oneliner(n - 1)

print(factorial_oneliner(5))         # 120
```

**Output:**  
```
120
```

---

## 8. While Loop Iterative Factorial

```python
def factorial_while(n: int) -> int:
    """
    Iterative factorial using a while loop.
    Decrements n while accumulating the product.
    """
    result = 1
    while n > 0:
        result *= n                  # Multiply current n into result
        n -= 1                        # Move to next lower integer
    return result

print(factorial_while(5))            # 120
```

**Output:**  
```
120
```

---

## 9. Recursive Factorial with Input Validation

```python
def factorial_validated(n: int) -> int:
    """
    Recursive factorial with explicit handling of invalid inputs.
    Raises ValueError for negative numbers or non‑integer types.
    """
    if not isinstance(n, int):
        raise TypeError("n must be an integer")
    if n < 0:
        raise ValueError("n must be non‑negative")
    if n == 0:
        return 1
    return n * factorial_validated(n - 1)

print(factorial_validated(5))        # 120
# print(factorial_validated(-1))     # Would raise ValueError
```

**Output:**  
```
120
```

---

## 10. Dynamic Programming Factorial (Bottom-Up)

```python
def factorial_dp(n: int) -> int:
    """
    Bottom‑up dynamic programming: builds a list of factorials from 0 to n.
    """
    if n < 0:
        raise ValueError("n must be non‑negative")
    # Table to store factorial values
    fact = [1] * (n + 1)             # fact[0] = 1 already
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i    # recurrence relation
    return fact[n]

print(factorial_dp(5))               # 120
```

**Output:**  
```
120
```