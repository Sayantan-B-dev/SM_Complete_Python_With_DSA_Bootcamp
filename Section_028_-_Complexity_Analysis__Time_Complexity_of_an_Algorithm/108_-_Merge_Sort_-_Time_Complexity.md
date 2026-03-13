## Time Complexity of Merge Sort – A Detailed Step-by-Step Analysis

Merge sort is a classic divide‑and‑conquer algorithm that splits the array into halves, recursively sorts each half, and then merges the two sorted halves. Its time complexity is **Θ(n log n)** in all cases (best, worst, average). Let's derive this step by step.

---

### 1. Algorithm Description

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])   # recursively sort left half
    right = merge_sort(arr[mid:])  # recursively sort right half
    return merge(left, right)      # merge the two sorted halves

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    # Add remaining elements (if any)
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

**Key operations**:
- Splitting (constant time, just computing mid)
- Recursive calls on two halves of size `n/2`
- Merging: The `merge` function compares and copies elements, doing **O(n)** work for a total of `n` elements.

---

### 2. Recurrence Relation

Let `T(n)` be the total time (or number of comparisons) for sorting an array of size `n`.

- Base case: `T(1) = c` (constant time, no merging needed).
- For `n > 1`: we have two recursive calls on `n/2` each, plus the merging step which takes linear time. So:

\[
T(n) = 2T\left(\frac{n}{2}\right) + f(n)
\]

where `f(n)` represents the time for merging. In the simplest form, `f(n) = k·n` (with `k` a constant). So:

\[
T(n) = 2T\left(\frac{n}{2}\right) + k n \quad \text{for } n > 1,\quad T(1) = c
\]

We can also write it as `T(n) = 2T(n/2) + O(n)`.

---

### 3. Solving the Recurrence

We'll solve using the **iteration method** (unfolding) and then confirm with a recursion tree and the Master Theorem.

#### 3.1 Iteration (Unfolding)

Assume `n` is a power of 2 for simplicity (the asymptotic result holds for all `n`). Write:

\[
\begin{align*}
T(n) &= 2T\left(\frac{n}{2}\right) + k n \\
T\left(\frac{n}{2}\right) &= 2T\left(\frac{n}{4}\right) + k \frac{n}{2} \\
T\left(\frac{n}{4}\right) &= 2T\left(\frac{n}{8}\right) + k \frac{n}{4} \\
&\;\;\vdots
\end{align*}
\]

Substitute back:

\[
\begin{align*}
T(n) &= 2\left[2T\left(\frac{n}{4}\right) + k\frac{n}{2}\right] + k n \\
     &= 4T\left(\frac{n}{4}\right) + k n + k n \\
     &= 4\left[2T\left(\frac{n}{8}\right) + k\frac{n}{4}\right] + 2k n \\
     &= 8T\left(\frac{n}{8}\right) + k n + k n + k n \\
     &\;\;\vdots
\end{align*}
\]

After `i` steps, we get:

\[
T(n) = 2^i T\left(\frac{n}{2^i}\right) + i \cdot (k n)
\]

We stop when `n/2^i = 1` (i.e., the base case), so `i = \log_2 n`. Then:

\[
T(n) = 2^{\log_2 n} T(1) + (\log_2 n) \cdot (k n) = n \cdot c + k n \log_2 n
\]

Thus:

\[
T(n) = c n + k n \log_2 n
\]

Since both terms are asymptotically dominated by `n log n` (the linear term is lower order), we have:

\[
T(n) = \Theta(n \log n)
\]

#### 3.2 Recursion Tree Method

- The root corresponds to the original problem of size `n`, with cost `k n` (merging).
- It splits into two subproblems of size `n/2`, each with cost `k n/2`.
- Those split further into four subproblems of size `n/4`, each with cost `k n/4`.
- This continues until we reach subproblems of size 1 (leaves) which have constant cost `c` (but we usually ignore constants).

The tree has `log₂ n + 1` levels (level 0 to level log₂ n). At level `i` (starting at 0), there are `2^i` subproblems, each of size `n/2^i`, and each contributes a merging cost of `k·(n/2^i)`. So the total cost at level `i` is:

\[
2^i \cdot k \cdot \frac{n}{2^i} = k n
\]

Thus **each level costs exactly `k n`**. There are `log₂ n + 1` levels, so total cost = `(log₂ n + 1) · k n`. That is `k n log₂ n + k n`. Again, this is `Θ(n log n)`.

#### 3.3 Master Theorem

The recurrence `T(n) = 2T(n/2) + Θ(n)` fits the form `T(n) = a T(n/b) + f(n)` with `a = 2`, `b = 2`, `f(n) = Θ(n)`. Compute:

\[
n^{\log_b a} = n^{\log_2 2} = n^1 = n
\]

We compare `f(n)` with `n^{\log_b a}`: here they are equal (both `Θ(n)`). This is **Case 2** of the Master Theorem, which gives:

\[
T(n) = \Theta(n^{\log_b a} \log n) = \Theta(n \log n)
\]

---

### 4. Best, Worst, and Average Cases

- **Best case**: The array is already sorted, but merge sort still divides and merges – it always does the same number of comparisons and merges. The algorithm does not adapt to input order, so the number of comparisons is the same regardless of initial order. Hence best case is also `Θ(n log n)`.
- **Worst case**: The array is in reverse order – still `Θ(n log n)`.
- **Average case**: For random arrays, it remains `Θ(n log n)`.

Thus merge sort is **Θ(n log n)** for all cases.

---

### 5. Mathematical Expression and Asymptotic Bounds

We derived:

\[
T(n) = k n \log_2 n + c n
\]

where `k` is the constant factor for merging per element, and `c` is the constant for base cases.

**Big O (upper bound)**: For `n ≥ 1`, we can bound `T(n) ≤ (k + c) n \log_2 n` (since `n ≤ n log n` for `n ≥ 2`). So `T(n) = O(n log n)`.

**Omega (lower bound)**: For `n ≥ 2`, `T(n) ≥ k n \log_2 n` (ignoring the linear term). So `T(n) = Ω(n log n)`.

**Theta**: Since both upper and lower bounds are `n log n`, we have `T(n) = Θ(n log n)`.

---

### 6. Space Complexity

Merge sort requires additional space for the temporary arrays during merging. Typically, it uses **O(n)** auxiliary space (if we create new arrays each time) or can be implemented in‑place with O(1) extra space but that is more complex. The standard implementation uses O(n) space.

---

### 7. Summary Table

| Case       | Time Complexity | Space Complexity |
|------------|-----------------|------------------|
| Best       | Θ(n log n)      | O(n)             |
| Worst      | Θ(n log n)      | O(n)             |
| Average    | Θ(n log n)      | O(n)             |

---

### 8. Key Takeaways

- Merge sort always divides the array into two halves, leading to a recursion tree of height `log n`.
- At each level, merging costs linear time `O(n)`.
- Total work = `O(n) * O(log n) = O(n log n)`.
- This complexity holds regardless of input order, making merge sort a reliable, stable sorting algorithm.

The recurrence `T(n) = 2T(n/2) + O(n)` is fundamental and appears in many divide‑and‑conquer algorithms. Understanding its solution is essential for analyzing similar algorithms.