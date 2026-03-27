```py

def min_steps_tabulation(n):
    dp = [0] *(n+1)

    dp[1] = 0

    for i in range(2,n+1):

        ans = dp[i-1] + 1

        if(i%2 ==0):
            ans = min(ans,dp[i//2] +1)
        
        if(i%3 ==0):
            ans = min(ans,dp[i//3] + 1)
        
        dp[i] = ans

    return dp[n]


def min_steps_memoization(n,memo):
    if(n==1):
        return 0
    if(memo[n]!=-1):
        return memo[n]
    
    steps = min_steps_memoization(n-1,memo)
    if(n%2==0):
        steps = min(steps,min_steps_memoization(n//2,memo))
    if(n%3==0):
        steps = min(steps,min_steps_memoization(n//3,memo))
    ans = 1 + steps

    memo[n] = ans
    return memo[n]

def min_steps_recursive(n):
    
    if(n==1):
        return 0 # Base Case
    
    steps = min_steps_recursive(n-1)

    if(n%2==0):
        steps = min(steps,min_steps_recursive(n//2))
    
    if(n%3==0):
        steps = min(steps,min_steps_recursive(n//3))

    ans = 1 + steps

    return ans

n=7
memo = [-1] * (n+1)
print(min_steps_memoization(7,memo))

print(min_steps_tabulation(10))


```

# Minimum Steps to 1: Three Approaches

## Problem Restatement

Given a positive integer `n`, the task is to reduce it to `1` using the following allowed operations:

1. Subtract 1.
2. Divide by 2 (if `n` is divisible by 2).
3. Divide by 3 (if `n` is divisible by 3).

Each operation counts as **one step**. The goal is to find the **minimum number of steps** required.

This problem exhibits **overlapping subproblems** – the same smaller numbers are reached via different paths. Three implementations are examined:

- **Naïve Recursion** (exponential time)
- **Memoization** (top‑down DP)
- **Tabulation** (bottom‑up DP)

---

## 1. Naïve Recursive Function

### Algorithm

1. Define a function `min_steps_recursive(n)`.
2. **Base case**: If `n == 1`, return `0` (no steps needed).
3. **Recursive step**:
   - Compute the number of steps if the first operation is subtraction:  
     `steps = min_steps_recursive(n-1)`.
   - If `n` is even, compute the steps for division by 2 and update `steps` to the minimum:  
     `steps = min(steps, min_steps_recursive(n//2))`.
   - If `n` is divisible by 3, compute steps for division by 3 and update `steps` to the minimum:  
     `steps = min(steps, min_steps_recursive(n//3))`.
   - The answer for `n` is `1 + steps` (one operation plus the best of the three possibilities).
4. Return the result.

### Visual Diagram (Recursion Tree for n = 7)

```
                        f(7)
                         |
                      f(6)              (only subtract)
                         |
         +---------------+---------------+
         |               |               |
       f(5)            f(3)            f(2)   (subtract, /2, /3)
         |               |               |
       f(4)            f(2)            f(1)
         |               |               |
       f(3)            f(1)            (0)
         |
       f(2)
         |
       f(1)
```

Each node spawns up to three recursive calls. The tree quickly becomes large due to repeated calls to the same `n`. For `n=7`, the number of calls is already 15 (including base cases). For `n=20`, it grows exponentially.

### Code with Comments

```python
def min_steps_recursive(n):
    # Base case: already at 1, no steps needed
    if n == 1:
        return 0

    # Option 1: subtract 1
    steps = min_steps_recursive(n - 1)

    # Option 2: if divisible by 2, consider dividing by 2
    if n % 2 == 0:
        steps = min(steps, min_steps_recursive(n // 2))

    # Option 3: if divisible by 3, consider dividing by 3
    if n % 3 == 0:
        steps = min(steps, min_steps_recursive(n // 3))

    # One operation is performed, so add 1
    ans = 1 + steps
    return ans
```

### Complexity

- **Time**: Exponential, `O(3^n)` in worst case, because each call may branch into three. In practice, it is slightly better due to base cases but still infeasible for `n > 30`.
- **Space**: `O(n)` due to recursion stack depth (worst case when repeatedly subtracting 1).

### Pros and Cons

| Pros | Cons |
|------|------|
| Simple and directly follows the problem definition. | Extremely inefficient due to massive redundant computations. |
| Easy to understand. | Cannot handle even moderately large `n` (e.g., `n=50`). |

---

## 2. Memoization (Top‑Down DP)

### Algorithm

1. Create a `memo` array of size `n+1`, initialized with a sentinel (e.g., `-1`) to indicate uncomputed states.
2. Define a recursive function `min_steps_memoization(n, memo)`:
   - **Base case**: If `n == 1`, return `0`.
   - If `memo[n] != -1`, return the stored value.
   - Compute `steps` by considering the three possible operations (same as naive recursion) but calling the memoized version.
   - Store `ans = 1 + steps` in `memo[n]`.
   - Return `ans`.
3. The answer for the original `n` is `min_steps_memoization(n, memo)`.

### Visual Diagram (Memoized Calls for n = 7)

```
f(7)
  |
  f(6)                    (first call, compute)
    |
    +-----------------+-----------------+
    |                 |                 |
  f(5)              f(3)              f(2)   (compute f(5), f(3), f(2))
    |                 |                 |
  f(4)              f(2)              f(1)   (f(2) already computed)
    |                 |
  f(3)              f(1)   (f(3) already computed)
    |
  f(2)   (already computed)
```

The key difference: once `f(2)` is computed, subsequent calls to `f(2)` return instantly from the memo. The number of unique states computed is exactly `n-1` (from 2 to n). This drastically reduces the total work.

### Code with Comments

