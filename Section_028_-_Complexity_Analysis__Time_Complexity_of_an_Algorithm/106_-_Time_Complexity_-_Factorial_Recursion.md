## Time Complexity of Recursive Algorithms – A Step-by-Step Guide

Analyzing recursive algorithms often involves setting up a **recurrence relation** that describes the running time in terms of the input size. Solving this recurrence gives us the asymptotic complexity. Let's explore the most common methods, using the factorial function as our primary example.

---

### 1. The Factorial Function (A Simple Recursive Algorithm)

Consider the classic recursive implementation:

```python
def factorial(n):
    if n == 0:
        return 1
    else:
        return n * factorial(n-1)
```

We want to find its time complexity. Let `T(n)` be the number of operations (or time) required for input `n`.

- **Base case**: When `n = 0`, the function does a constant amount of work (comparison and return). So `T(0) = c` (some constant).
- **Recursive case**: For `n > 0`, the algorithm:
  - Does a constant amount of work (comparison, multiplication, and return) – call this `k`.
  - Then makes one recursive call on `n-1`.

Thus the recurrence is:
\[
T(n) = T(n-1) + k \quad \text{for } n > 0, \quad T(0) = c
\]

---

### 2. Solving the Recurrence – Intuitive Approach

We can unroll the recurrence (also called the **iteration method**):

\[
\begin{align*}
T(n) &= T(n-1) + k \\
     &= [T(n-2) + k] + k = T(n-2) + 2k \\
     &= T(n-3) + 3k \\
     &\;\;\vdots \\
     &= T(0) + n \cdot k = c + nk
\end{align*}
\]

So `T(n) = c + nk`. This is a linear function in `n`. Therefore, the time complexity is **O(n)**.

The intuitive idea: each recursive call reduces `n` by 1, and we do constant work per call. Since we make `n` calls (from `n` down to 0), the total work is proportional to `n`.

---

### 3. Recurrence Relation Method – Step by Step

The recurrence relation method involves three steps:

1. **Formulate the recurrence** based on the algorithm's structure.
2. **Solve the recurrence** using one of several techniques.
3. **Express the result** in asymptotic notation.

#### Common Techniques to Solve Recurrences

##### a) Iteration (Substitution) Method
We already did this for factorial. It works well for recurrences where the pattern is clear.

##### b) Recursion Tree Method
Draw a tree where each node represents the cost of a recursive call. Sum the costs at each level.

**Example for factorial**: The tree is just a chain (each node has one child). The depth is `n`, and each node has cost `k`. Total cost = `n*k` → O(n).

**Example for binary search**: `T(n) = T(n/2) + O(1)`. The tree has depth `log n`, each level cost O(1) → total O(log n).

**Example for merge sort**: `T(n) = 2T(n/2) + O(n)`. The tree has log n levels, each level total cost O(n) → total O(n log n).

##### c) Master Theorem
For recurrences of the form `T(n) = a T(n/b) + f(n)` where `a ≥ 1, b > 1`, and `f(n)` is asymptotically positive, the Master Theorem provides a direct solution. It has three cases based on comparing `f(n)` with `n^{log_b a}`.

**Factorial does not fit** because the subproblem size is `n-1`, not `n/b`. The Master Theorem applies only to divide‑and‑conquer recurrences where the subproblems are a fraction of the original.

##### d) Akra–Bazzi Method
A more general method for recurrences of the form:
\[
T(n) = \sum_{i=1}^{k} a_i T(b_i n + h_i(n)) + f(n)
\]
It handles cases where the subproblem sizes are not necessarily equal or fractional. This is beyond introductory scope but useful for complex recurrences.

---

### 4. More Examples to Illustrate Different Patterns

#### Example 1: Binary Search
```python
def binary_search(arr, low, high, target):
    if low > high: return -1
    mid = (low + high) // 2
    if arr[mid] == target: return mid
    elif arr[mid] < target: return binary_search(arr, mid+1, high, target)
    else: return binary_search(arr, low, mid-1, target)
```
Recurrence: `T(n) = T(n/2) + O(1)`, with `T(1) = O(1)`.  
Solution (iteration or Master Theorem case 2): `T(n) = O(log n)`.

#### Example 2: Merge Sort
```python
def merge_sort(arr):
    if len(arr) <= 1: return arr
    mid = len(arr)//2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)
```
Recurrence: `T(n) = 2T(n/2) + O(n)`.  
Master Theorem case 2: `T(n) = O(n log n)`.

#### Example 3: Fibonacci (Naive Recursion)
```python
def fib(n):
    if n <= 1: return n
    return fib(n-1) + fib(n-2)
```
Recurrence: `T(n) = T(n-1) + T(n-2) + O(1)`. This is not of the form for Master Theorem. The solution is exponential: `T(n) = O(2^n)` (actually about golden ratio^n, but still exponential). This can be solved via recursion tree – the tree is highly unbalanced, and the number of nodes is about 2^n.

---

### 5. Summary of Methods

| Method              | When to Use                                                                 | Example                         |
|---------------------|-----------------------------------------------------------------------------|---------------------------------|
| Iteration (unrolling) | Simple recurrences with clear pattern (like linear recurrences)            | Factorial: T(n)=T(n-1)+O(1)     |
| Recursion Tree      | Visualizing the cost at each level; good for divide‑and‑conquer            | Merge sort, binary search       |
| Master Theorem      | Recurrences of form T(n)=aT(n/b)+f(n) (a≥1, b>1)                           | Merge sort, binary search       |
| Substitution (guess & prove) | When you have a guess for the solution and can prove by induction | Many recurrences                 |
| Akra–Bazzi          | More general recurrences (uneven splits, etc.)                             | Some advanced divide‑and‑conquer |

---

### 6. Key Takeaways for Recursive Complexity

- Identify the recurrence by counting the work done in each recursive call and the number of subproblems.
- For recurrences that reduce the input size by a constant (like `n-1`), you often get linear complexity (if only one recursive call) or exponential (if multiple calls).
- For recurrences that reduce by a fraction (like `n/2`), you get logarithmic or linearithmic complexity depending on the work done at each level.
- The Master Theorem is a powerful tool for many common divide‑and‑conquer algorithms.
- Always remember that the base case(s) matter and must be included in the recurrence.

Understanding these methods will allow you to analyze any recursive algorithm systematically. The factorial example is the simplest – it demonstrates how a single recursive call that reduces the problem size by one leads to linear time.