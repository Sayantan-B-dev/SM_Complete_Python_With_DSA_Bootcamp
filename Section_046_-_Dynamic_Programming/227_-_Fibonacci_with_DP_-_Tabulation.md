# Dynamic Programming Approaches for Fibonacci: Top-Down vs. Bottom-Up

## Introduction

The Fibonacci sequence is a classic problem used to demonstrate the two primary strategies of dynamic programming:

1. **Top-Down Approach (Memoization)** – Recursive solution with caching.
2. **Bottom-Up Approach (Tabulation)** – Iterative solution building from base cases.

Both techniques eliminate redundant calculations, reducing the time complexity from exponential to linear. However, they differ in implementation, memory usage, and practical considerations.

Below, each approach is examined in detail.

---

## 1. Top-Down Approach (Memoization)

### Definition
Top-down DP starts with the original problem and recursively breaks it into smaller subproblems. Results of subproblems are stored (memoized) so that each subproblem is solved only once. This is also known as **memoization**.

### Algorithm Description
- Use a recursive function that takes `n` as input.
- Base cases: `fib(0) = 1`, `fib(1) = 1` (as per the given definition).
- Before computing `fib(n)`, check if the value is already stored in a memo array/dictionary.
- If stored, return it immediately.
- If not, recursively compute `fib(n-1)` and `fib(n-2)`, store the result in the memo, then return.

### Code Example
```python
# Top-down with memoization using a list (diary)
diary = [-1] * (50+1)   # global memo, size based on expected max n

def fib_top_down(n):
    if n <= 1:
        return 1
    if diary[n] != -1:
        return diary[n]
    result = fib_top_down(n-1) + fib_top_down(n-2)
    diary[n] = result
    return result
```

### Complexity
- **Time**: O(n) – each distinct `n` is computed once.
- **Space**: O(n) – recursion stack depth of `n` plus the memo array of size `n+1`.

### Advantages
- Intuitive for problems with a recursive definition.
- Solves only necessary subproblems; if the problem requires only a subset of all possible subproblems, memoization can be more efficient in practice.
- Easier to implement for some problems where the state space is sparse.

### Disadvantages
- Risk of recursion depth limit for very large `n` (e.g., n=1000 may cause RecursionError in Python).
- Slightly higher constant overhead due to function calls.
- Global or passed memo can be less clean for encapsulation.

---

## 2. Bottom-Up Approach (Tabulation)

### Definition
Bottom-up DP starts with the smallest subproblems (base cases) and iteratively builds up to the desired solution. It fills a table (array) in a systematic order, typically from 0 to n.

### Algorithm Description
- Create an array `dp` of size `n+1`.
- Initialize base values: `dp[0] = 1`, `dp[1] = 1`.
- For each `i` from 2 to `n`, compute `dp[i] = dp[i-1] + dp[i-2]`.
- Return `dp[n]`.

### Code Example
```python
def fib_bottom_up(n):
    if n <= 1:
        return 1
    dp = [0] * (n+1)
    dp[0] = 1
    dp[1] = 1
    for i in range(2, n+1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```

### Complexity
- **Time**: O(n) – single loop from 2 to n.
- **Space**: O(n) – the dp array of size n+1. However, this can be reduced to O(1) by noting that only the last two values are needed (space optimization).

### Advantages
- No recursion overhead; uses iteration, so it avoids stack depth limits.
- Often more efficient in practice due to loop instead of function calls.
- Easy to optimize space (only keep the last two values).

### Disadvantages
- Computes all values from 0 to n, even if some subproblems are not needed (but for Fibonacci, all are needed anyway).
- Less intuitive for problems with complex recursive structures.

---

## 3. Side-by-Side Comparison

| Aspect               | Top-Down (Memoization)                | Bottom-Up (Tabulation)                |
|----------------------|---------------------------------------|---------------------------------------|
| **Direction**        | Starts from original problem, goes down to base cases | Starts from base cases, builds up to target |
| **Implementation**   | Recursive with caching                | Iterative with table filling          |
| **Memory Usage**     | O(n) for memo + O(n) call stack       | O(n) for dp array (can be O(1) with optimization) |
| **Speed**            | Slightly slower due to recursion overhead | Generally faster due to loop          |
| **Recursion Limit**  | Limited by Python recursion depth (~1000) | No recursion limit issue              |
| **Subproblem Coverage** | Only solves subproblems that are actually reached | May solve all subproblems up to n      |
| **Ease of Understanding** | More natural for recursively defined problems | More natural for iterative thinking    |

---

## 4. Space Optimization for Bottom-Up

Since Fibonacci only depends on the previous two values, the bottom-up approach can be implemented with constant space:

```python
def fib_bottom_up_optimized(n):
    if n <= 1:
        return 1
    prev2, prev1 = 1, 1   # dp[0] and dp[1]
    for i in range(2, n+1):
        current = prev1 + prev2
        prev2, prev1 = prev1, current
    return prev1
```

This maintains O(1) space while still O(n) time.

The top-down approach cannot easily achieve O(1) space because it inherently uses the call stack for recursion, though iterative versions of memoization are possible (e.g., using explicit stack), but they are less common.

---

## 5. When to Use Which

- **Top-down** is preferred when:
  - The problem naturally fits a recursive formulation.
  - Only a subset of subproblems might be needed.
  - The state space is large and sparse, so tabulating all possible states is wasteful.

- **Bottom-up** is preferred when:
  - The problem requires computing all subproblems up to n.
  - The recursion depth might be a concern.
  - Maximum efficiency (speed and memory) is required.

For Fibonacci, both are acceptable, but bottom-up is often chosen because it’s simple, fast, and avoids recursion limits.

---

## 6. Key Takeaways

- Both approaches convert an exponential-time recursive solution into a linear-time DP solution by avoiding redundant calculations.
- **Memoization** (top-down) uses recursion and caching; **tabulation** (bottom-up) uses iteration and a table.
- Space can often be optimized in bottom-up DP, whereas top-down typically requires O(n) stack space.
- Understanding both techniques is essential for solving a wide range of DP problems in competitive programming and software development.

---

## 7. Full Code with Both Approaches

```python
# Top-Down (Memoization)
def fib_top_down(n, memo):
    if n <= 1:
        return 1
    if memo[n] != -1:
        return memo[n]
    memo[n] = fib_top_down(n-1, memo) + fib_top_down(n-2, memo)
    return memo[n]

# Bottom-Up (Tabulation)
def fib_bottom_up(n):
    if n <= 1:
        return 1
    dp = [0] * (n+1)
    dp[0] = dp[1] = 1
    for i in range(2, n+1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]

# Bottom-Up with O(1) space
def fib_bottom_up_optimized(n):
    if n <= 1:
        return 1
    prev2, prev1 = 1, 1
    for i in range(2, n+1):
        current = prev1 + prev2
        prev2, prev1 = prev1, current
    return prev1

# Example usage
n = 6
print(fib_top_down(n, [-1]*(n+1)))          # Output: 13
print(fib_bottom_up(n))                    # Output: 13
print(fib_bottom_up_optimized(n))          # Output: 13
```

Both methods produce the same result for the given definition: `fib(0)=1, fib(1)=1, fib(6)=13`.

---

## Conclusion

The top-down and bottom-up DP approaches for Fibonacci illustrate the core principles of dynamic programming: breaking a problem into overlapping subproblems and solving each only once. While they achieve the same asymptotic complexity, their implementation details, memory usage, and suitability differ. Mastery of both techniques is fundamental for solving more complex DP problems.