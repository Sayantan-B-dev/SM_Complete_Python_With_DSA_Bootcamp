## Simple Recursive Fibonacci
### Explanation
This is the most straightforward recursive implementation based on the mathematical definition. It directly translates F(n) = F(n-1) + F(n-2) with base cases n=0 and n=1.

### Approach
- If n is 0, return 0.
- If n is 1, return 1.
- Otherwise, recursively compute F(n-1) and F(n-2) and return their sum.

### Algorithm
1. Check if n == 0: return 0
2. Check if n == 1: return 1
3. Set last = fibonacci(n-1)
4. Set last2 = fibonacci(n-2)
5. Set result = last + last2
6. Return result

### Pseudocode
```
FUNCTION fibonacci(n)
    IF n == 0 THEN
        RETURN 0
    IF n == 1 THEN
        RETURN 1
    last = fibonacci(n-1)
    last2 = fibonacci(n-2)
    result = last + last2
    RETURN result
END FUNCTION
```

### Code
```python
def fibonacci(n):
    # Base case: F(0) = 0
    if n == 0:
        return 0
    # Base case: F(1) = 1
    if n == 1:
        return 1

    # Recursive calls for the two previous terms
    last = fibonacci(n - 1)   # Compute F(n-1)
    last2 = fibonacci(n - 2)  # Compute F(n-2)

    # Sum to get F(n)
    f = last + last2

    return f  # Return the computed Fibonacci number
```

### Example Output
```python
print(fibonacci(6))  # Output: 8
```

---

## Iterative Fibonacci using Loop
### Explanation
Instead of recursion, we use a loop to compute Fibonacci numbers sequentially. This avoids the exponential overhead of recursive calls.

### Approach
- Start with variables a = 0, b = 1.
- For i from 2 to n, compute next = a + b and update a, b.
- Return b (or a depending on starting indices).

### Algorithm
1. If n == 0: return 0
2. If n == 1: return 1
3. Initialize a = 0, b = 1
4. For i from 2 to n:
   - next = a + b
   - a = b
   - b = next
5. Return b

### Pseudocode
```
FUNCTION fibonacci(n)
    IF n == 0 THEN
        RETURN 0
    IF n == 1 THEN
        RETURN 1
    a = 0
    b = 1
    FOR i = 2 TO n DO
        next = a + b
        a = b
        b = next
    END FOR
    RETURN b
END FUNCTION
```

### Code
```python
def fibonacci(n):
    # Handle base cases
    if n == 0:
        return 0
    if n == 1:
        return 1

    # Initialize first two Fibonacci numbers
    a, b = 0, 1

    # Loop from 2 to n inclusive
    for i in range(2, n + 1):
        next_fib = a + b   # Compute next Fibonacci number
        a, b = b, next_fib # Shift variables forward

    return b  # b holds F(n)
```

### Example Output
```python
print(fibonacci(10))  # Output: 55
```

---

## Memoized Recursive Fibonacci
### Explanation
We enhance the recursive version by storing already computed results in a dictionary (memo). This reduces time complexity from exponential to linear.

### Approach
- Use a dictionary to cache results.
- Before computing, check if the result for n is already in the cache; if yes, return it.
- Otherwise, compute recursively and store in cache before returning.

### Algorithm
1. Initialize an empty dictionary memo.
2. Define function fib(n):
   - If n in memo: return memo[n]
   - If n == 0: memo[0] = 0; return 0
   - If n == 1: memo[1] = 1; return 1
   - memo[n] = fib(n-1) + fib(n-2)
   - Return memo[n]

### Pseudocode
```
memo = {}
FUNCTION fibonacci(n)
    IF n IN memo THEN
        RETURN memo[n]
    IF n == 0 THEN
        memo[0] = 0
        RETURN 0
    IF n == 1 THEN
        memo[1] = 1
        RETURN 1
    memo[n] = fibonacci(n-1) + fibonacci(n-2)
    RETURN memo[n]
END FUNCTION
```