```python
def min_steps_memoization(n, memo):
    # Base case: 1 step needed? Actually 0 steps if already 1
    if n == 1:
        return 0

    # If already computed, return stored result
    if memo[n] != -1:
        return memo[n]

    # Option 1: subtract 1
    steps = min_steps_memoization(n - 1, memo)

    # Option 2: if divisible by 2, check division
    if n % 2 == 0:
        steps = min(steps, min_steps_memoization(n // 2, memo))

    # Option 3: if divisible by 3, check division
    if n % 3 == 0:
        steps = min(steps, min_steps_memoization(n // 3, memo))

    # One operation + best of the three subproblems
    ans = 1 + steps

    # Store result in memo before returning
    memo[n] = ans
    return ans

# Usage
n = 7
memo = [-1] * (n + 1)
print(min_steps_memoization(n, memo))   # Output: 3
```

### Complexity

- **Time**: `O(n)`. Each distinct `n` is computed once; each computation involves a constant number of recursive calls (that hit the memo) and arithmetic.
- **Space**: `O(n)` for the memo array, plus `O(n)` recursion stack depth in the worst case (when repeatedly subtracting 1). However, recursion depth is limited by the input size.

### Pros and Cons

| Pros | Cons |
|------|------|
| Linear time – dramatically faster than naive recursion. | Still uses recursion, may hit Python’s recursion limit for very large `n` (e.g., n=10⁵). |
| Only computes states that are reachable from `n`. | Slightly higher overhead due to function calls. |
| Intuitive for problems with recursive structure. | Requires manual memo management (global or passed). |

---

## 3. Tabulation (Bottom‑Up DP)

### Algorithm

1. Create an array `dp` of length `n+1`. The index `i` will hold the minimum steps for `i`.
2. Initialize `dp[1] = 0` (base case).
3. For `i` from `2` to `n`:
   - Start with the value obtained by subtracting 1: `ans = dp[i-1] + 1`.
   - If `i` is even, compare with `dp[i//2] + 1` and keep the minimum.
   - If `i` is divisible by 3, compare with `dp[i//3] + 1` and keep the minimum.
   - Set `dp[i] = ans`.
4. Return `dp[n]`.

### Visual Diagram (DP Table Filling for n = 7)

We fill the table from left to right:

| i   | dp[i] | Computation                                                  |
|-----|-------|--------------------------------------------------------------|
| 1   | 0     | base                                                         |
| 2   | 1     | min( dp[1]+1, dp[1]+1 ) = min(1,1) = 1                      |
| 3   | 1     | min( dp[2]+1, dp[1]+1 ) = min(2,1) = 1                      |
| 4   | 2     | min( dp[3]+1, dp[2]+1 ) = min(2,2) = 2                      |
| 5   | 3     | min( dp[4]+1 ) = min(3) = 3 (no division)                   |
| 6   | 2     | min( dp[5]+1, dp[3]+1, dp[2]+1 ) = min(4,2,2) = 2          |
| 7   | 3     | min( dp[6]+1 ) = min(3) = 3 (no division)                   |

The table shows the optimal steps for each number.

### Code with Comments

```python
def min_steps_tabulation(n):
    # dp[i] = minimum steps to reduce i to 1
    dp = [0] * (n + 1)   # allocate array, index 0 unused

    # Base case: 1 requires 0 steps
    dp[1] = 0

    # Build the table from 2 up to n
    for i in range(2, n + 1):
        # Option 1: subtract 1
        ans = dp[i - 1] + 1

        # Option 2: if divisible by 2, consider division
        if i % 2 == 0:
            ans = min(ans, dp[i // 2] + 1)

        # Option 3: if divisible by 3, consider division
        if i % 3 == 0:
            ans = min(ans, dp[i // 3] + 1)

        dp[i] = ans

    # Result for original n
    return dp[n]
```

### Complexity

- **Time**: `O(n)`. A single loop of length `n-1` with constant‑time operations per iteration.
- **Space**: `O(n)` for the DP table.

### Pros and Cons

| Pros | Cons |
|------|------|
| No recursion – avoids stack overflow. | Computes all `dp[i]` for `i=2..n`, even if some are not needed for the answer (but in this problem, all are needed anyway because subtraction reaches every integer). |
| Iterative, often faster than memoization due to lower overhead. | May be slightly less intuitive for problems that naturally map to recursion. |
| Easy to optimize further (space can be reduced to O(1) if we only keep necessary values, but here dependencies are not strictly sequential because division jumps, so a full table is required unless we process numbers in order – which we already do). |

---

## Summary of Approaches

| Approach          | Time Complexity | Space Complexity | Pros                                   | Cons                                     |
|-------------------|-----------------|------------------|----------------------------------------|------------------------------------------|
| Naïve Recursion   | O(3ⁿ)           | O(n)             | Simple, direct                         | Exponential, unusable for large n        |
| Memoization       | O(n)            | O(n)             | Linear time, only computes reachable states | Recursion overhead, risk of stack overflow |
| Tabulation        | O(n)            | O(n)             | No recursion, fast iteration           | Computes all states, may waste memory if only few needed |

All three functions correctly compute the minimum steps for the given `n`. The dynamic programming versions (memoization and tabulation) transform an exponential‑time algorithm into a linear‑time one by avoiding redundant calculations.

---

## Example Usage and Output

For `n = 7`:
- Memoization returns `3` (path: 7 → 6 → 2 → 1 or 7 → 6 → 3 → 1).
- For `n = 10`, tabulation returns `3` (path: 10 → 9 → 3 → 1 or 10 → 5 → 4 → 2 → 1? Actually 10→9→3→1 is 3 steps: 10→9 (-1), 9→3 (/3), 3→1 (/3)).

The code provided outputs `3` for both calls, confirming correctness.