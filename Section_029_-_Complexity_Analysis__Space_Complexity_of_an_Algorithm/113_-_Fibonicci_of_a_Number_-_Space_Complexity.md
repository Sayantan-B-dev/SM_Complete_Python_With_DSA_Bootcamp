## Space Complexity of Fibonacci Number: Recursive vs Iterative

The Fibonacci sequence is defined as:
- \( F(0) = 0 \), \( F(1) = 1 \)
- \( F(n) = F(n-1) + F(n-2) \) for \( n \geq 2 \)

We'll analyze the space complexity of two common implementations: **iterative** and **naive recursive**. We'll cover mathematical and non-mathematical approaches, graphical representations, and key terminology.

---

### 1. Iterative Fibonacci

#### Code (Python)
```python
def fib_iterative(n):
    if n <= 1:
        return n
    a, b = 0, 1
    for i in range(2, n+1):
        a, b = b, a + b
    return b
```

#### Space Complexity Analysis

- **Input space**: The input `n` (an integer) – negligible, typically considered constant.
- **Auxiliary space**: Only a few variables (`a`, `b`, `i`) are used. Their number does not grow with `n`.
- **Total auxiliary space**: \(O(1)\) – constant.

**Mathematical approach**:  
Let \(S(n)\) be the space required for input \(n\). The algorithm uses a fixed number of memory cells regardless of \(n\), so \(S(n) = c\) (constant). In Big O notation, \(O(1)\).

**Non-mathematical explanation**:  
No matter how large `n` is, you only ever store the last two Fibonacci numbers and a loop counter. The memory usage stays the same.

**Graphical representation**:  
Imagine a box representing memory. For any `n`, the box contains the same three items: `a`, `b`, `i`. No extra boxes are added as `n` grows.

```
n=5: [a][b][i]   → same as n=100: [a][b][i]
```

---

### 2. Naive Recursive Fibonacci

#### Code (Python)
```python
def fib_recursive(n):
    if n <= 1:
        return n
    return fib_recursive(n-1) + fib_recursive(n-2)
```

#### Space Complexity Analysis

- **Input space**: Still just `n` (constant).
- **Auxiliary space**: This is dominated by the **call stack**. Each recursive call adds a stack frame (containing local variables, parameters, return address). The stack unwinds only when base cases are reached.
- **Key insight**: Even though the total number of calls is exponential, the maximum stack depth is the longest chain of calls. For Fibonacci, the recursion tree is binary, but the deepest path is achieved by repeatedly following the `fib(n-1)` branch until reaching `fib(0)` or `fib(1)`. That depth is \(n\).

Thus, **auxiliary space = \(O(n)\)** (linear in `n`).

**Mathematical approach**:  
Let \(S(n)\) be the maximum stack space for `fib_recursive(n)`. Each call uses constant space \(c\). The recursion depth is \(n\) (worst case), so:
\[
S(n) = c \cdot n \quad \Rightarrow \quad O(n)
\]
We can also write a recurrence:  
\(S(n) = S(n-1) + c\) (since one branch is explored first), with base \(S(0)=c\), \(S(1)=c\). Solving gives \(S(n) = c(n+1) = O(n)\).

**Non-mathematical explanation**:  
Imagine you are making phone calls to compute Fibonacci. You call yourself with `n-1`, and while waiting for that answer, you call yourself again with `n-2`, etc. The longest chain of calls you have to wait for is `n` calls deep (like `fib(5)` calls `fib(4)` which calls `fib(3)` ...). Each pending call is kept on hold, using a "stack" of phone lines. So you need `n` lines simultaneously. That's why space grows linearly.

**Graphical representation**:  
Consider `fib_recursive(4)`. The recursion tree (Figure 1) shows all calls, but the stack at any moment only follows one path (depth-first). Figure 2 illustrates the stack at the deepest point.

```
Figure 1: Recursion tree for fib(4)
          fib(4)
         /      \
     fib(3)    fib(2)
     /    \    /    \
  fib(2) fib(1) fib(1) fib(0)
  /    \
fib(1) fib(0)

Figure 2: Call stack at deepest point (following leftmost branch)
Top of stack → fib(1)  (returns 1)
              fib(2)  (waiting for fib(1) and fib(0))
              fib(3)  (waiting for fib(2) and fib(1))
              fib(4)  (waiting for fib(3) and fib(2))
Bottom of stack (main call)
Depth = 4 → O(n) stack frames.
```

---

### 3. Comparison and Trade-offs

| Approach      | Time Complexity | Space Complexity | Explanation |
|---------------|-----------------|------------------|-------------|
| Iterative     | \(O(n)\)        | \(O(1)\)         | Constant extra memory, fast. |
| Naive Recursive | \(O(2^n)\)    | \(O(n)\)         | Exponential time, linear stack space due to recursion depth. |

- **Iterative** is superior in both time and space for Fibonacci.
- **Naive recursive** is simple to write but extremely inefficient.
- **Memoized recursion (top-down DP)** uses \(O(n)\) time and \(O(n)\) space (for the memoization array + stack depth). This trades space for time.

---

### 4. Additional Concepts and Terminology

- **Stack frame**: Memory block pushed onto the call stack for each function call; contains local variables, parameters, and return address.
- **Recursion depth**: Maximum number of nested recursive calls at any moment – determines stack space.
- **Auxiliary space**: Extra memory excluding input storage. For Fibonacci, input is just an integer, so total space ≈ auxiliary space.
- **Input space**: Memory to store the input itself – often ignored in complexity analysis because it's common to all algorithms solving the same problem.
- **Big O notation**: Describes how space grows as input size increases (upper bound).
- **Tail recursion**: A recursive call where the last operation is the recursive call. Some languages optimize tail recursion to reuse stack frame, reducing space to \(O(1)\). Fibonacci cannot be directly tail-recursive because it needs two calls.
- **Memoization**: Storing computed results to avoid recomputation; increases space but reduces time.

---

### 5. Summary

- **Iterative Fibonacci**: Space \(O(1)\) – constant, efficient.
- **Naive Recursive Fibonacci**: Space \(O(n)\) due to call stack depth, even though total calls are exponential.
- Understanding space complexity helps in choosing algorithms for memory-limited environments and optimizing performance.

For Fibonacci, the iterative method is clearly better. However, the recursive analysis illustrates how recursion depth impacts memory, a key concept for many algorithms.