### Code
```python
# Dictionary to store computed Fibonacci numbers
memo = {}

def fibonacci(n):
    # Return cached result if available
    if n in memo:
        return memo[n]

    # Base cases
    if n == 0:
        memo[0] = 0
        return 0
    if n == 1:
        memo[1] = 1
        return 1

    # Recursive computation with memoization
    memo[n] = fibonacci(n - 1) + fibonacci(n - 2)  # Store before returning
    return memo[n]  # Return the computed value
```

### Example Output
```python
print(fibonacci(15))  # Output: 610
```

---

## Dynamic Programming with List
### Explanation
We use a bottom-up approach, building a list of Fibonacci numbers from 0 up to n. This is also known as tabulation.

### Approach
- Create a list fib of length n+1.
- Initialize fib[0] = 0, fib[1] = 1.
- For i from 2 to n: fib[i] = fib[i-1] + fib[i-2].
- Return fib[n].

### Algorithm
1. If n == 0: return 0
2. Create list fib of size n+1
3. fib[0] = 0; fib[1] = 1
4. For i = 2 to n:
   - fib[i] = fib[i-1] + fib[i-2]
5. Return fib[n]

### Pseudocode
```
FUNCTION fibonacci(n)
    IF n == 0 THEN
        RETURN 0
    fib = ARRAY[0..n]
    fib[0] = 0
    fib[1] = 1
    FOR i = 2 TO n DO
        fib[i] = fib[i-1] + fib[i-2]
    END FOR
    RETURN fib[n]
END FUNCTION
```

### Code
```python
def fibonacci(n):
    # Handle the special case n=0
    if n == 0:
        return 0

    # Create a list to store Fibonacci numbers up to n
    fib = [0] * (n + 1)
    fib[0] = 0  # F(0)
    fib[1] = 1  # F(1)

    # Fill the list iteratively
    for i in range(2, n + 1):
        fib[i] = fib[i - 1] + fib[i - 2]  # Main recurrence relation

    return fib[n]  # Return the desired Fibonacci number
```

### Example Output
```python
print(fibonacci(12))  # Output: 144
```

---

## Fibonacci using Generator
### Explanation
A generator function yields Fibonacci numbers one by one, allowing lazy evaluation and memory efficiency when iterating over a sequence.

### Approach
- Initialize a = 0, b = 1.
- In an infinite loop, yield a, then update a, b = b, a+b.
- To get the nth number, we can iterate the generator n+1 times.

### Algorithm
1. Generator function:
   - a = 0, b = 1
   - While True:
       - yield a
       - a, b = b, a + b
2. To get nth Fibonacci:
   - Create generator instance
   - Loop n+1 times to get the nth value.

### Pseudocode
```
GENERATOR fibonacci_gen()
    a = 0
    b = 1
    WHILE TRUE DO
        YIELD a
        a = b
        b = a + b  # careful: after assignment, a has old b
    END WHILE
END GENERATOR

FUNCTION fibonacci(n)
    gen = fibonacci_gen()
    FOR i = 0 TO n DO
        result = NEXT(gen)
    END FOR
    RETURN result
END FUNCTION
```

### Code
```python
def fibonacci_gen():
    """Generator that yields Fibonacci numbers indefinitely."""
    a, b = 0, 1
    while True:
        yield a          # Yield the current Fibonacci number
        a, b = b, a + b  # Update to next pair

def fibonacci(n):
    # Get the nth Fibonacci number using the generator
    gen = fibonacci_gen()
    for i in range(n + 1):
        result = next(gen)  # Advance the generator until we reach the nth number
    return result  # Return the nth Fibonacci number
```

### Example Output
```python
print(fibonacci(9))  # Output: 34
```

---

## Fibonacci using functools.lru_cache
### Explanation
Python's built-in `lru_cache` decorator automatically memoizes function results, making the simple recursive version efficient with minimal code changes.

### Approach
- Import lru_cache from functools.
- Decorate the recursive function with @lru_cache(maxsize=None).
- The decorator handles caching of return values.

### Algorithm
1. Import lru_cache
2. Define fibonacci(n) with @lru_cache
3. Inside: if n < 2: return n; else return fibonacci(n-1) + fibonacci(n-2)

