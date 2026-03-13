https://www.bigocheatsheet.com  
https://www.desmos.com/calculator/l99rykuzx8  

---

# Comprehensive Time Complexity Analysis – A Definitive Tabular Guide

This guide consolidates all essential concepts of time complexity analysis into a set of clear, structured tables. Use it as a quick reference for definitions, notations, common complexities, analysis techniques, and practical tips.

---

## 1. What is Time Complexity?

| Aspect | Description |
|--------|-------------|
| **Definition** | A theoretical measure of the amount of time an algorithm takes to run, expressed as a function of the input size `n`. |
| **Goal** | To compare algorithm efficiency independent of hardware, language, or implementation details. |
| **Key Principle** | Count **basic operations** (comparisons, assignments, arithmetic) – not actual seconds. |
| **Focus** | Growth rate as `n` becomes very large (asymptotic behavior). |
| **Why Not Actual Time?** | Actual time varies with machine, compiler, input data, and background processes. Complexity abstracts these away. |

---

## 2. Asymptotic Notations

| Notation | Name | Meaning | Mathematical Definition | Typical Use | Analogy |
|----------|------|---------|-------------------------|-------------|---------|
| **O (Big O)** | Upper bound | Algorithm takes **at most** this long | `f(n) ≤ c·g(n)` for `n ≥ n₀` | Worst‑case guarantee | “This task will be done within 2 hours” |
| **Ω (Omega)** | Lower bound | Algorithm takes **at least** this long | `f(n) ≥ c·g(n)` for `n ≥ n₀` | Best‑case (or lower bound proof) | “This task needs at least 30 minutes” |
| **Θ (Theta)** | Tight bound | Algorithm grows **exactly** like this | `c₁·g(n) ≤ f(n) ≤ c₂·g(n)` for `n ≥ n₀` | Exact growth rate (both upper and lower) | “This task always takes about 1 hour” |

### Visual Representation of Bounds

```
Operations
  ^
  |     .......... f(n)   (actual)
  |    .        .
  |   .          . c₂·g(n)   (upper)
  |  .            .
  | .              .
  |.                . c₁·g(n)   (lower)
  +--------------------> n
         n₀
```

---

## 3. Common Time Complexities – At a Glance

| Complexity | Name | Growth Pattern | Example Algorithms / Operations | Scalability |
|------------|------|----------------|----------------------------------|-------------|
| **O(1)** | Constant | Flat, independent of n | Array access, hash table lookup, stack push/pop | Excellent |
| **O(log n)** | Logarithmic | Very slow increase | Binary search, balanced BST operations | Excellent |
| **O(n)** | Linear | Grows proportionally | Linear search, finding max/min, array traversal | Good |
| **O(n log n)** | Linearithmic | Slightly steeper than linear | Merge sort, heap sort, fast Fourier transform | Fair |
| **O(n²)** | Quadratic | Steep parabola | Bubble sort (worst), insertion sort (worst), nested loops over n | Poor |
| **O(2ⁿ)** | Exponential | Extremely steep | Recursive Fibonacci (naive), subset generation | Very poor |
| **O(n!)** | Factorial | Explosive growth | Traveling salesman (brute force), permutations | Impractical |

### Visual Growth Comparison (Conceptual)

```
Operations
  ^
  |                                 / (n!)
  |                               /
  |                            /   (2ⁿ)
  |                         /
  |                      /      (n²)
  |                   /
  |                /            (n log n)
  |             /
  |          /                  (n)
  |       /
  |    /                         (log n)
  |___/_______________________________> n
```

---

## 4. How to Determine Time Complexity from Code

### 4.1 General Rules

| Rule | Explanation |
|------|-------------|
| **Count operations, not seconds** | Focus on comparisons, assignments, etc. |
| **Focus on highest‑power term** | For large n, lower terms are negligible. |
| **Ignore constant coefficients** | 5n² and 1000n² are both O(n²). |
| **Consider worst case** | Usually the most useful guarantee. |

### 4.2 Common Code Patterns

| Code Pattern | Complexity | Reasoning |
|--------------|------------|-----------|
| Single loop `for i in range(n):` | O(n) | Executes n times, each iteration O(1). |
| Nested loops both `range(n)` | O(n²) | n × n iterations. |
| Nested loop with inner `range(i)` | O(n²) | Sum of 1..n-1 = n(n-1)/2. |
| Loop where index multiplies (`i *= 2`) | O(log n) | Number of steps ≈ log₂ n. |
| Recursion with one call `T(n) = T(n-1) + O(1)` | O(n) | Unrolls n times. |
| Recursion with two calls `T(n) = 2T(n-1) + O(1)` | O(2ⁿ) | Exponential growth. |
| Divide and conquer `T(n) = 2T(n/2) + O(n)` | O(n log n) | Merge sort recurrence. |

### 4.3 Analyzing Recursive Algorithms

