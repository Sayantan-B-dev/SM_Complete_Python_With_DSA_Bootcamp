## Time Complexity of Bubble Sort – A Step-by-Step Analysis

Bubble sort is one of the simplest sorting algorithms. It repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. The pass through the list is repeated until no swaps are needed, which indicates the list is sorted.

Let's analyze its time complexity in detail, counting the number of comparisons (the dominant operation) and then expressing the result in asymptotic notation.

---

### 1. Algorithm Description (Worst-Case Scenario)

Consider an array `arr` of size `n`. A typical implementation (without early termination) is:

```
for i = 0 to n-2                     // outer loop runs n-1 times
    for j = 0 to n-2-i                // inner loop runs (n-1-i) times
        if arr[j] > arr[j+1]          // one comparison
            swap(arr[j], arr[j+1])    // swap (ignored in count, as we focus on comparisons)
```

We will count only the **comparisons** because they are the basic operation that determines the algorithm’s growth. Swaps are at most the same order, so they don’t affect the asymptotic complexity.

---

### 2. Counting Comparisons

- In the **first pass** (i = 0), the inner loop runs `n-1` times → `n-1` comparisons.
- In the **second pass** (i = 1), the inner loop runs `n-2` times → `n-2` comparisons.
- ...
- In the last pass (i = n-2), the inner loop runs `1` time → `1` comparison.

Thus the total number of comparisons `C(n)` is:

\[
C(n) = (n-1) + (n-2) + \cdots + 2 + 1
\]

This is the sum of the first `n-1` natural numbers:

\[
C(n) = \frac{(n-1) \cdot n}{2} = \frac{n(n-1)}{2}
\]

---

### 3. Mathematical Expression

Expanding the product:

\[
C(n) = \frac{n^2 - n}{2} = \frac{1}{2}n^2 - \frac{1}{2}n
\]

This is the exact number of comparisons performed by the basic bubble sort (without any optimization) for any array of size `n`. Note that in the worst case (array in reverse order), this is also the number of swaps, but we only need comparisons for complexity.

---

### 4. Asymptotic Analysis – Big O Notation

We want to find a simple function `g(n)` that gives an **upper bound** for `C(n)`. The dominant term is `n²`, so we suspect `C(n) = O(n²)`.

#### Formal Definition of Big O
We say that `C(n) = O(g(n))` if there exist positive constants `c` and `n₀` such that:

\[
C(n) \le c \cdot g(n) \quad \text{for all } n \ge n₀
\]

Here we take `g(n) = n²`.

#### Finding Suitable Constants
We have:

\[
C(n) = \frac{1}{2}n^2 - \frac{1}{2}n
\]

For `n ≥ 1`, the term `-½n` is negative, so:

\[
C(n) \le \frac{1}{2}n^2 \quad \text{(since subtracting makes it smaller)}
\]

But we need an inequality of the form `C(n) ≤ c·n²`. Choosing `c = 1` works because:

\[
\frac{1}{2}n^2 - \frac{1}{2}n \le \frac{1}{2}n^2 \le n^2 \quad \text{for all } n \ge 1
\]

Thus, with `c = 1` and `n₀ = 1`, we have `C(n) ≤ 1·n²` for all `n ≥ 1`. Therefore:

\[
C(n) = O(n^2)
\]

We could also pick a smaller `c` (e.g., `c = 1/2`), but then we would need a larger `n₀` because for small `n`, `½n² - ½n` might exceed `½n²`? Actually it never exceeds `½n²` because subtracting makes it smaller. Wait, careful: `½n² - ½n` is less than `½n²` for positive n, so `c = 1/2` also works: `½n² - ½n ≤ ½n²` holds. So actually any `c ≥ 1/2` works. But the existence of any such pair is enough.

---

### 5. Why Not O(n) or O(n log n)?

- **O(n)** would require the number of comparisons to grow linearly with `n`. But our expression has an `n²` term, so for large `n`, `n²` dominates. No constant factor can make `n²` bounded by a linear function.
- **O(n log n)** also grows slower than `n²`. For large `n`, `n² / (n log n) = n / log n → ∞`, so `n²` is not bounded by a constant multiple of `n log n`.

Thus bubble sort is indeed **quadratic** in the worst case.

---

### 6. Best-Case and Average-Case

- **Best case**: If the array is already sorted, an optimized version with a flag can stop after the first pass, performing only `n-1` comparisons → **O(n)**. But the unoptimized version still does all `n(n-1)/2` comparisons, so its best case is also O(n²). Usually we analyze the standard version without early termination, so best case is still O(n²) but we note that optimization can improve it.
- **Average case**: For random arrays, the expected number of comparisons is still proportional to `n²` (roughly half of the worst case), so **O(n²)** as well.

---

### 7. Visual Representation

Plot `C(n) = n(n-1)/2` (blue curve) and the bounding line `c·n²` with `c = 1` (red dashed). For all `n ≥ 1`, the blue curve stays below the red line.

```
Operations
  ^
  |                     / (c·n²)
  |                   /
  |                 /
  |               /   ...... C(n)
  |             /
  |           /
  |         /
  |       /
  |     /
  |   /
  |_/______________________> n
```

---

### 8. Summary

| Algorithm          | Comparisons Count (exact) | Asymptotic Complexity |
|--------------------|---------------------------|------------------------|
| Bubble Sort (worst) | \( \frac{n(n-1)}{2} \)   | O(n²)                 |

The key takeaway: The quadratic growth makes bubble sort impractical for large datasets, but it serves as an excellent example for understanding how to derive complexity from nested loops and summation formulas.