### Pseudocode
```
IMPORT lru_cache FROM functools

@lru_cache(maxsize=None)
FUNCTION fibonacci(n)
    IF n < 2 THEN
        RETURN n
    ELSE
        RETURN fibonacci(n-1) + fibonacci(n-2)
    END IF
END FUNCTION
```

### Code
```python
from functools import lru_cache

@lru_cache(maxsize=None)  # Cache all results, no limit
def fibonacci(n):
    # Base cases: F(0)=0, F(1)=1
    if n < 2:
        return n

    # Recursive calls with automatic memoization
    return fibonacci(n - 1) + fibonacci(n - 2)  # Main logic
```

### Example Output
```python
print(fibonacci(20))  # Output: 6765
```

---

## Fibonacci using Matrix Exponentiation
### Explanation
Fibonacci numbers can be computed using matrix exponentiation in O(log n) time. This method leverages the matrix identity: [F(n+1) F(n); F(n) F(n-1)] = [1 1; 1 0]^n.

### Approach
- Define matrix multiplication function for 2x2 matrices.
- Compute the matrix power of [[1,1],[1,0]] raised to n using exponentiation by squaring.
- Extract F(n) from the resulting matrix.

### Algorithm
1. Define multiply(A, B) returns product of 2x2 matrices.
2. Define power(matrix, n):
   - result = identity matrix [[1,0],[0,1]]
   - while n > 0:
       - if n odd: result = multiply(result, matrix)
       - matrix = multiply(matrix, matrix)
       - n //= 2
3. base = [[1,1],[1,0]]
4. mat = power(base, n)
5. Return mat[0][1] (or mat[1][0]) which equals F(n).

### Pseudocode
```
FUNCTION multiply(A, B)
    return [
        [A[0][0]*B[0][0] + A[0][1]*B[1][0], A[0][0]*B[0][1] + A[0][1]*B[1][1]],
        [A[1][0]*B[0][0] + A[1][1]*B[1][0], A[1][0]*B[0][1] + A[1][1]*B[1][1]]
    ]

FUNCTION power(mat, n)
    result = [[1,0],[0,1]]
    WHILE n > 0 DO
        IF n % 2 == 1 THEN
            result = multiply(result, mat)
        END IF
        mat = multiply(mat, mat)
        n = n // 2
    END WHILE
    RETURN result

FUNCTION fibonacci(n)
    IF n == 0 THEN RETURN 0
    base = [[1,1],[1,0]]
    mat = power(base, n-1)
    RETURN mat[0][0]
END FUNCTION
```

### Code
```python
def multiply(A, B):
    """Multiply two 2x2 matrices."""
    return [
        [A[0][0]*B[0][0] + A[0][1]*B[1][0], A[0][0]*B[0][1] + A[0][1]*B[1][1]],
        [A[1][0]*B[0][0] + A[1][1]*B[1][0], A[1][0]*B[0][1] + A[1][1]*B[1][1]]
    ]

def matrix_power(mat, n):
    """Raise matrix to power n using binary exponentiation."""
    result = [[1, 0], [0, 1]]  # Identity matrix
    while n > 0:
        if n % 2 == 1:                     # If current bit is 1, multiply result
            result = multiply(result, mat)
        mat = multiply(mat, mat)            # Square the matrix
        n //= 2                             # Move to next bit
    return result

def fibonacci(n):
    # Base case for n=0
    if n == 0:
        return 0
    base = [[1, 1], [1, 0]]
    # Compute base^(n-1) because base^(n-1) contains F(n) at [0][0]
    result_matrix = matrix_power(base, n - 1)
    return result_matrix[0][0]  # This is F(n)
```

### Example Output
```python
print(fibonacci(16))  # Output: 987
```

---

## Fibonacci using Binet's Formula
### Explanation
Binet's formula gives a closed-form expression: F(n) = (phi^n - psi^n) / sqrt(5), where phi = (1+√5)/2 and psi = (1-√5)/2. This computes in O(1) time but suffers from floating-point precision issues for large n.

### Approach
- Compute phi = (1 + sqrt(5)) / 2.
- Compute psi = (1 - sqrt(5)) / 2.
- Return round((phi**n - psi**n) / sqrt(5)).