| Method | When to Use | Example |
|--------|-------------|---------|
| **Iteration (unfolding)** | Simple recurrences (linear, etc.) | Factorial: T(n)=T(n-1)+O(1) → O(n) |
| **Recursion Tree** | Visualizing divide‑and‑conquer | Merge sort: each level costs O(n), log n levels → O(n log n) |
| **Master Theorem** | Recurrences of form T(n)=aT(n/b)+f(n) | Binary search: T(n)=T(n/2)+O(1) → O(log n) |
| **Substitution (guess & prove)** | When you have a candidate solution | Often used for recurrences not fitting Master Theorem |

---

## 5. Complexity of Classic Algorithms (Tabular Summary)

| Algorithm | Best Case | Average Case | Worst Case | Space Complexity | Notes |
|-----------|-----------|--------------|------------|------------------|-------|
| **Linear Search** | O(1) | O(n) | O(n) | O(1) | |
| **Binary Search** | O(1) | O(log n) | O(log n) | O(1) | Sorted array required |
| **Bubble Sort** | O(n) (optimized) | O(n²) | O(n²) | O(1) | |
| **Insertion Sort** | O(n) | O(n²) | O(n²) | O(1) | Good for small or nearly sorted data |
| **Selection Sort** | O(n²) | O(n²) | O(n²) | O(1) | Always does n(n-1)/2 comparisons |
| **Merge Sort** | O(n log n) | O(n log n) | O(n log n) | O(n) | Stable, divide and conquer |
| **Quick Sort** | O(n log n) | O(n log n) | O(n²) | O(log n) | In‑place, pivot choice matters |
| **Heap Sort** | O(n log n) | O(n log n) | O(n log n) | O(1) | |
| **Counting Sort** | O(n+k) | O(n+k) | O(n+k) | O(k) | k = range of input |
| **Radix Sort** | O(d·n) | O(d·n) | O(d·n) | O(n) | d = number of digits |
| **Fibonacci (naive recursion)** | O(φⁿ) | O(φⁿ) | O(φⁿ) | O(n) | Exponential (φ≈1.618) |
| **Fibonacci (iterative)** | O(n) | O(n) | O(n) | O(1) | Linear |
| **Dijkstra (binary heap)** | O((V+E) log V) | O((V+E) log V) | O((V+E) log V) | O(V) | |
| **Floyd‑Warshall** | O(V³) | O(V³) | O(V³) | O(V²) | All‑pairs shortest path |

---

## 6. Practical Tips: Guessing Complexity from Constraints

When solving problems on platforms like Codeforces, LeetCode, etc., the input size often hints at the expected complexity.

| Constraint (n) | Likely Acceptable Complexity | Typical Algorithms |
|----------------|------------------------------|---------------------|
| n ≤ 10–12 | O(n!), O(2ⁿ) | Brute force, recursion, permutations |
| n ≤ 20–30 | O(2ⁿ) | Bitmask, backtracking |
| n ≤ 100 | O(n⁴) | Dynamic programming with 4 dimensions |
| n ≤ 500 | O(n³) | Floyd‑Warshall, DP with 3 dimensions |
| n ≤ 10⁴ | O(n²) | Nested loops, simple DP |
| n ≤ 10⁶ | O(n log n) | Sorting, divide & conquer, heap |
| n ≤ 10⁸ | O(n) | Linear scan, greedy, math |
| n > 10⁸ | O(log n) or O(1) | Binary search, mathematical formulas |

---

## 7. Mathematical Representation & Formal Definition

Let `f(n)` be the exact number of operations.

| Notation | Formal Condition |
|----------|------------------|
| **O(g(n))** | ∃ c > 0, n₀ > 0 such that `f(n) ≤ c·g(n)` for all `n ≥ n₀` |
| **Ω(g(n))** | ∃ c > 0, n₀ > 0 such that `f(n) ≥ c·g(n)` for all `n ≥ n₀` |
| **Θ(g(n))** | ∃ c₁, c₂ > 0, n₀ > 0 such that `c₁·g(n) ≤ f(n) ≤ c₂·g(n)` for all `n ≥ n₀` |

**Example:** For `f(n) = 5n² + 3n + 1`, we can show:
- O(n²): `f(n) ≤ 6n²` for `n ≥ 1` (c=6)
- Ω(n²): `f(n) ≥ 5n²` for `n ≥ 1` (c=5)
- Θ(n²): both hold.

---

## 8. Time Complexity ≠ Time Taken – Why?

| Factor | Explanation |
|--------|-------------|
| **Machine speed** | A faster CPU executes the same number of operations in less time. |
| **Programming language** | Compiled code (C, C++) runs faster than interpreted (Python, JavaScript). |
| **Implementation details** | Small optimizations (e.g., using local variables) can change constant factors. |
| **Input specifics** | Best‑case vs. worst‑case input produce different operation counts. |
| **Background processes** | System load affects actual runtime. |

