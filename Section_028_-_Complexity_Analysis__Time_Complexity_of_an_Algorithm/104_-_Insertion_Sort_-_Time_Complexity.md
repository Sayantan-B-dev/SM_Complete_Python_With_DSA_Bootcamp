## Time Complexity of Insertion Sort – A Detailed Step-by-Step Analysis

Insertion sort is a simple sorting algorithm that builds the final sorted array one element at a time. It is much like sorting a hand of playing cards: you take cards one by one from the unsorted part and insert them into their correct position in the sorted part.

Let's break down its time complexity in detail, counting both **comparisons** and **shifts/assignments** (since both contribute to the total work). We'll analyze the worst case, best case, and briefly mention the average case.

---

### 1. Algorithm Description

Given an array `A` of `n` elements, insertion sort works as follows:

```
for i = 1 to n-1               // i is the index of the element to be inserted
    key = A[i]                  // element to be inserted
    j = i-1
    while j >= 0 and A[j] > key // compare and shift
        A[j+1] = A[j]
        j = j-1
    A[j+1] = key
```

**Key operations**:
- **Comparisons**: In the `while` condition (`A[j] > key`).
- **Shifts/Assignments**: Moving `A[j]` to `A[j+1]` and finally placing `key` into its position.

We'll count both to understand the total work, but for asymptotic complexity we usually focus on the dominant operations.

---

### 2. Counting Comparisons and Shifts

Let `C(n)` = total number of comparisons, and `S(n)` = total number of shifts (assignments) in the worst case.

#### 2.1 Worst-Case Analysis (Array in Reverse Order)

When the array is sorted in descending order, each new element must be compared with all previously sorted elements and moved to the very beginning.