### Algorithm
1. Import math
2. sqrt5 = math.sqrt(5)
3. phi = (1 + sqrt5) / 2
4. psi = (1 - sqrt5) / 2
5. result = (phi**n - psi**n) / sqrt5
6. Return round(result)

### Pseudocode
```
IMPORT math

FUNCTION fibonacci(n)
    sqrt5 = math.sqrt(5)
    phi = (1 + sqrt5) / 2
    psi = (1 - sqrt5) / 2
    fib = (phi**n - psi**n) / sqrt5
    RETURN round(fib)
END FUNCTION
```

### Code
```python
import math

def fibonacci(n):
    # Binet's formula using floating-point arithmetic
    sqrt5 = math.sqrt(5)                        # Square root of 5
    phi = (1 + sqrt5) / 2                        # Golden ratio
    psi = (1 - sqrt5) / 2                        # Conjugate of phi

    # Compute using the formula
    fib = (phi**n - psi**n) / sqrt5              # Main computation
    return round(fib)                             # Round to nearest integer
```

### Example Output
```python
print(fibonacci(8))  # Output: 21
```

---

## Tail Recursive Fibonacci
### Explanation
Tail recursion is a form of recursion where the recursive call is the last operation in the function. Python doesn't optimize tail recursion, but it's a useful concept. We use an accumulator to pass intermediate results.

### Approach
- Define a helper function that takes n, a, b where a = F(n-2), b = F(n-1).
- If n == 0, return a; if n == 1, return b.
- Otherwise, call helper(n-1, b, a+b).

### Algorithm
1. Define fib_helper(n, a, b):
   - if n == 0: return a
   - if n == 1: return b
   - return fib_helper(n-1, b, a+b)
2. fibonacci(n) calls fib_helper(n, 0, 1)

### Pseudocode
```
FUNCTION fib_helper(n, a, b)
    IF n == 0 THEN
        RETURN a
    IF n == 1 THEN
        RETURN b
    RETURN fib_helper(n-1, b, a+b)
END FUNCTION

FUNCTION fibonacci(n)
    RETURN fib_helper(n, 0, 1)
END FUNCTION
```

### Code
```python
def fib_helper(n, a, b):
    # Tail-recursive helper with accumulators a and b
    if n == 0:
        return a          # F(0) = a
    if n == 1:
        return b          # F(1) = b
    # Recursive call with updated values, now the last operation
    return fib_helper(n - 1, b, a + b)  # Shift window forward

def fibonacci(n):
    # Initial call: a = F(0) = 0, b = F(1) = 1
    return fib_helper(n, 0, 1)
```

### Example Output
```python
print(fibonacci(13))  # Output: 233
```

---

## Fibonacci using Reduce (Functional)
### Explanation
We can use `functools.reduce` to simulate the iterative process in a functional style. Reduce applies a function cumulatively to items of a sequence.

### Approach
- Create a list of n ones (or a range of length n) to serve as steps.
- Use reduce with a lambda that takes a tuple (a, b) and returns (b, a+b).
- Start with initial tuple (0, 1). After n reductions, the first element of the tuple is F(n).

### Algorithm
1. Import reduce from functools.
2. Define an initial tuple (0, 1).
3. Create a sequence of length n (e.g., range(n)).
4. Use reduce: for each step, transform (a, b) -> (b, a+b).
5. After n steps, take the first element of the tuple.

### Pseudocode
```
IMPORT reduce FROM functools

FUNCTION fibonacci(n)
    initial = (0, 1)
    sequence = range(n)  # n steps
    final = reduce(lambda pair, _: (pair[1], pair[0] + pair[1]), sequence, initial)
    RETURN final[0]
END FUNCTION
```

### Code
```python
from functools import reduce

def fibonacci(n):
    # Starting pair: (F(0), F(1))
    initial = (0, 1)

    # Create a range of length n to iterate n times
    steps = range(n)

    # Reduce: each step updates (a, b) -> (b, a+b)
    # The underscore ignores the step value (since we just need to repeat)
    final_pair = reduce(lambda pair, _: (pair[1], pair[0] + pair[1]), steps, initial)

    # After n reductions, the first element is F(n)
    return final_pair[0]
```

### Example Output
```python
print(fibonacci(7))  # Output: 13
```