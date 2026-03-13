## Time Complexity of Selection Sort – A Detailed Step-by-Step Analysis

Selection sort is a simple, in‑place comparison‑based sorting algorithm. It works by repeatedly finding the minimum element from the unsorted part of the array and swapping it with the first element of that unsorted part. Unlike insertion sort, its behavior does **not** depend on the initial order of the elements – the number of comparisons is always the same.

Let's break down its time complexity in detail, counting **comparisons** (the dominant operation) and **swaps**. We'll derive exact expressions and then express the asymptotic bounds.

---

### 1. Algorithm Description

Given an array `A` of length `n`, selection sort can be implemented as:

```
for i = 0 to n-2                     // i points to the start of the unsorted region
    min_index = i
    for j = i+1 to n-1                // find the smallest element in the unsorted part
        if A[j] < A[min_index]        // one comparison
            min_index = j
    swap A[i] and A[min_index]         // swap (three assignments, but we count only comparisons for complexity)
```

We will focus on the number of comparisons, because they determine the asymptotic growth. Swaps are at most O(n) and do not affect the overall quadratic complexity.

---

### 2. Counting Comparisons

- In the **first pass** (`i = 0`), the inner loop runs from `j = 1` to `n-1`, performing `n-1` comparisons.
- In the **second pass** (`i = 1`), the inner loop runs from `j = 2` to `n-1`, performing `n-2` comparisons.
- ...
- In the **last pass** (`i = n-2`), the inner loop runs from `j = n-1` to `n-1`, performing exactly `1` comparison.

Thus the total number of comparisons `C(n)` is:

\[
C(n) = (n-1) + (n-2) + \cdots + 2 + 1
\]

This is the sum of the first `n-1` natural numbers:

\[
C(n) = \frac{(n-1) \cdot n}{2} = \frac{n(n-1)}{2}
\]

**Important:** This count is **independent of the input order**. Whether the array is already sorted, reverse sorted, or random, the algorithm always performs exactly the same number of comparisons. Therefore the best‑case, worst‑case, and average‑case number of comparisons are identical.

---

### 3. Counting Swaps

Swaps occur once per outer loop iteration (when we exchange the found minimum with `A[i]`). The loop runs `n-1` times, so:

\[
S(n) = n-1
\]

Each swap involves three assignments (if we count the temporary variable), but that is still O(n). Because `n-1` is linear, swaps do not affect the quadratic complexity.

---

### 4. Exact Mathematical Expression

\[
C(n) = \frac{n(n-1)}{2} = \frac{1}{2}n^2 - \frac{1}{2}n
\]

This is a quadratic function in `n`. The term `-½n` is negligible for large `n`.

---

### 5. Asymptotic Analysis

#### 5.1 Big O (Upper Bound)
We want to show that `C(n) = O(n²)`. According to the definition, we need constants `c` and `n₀` such that for all `n ≥ n₀`:

\[
C(n) \le c \cdot n^2
\]

Choose `c = 1` and `n₀ = 1`. Then:

\[
\frac{1}{2}n^2 - \frac{1}{2}n \le \frac{1}{2}n^2 \le n^2 \quad \text{for } n \ge 1
\]

Thus `C(n) ≤ 1·n²` holds, proving `C(n) = O(n²)`. (Any `c ≥ 1/2` would also work; the existence is sufficient.)

#### 5.2 Omega (Lower Bound)
We show `C(n) = Ω(n²)` by finding constants `c'` and `n₀` such that:

\[
C(n) \ge c' \cdot n^2
\]

Take `c' = 1/4` and `n₀ = 2`. For `n ≥ 2`:

\[
\frac{1}{2}n^2 - \frac{1}{2}n \ge \frac{1}{4}n^2
\]

because `½n² - ½n - ¼n² = ¼n² - ½n = ¼n(n-2) ≥ 0`. Hence `C(n) ≥ ¼·n²` for all `n ≥ 2`, proving `C(n) = Ω(n²)`.

#### 5.3 Theta (Tight Bound)
Since we have both `O(n²)` and `Ω(n²)` with the same `g(n) = n²`, we conclude:

\[
C(n) = \Theta(n^2)
\]

The same holds for the total time including swaps, because swaps contribute only a linear term, which does not change the quadratic growth.

---

### 6. Visual Representation

Plot `C(n) = n(n-1)/2` (blue curve) along with the lower bound `¼·n²` (green dashed) and the upper bound `1·n²` (red dashed). For `n ≥ 2`, the blue curve lies between the two bounding lines.

```
Operations
  ^
  |                     / (c₂·n² = n²)
  |                   /
  |                 /
  |               /   ...... C(n) = n(n-1)/2
  |             /
  |           /
  |         / (c₁·n² = n²/4)
  |       /
  |     /
  |   /
  |_/______________________> n
         ↑
        n₀ = 2
```

---

### 7. Comparison with Bubble Sort and Insertion Sort

| Algorithm       | Best Case Comparisons | Worst Case Comparisons | Average Comparisons | Swaps (worst) |
|-----------------|-----------------------|------------------------|---------------------|---------------|
| **Selection Sort** | \( \frac{n(n-1)}{2} \) (Θ(n²)) | same (Θ(n²)) | same (Θ(n²)) | Θ(n) |
| **Bubble Sort** (optimized) | Θ(n) (if already sorted) | Θ(n²) | Θ(n²) | Θ(n²) |
| **Insertion Sort** | Θ(n) (if already sorted) | Θ(n²) | Θ(n²) | Θ(n²) (shifts) |

**Key Observations**:
- Selection sort always performs the same number of comparisons, making it **non‑adaptive** (it does not take advantage of existing order).
- Its swap count is linear, which can be an advantage if swapping is expensive (e.g., large records), because it makes at most `n-1` swaps, whereas bubble sort may make O(n²) swaps.
- However, because it always does Θ(n²) comparisons, it is generally slower than insertion sort for nearly sorted data.

---

### 8. Summary

| Case       | Comparisons (exact)            | Swaps (exact)  | Asymptotic Complexity |
|------------|--------------------------------|----------------|------------------------|
| Best       | \( \frac{n(n-1)}{2} \)        | \( n-1 \)      | Θ(n²)                 |
| Worst      | \( \frac{n(n-1)}{2} \)        | \( n-1 \)      | Θ(n²)                 |
| Average    | \( \frac{n(n-1)}{2} \)        | \( n-1 \)      | Θ(n²)                 |

Selection sort’s time complexity is always **Θ(n²)** due to the fixed number of comparisons. This makes it inefficient for large lists, but its simplicity and minimal swapping can be useful in certain memory‑constrained or swap‑costly scenarios. The step‑by‑step counting of comparisons (sum of first `n-1` integers) clearly illustrates why it is quadratic.