Complexity analysis gives a **machine‑independent, asymptotic** measure, allowing fair comparison between algorithms.

---

## 9. Summary: The Three Golden Rules

1. **Talk in terms of number of operations, not time.**
2. **Focus only on the highest‑power term.**
3. **Don’t care about coefficients.**

These rules make time complexity a powerful, universal tool for algorithm design and selection.

---

### 1. What is the time complexity of the following code?

```python
a = 0
for i in range(N):
    a = a + i
```

Write your analysis below:

### 2. What is the time complexity of the following code?

```python
a = 0
for i in range(N):
    for j in range(N):
        a = a + i + j
```

Write your analysis below:

### 3. What is the time complexity of the following code?

```python
a = 0
for i in range(N):
    for j in range(i):
        a = a + i + j
```

Write your analysis below:

### 4. What is the time complexity of the following code?

```python
a = 0
for i in range(N):
    for j in range(N, i, -1):
        a = a + i + j
```

Write your analysis below:

### 5. What is the time complexity of the following code?

```python
a = 0
for i in range(N):
    for j in range(N):
        for k in range(N):
            a = a + i + j + k
```

Write your analysis below:

### 6. What is the time complexity of the following code?

```python
a = 0
i = 0
while i < N:
    a = a + i
    i *= 2
```

Write your analysis below:

### 7. What is the time complexity of the following code?

```python
a = 0
i = 1
while i < N:
    a = a + i
    i *= 3
```

Write your analysis below:

### 8. What is the time complexity of the following code?

```python
a = 0
for i in range(N):
    for j in range(i, N):
        for k in range(j, N):
            a = a + i + j + k
```

Write your analysis below:

### 9. What is the time complexity of the following recursive function?

```python
def recursive_function(n):
    if n <= 1:
        return 1
    else:
        return recursive_function(n-1) + recursive_function(n-1)
```

Write your analysis below:

### 10. What is the time complexity of the following code?

```python
import math

a = 0
for i in range(N):
    for j in range(int(math.sqrt(N))):
        a = a + i + j
```

Write your analysis below:

---

## Solutions

### 1. Solution
**Code:**
```python
a = 0
for i in range(N):
    a = a + i
```
**Explanation:** This code runs a loop N times, so the time complexity is **O(N)**.

### 2. Solution
**Code:**
```python
a = 0
for i in range(N):
    for j in range(N):
        a = a + i + j
```
**Explanation:** The nested loops each run N times, resulting in a time complexity of **O(N²)**.

### 3. Solution
**Code:**
```python
a = 0
for i in range(N):
    for j in range(i):
        a = a + i + j
```
**Explanation:** The outer loop runs N times, while the inner loop runs i times. The total number of iterations is the sum of i from 1 to N‑1, which is approximately N(N‑1)/2, so the complexity is **O(N²)**.

### 4. Solution
**Code:**
```python
a = 0
for i in range(N):
    for j in range(N, i, -1):
        a = a + i + j
```
**Explanation:** The outer loop runs N times, and the inner loop runs (N - i) times. The total iterations are the sum of (N‑i) for i = 0 to N‑1, which is also N(N+1)/2, hence **O(N²)**.

### 5. Solution
**Code:**
```python
a = 0
for i in range(N):
    for j in range(N):
        for k in range(N):
            a = a + i + j + k
```
**Explanation:** Three nested loops each run N times, resulting in **O(N³)**.

### 6. Solution
**Code:**
```python
a = 0
i = 0
while i < N:
    a = a + i
    i *= 2
```
**Explanation:** The loop variable doubles each time, so the number of iterations is about log₂(N). Thus **O(log N)**.

### 7. Solution
**Code:**
```python
a = 0
i = 1
while i < N:
    a = a + i
    i *= 3
```
**Explanation:** The loop variable triples each time, so the number of iterations is about log₃(N). In Big O, we ignore the base, so it is **O(log N)**.

### 8. Solution
**Code:**
```python
a = 0
for i in range(N):
    for j in range(i, N):
        for k in range(j, N):
            a = a + i + j + k
```
**Explanation:** This is a triple nested loop where each loop’s range depends on the outer indices. The number of iterations is approximately N³/6, so the complexity is **O(N³)**.

### 9. Solution
**Code:**
```python
def recursive_function(n):
    if n <= 1:
        return 1
    else:
        return recursive_function(n-1) + recursive_function(n-1)
```
**Explanation:** Each call creates two recursive calls, forming a binary tree of depth n. The total number of calls is 2ⁿ – 1, so the time complexity is **O(2ⁿ)** (exponential).

### 10. Solution
**Code:**
```python
import math
a = 0
for i in range(N):
    for j in range(int(math.sqrt(N))):
        a = a + i + j
```
**Explanation:** The outer loop runs N times, and the inner loop runs √N times. The total iterations are N·√N = N^(3/2), so the complexity is **O(N^(3/2))**, often written as **O(N√N)**.