- **For i = 1** (second element):  
  - Comparisons: `A[0] > key` is true → 1 comparison.  
  - Shifts: `A[0]` shifts to `A[1]` → 1 shift.  
  Then key placed at `A[0]` (one final assignment, which we count separately if we like, but often shifts include that final placement? We'll be explicit.)

- **For i = 2** (third element):  
  - Compare with `A[1]` (true), then with `A[0]` (true) → 2 comparisons.  
  - Shifts: `A[1]` to `A[2]`, `A[0]` to `A[1]` → 2 shifts.

- **General i** (i-th element, 1-based index in loop):  
  - Comparisons: `i` comparisons (since it compares with all `i` elements in the sorted part).  
  - Shifts: `i` shifts (each larger element moves right one position).

Thus for a given `i` (where `i` runs from 1 to `n-1`), we have:
- Comparisons for this i: `i`
- Shifts for this i: `i`

**Total comparisons** in worst case:
\[
C_{\text{worst}}(n) = \sum_{i=1}^{n-1} i = 1 + 2 + \cdots + (n-1) = \frac{n(n-1)}{2}
\]

**Total shifts** in worst case:
\[
S_{\text{worst}}(n) = \sum_{i=1}^{n-1} i = \frac{n(n-1)}{2}
\]

If we also count the final placement of `key` as an assignment, then there are an additional `(n-1)` assignments (one per outer loop iteration). But those are lower order (linear), so they don't affect the asymptotic quadratic behavior. So total assignments (shifts + placements) = `n(n-1)/2 + (n-1) = (n-1)(n/2 + 1)`.

#### 2.2 Best-Case Analysis (Array Already Sorted)

When the array is already sorted in ascending order, for each `i`:

- The `while` condition `A[j] > key` is **false** immediately (because `A[i-1] ≤ key`). So only **one comparison** is performed per outer loop iteration.
- No shifts occur (the element is already in correct position).

Thus:
- Comparisons for each i: exactly 1.
- Shifts: 0.

**Total comparisons** in best case:
\[
C_{\text{best}}(n) = \sum_{i=1}^{n-1} 1 = n-1
\]

**Total shifts** in best case:
\[
S_{\text{best}}(n) = 0
\]

Placement of `key` still happens (the element is just copied back to itself? Actually the algorithm does `A[j+1] = key`, which in this case is the same index, so it's an assignment that doesn't change anything. So there are still `n-1` assignments, but they are trivial and could be optimized away in some implementations. For complexity, we can ignore them as they are O(n) anyway.

---

### 3. Asymptotic Complexity

#### 3.1 Worst-Case: O(n²)

From the exact expression:
\[
C_{\text{worst}}(n) = \frac{n(n-1)}{2} = \frac{1}{2}n^2 - \frac{1}{2}n
\]

We want to show it is **O(n²)**. According to the definition, we need constants `c` and `n₀` such that for all `n ≥ n₀`, `C(n) ≤ c·n²`.

Take `c = 1`. Then for `n ≥ 1`:
\[
\frac{1}{2}n^2 - \frac{1}{2}n \le \frac{1}{2}n^2 \le n^2
\]
Thus with `c = 1` and `n₀ = 1`, the inequality holds. Hence `C_{\text{worst}}(n) = O(n²)`.

We can also show it is **Ω(n²)** because for `n ≥ 2`, `½n² - ½n ≥ ¼n²` (since `½n² - ½n ≥ ¼n²` ⇔ `¼n² ≥ ½n` ⇔ `n ≥ 2`). Therefore `C(n) = Ω(n²)` and consequently **Θ(n²)** for the worst case. The same holds for shifts.

#### 3.2 Best-Case: O(n)

Best-case comparisons: `C_{\text{best}}(n) = n-1`. Clearly `n-1 ≤ 1·n` for `n ≥ 1`, so it is **O(n)**. It is also **Ω(n)** because `n-1 ≥ ½n` for `n ≥ 2`. Thus best case is **Θ(n)**.

Note that the best-case complexity is linear, but this only occurs when the array is already sorted. In practice, we often consider worst-case or average-case.

#### 3.3 Average-Case: O(n²)

On average (for random arrays), insertion sort performs about half the comparisons of the worst case, i.e., roughly `n²/4` comparisons. This is still quadratic, so average-case is **Θ(n²)** as well. The exact average is `n(n-1)/4` comparisons (derived from the fact that each element is expected to go halfway into the sorted portion). So average complexity is also O(n²).

---

### 4. Mathematical Representation and Bounding

We can write the exact operation count for worst-case as:
\[
T(n) = a n^2 + b n + c
\]
where `a = 1/2`, `b = -1/2`, `c = 0` for comparisons (ignoring lower-order terms). For the total work including shifts, it's similar with maybe a different constant.

**Big O**: To prove `T(n) = O(n²)`, we find `c` and `n₀` such that `T(n) ≤ c·n²`. For example, with `c = 1` and `n₀ = 1`:
\[
\frac{1}{2}n^2 - \frac{1}{2}n \le \frac{1}{2}n^2 \le n^2
\]
Thus `T(n) ≤ 1·n²` for all `n ≥ 1`. So `T(n) = O(n²)`.

**Omega**: To prove `T(n) = Ω(n²)`, we find `c'` and `n₀` such that `T(n) ≥ c'·n²`. For instance, with `c' = 1/4` and `n₀ = 2`:
\[
\frac{1}{2}n^2 - \frac{1}{2}n \ge \frac{1}{4}n^2 \quad \text{for } n \ge 2
\]
because `½n² - ½n - ¼n² = ¼n² - ½n = ¼n(n-2) ≥ 0`. Hence `T(n) = Ω(n²)`.

Since both hold, `T(n) = Θ(n²)` in worst case.

---

### 5. Visual Representation

Plot the worst-case comparisons `C(n) = n(n-1)/2` (blue curve) and the bounding lines `c₁·n²` and `c₂·n²` with `c₁ = 1/4` (lower bound) and `c₂ = 1` (upper bound). For `n ≥ 2`, the blue curve lies between them.

```
Operations
  ^
  |                     / c₂·n² = n²
  |                   /
  |                 /  ...... C(n) = n(n-1)/2
  |               /
  |             /
  |           /
  |         /  c₁·n² = n²/4
  |       /
  |     /
  |   /
  |_/______________________> n
         ↑
        n₀ = 2
```

---

### 6. Comparison with Bubble Sort

Both bubble sort and insertion sort have worst-case O(n²) and best-case O(n) (if optimized). However, insertion sort is generally faster in practice because it makes fewer swaps/shifts on average and has better cache behavior. The analysis for insertion sort shows that its inner loop can stop early (when the correct position is found), whereas bubble sort always plods through the entire unsorted part even if no swaps occur (unless optimized with a flag).

---

### 7. Summary Table

| Case       | Comparisons (exact)            | Shifts (exact)                  | Asymptotic Complexity |
|------------|--------------------------------|---------------------------------|------------------------|
| Worst      | \( \frac{n(n-1)}{2} \)        | \( \frac{n(n-1)}{2} \)          | Θ(n²)                 |
| Best       | \( n-1 \)                      | 0                               | Θ(n)                  |
| Average    | ≈ \( \frac{n(n-1)}{4} \)       | ≈ \( \frac{n(n-1)}{4} \)        | Θ(n²)                 |

The detailed step‑by‑step counting shows how the quadratic term emerges from the sum of the first `n-1` integers. This fundamental pattern is common in nested-loop algorithms where the inner loop's length decreases linearly with the outer loop.