## Quantifying Time Complexity Step by Step

When we talk about time complexity, we want a **mathematical description** of how the number of operations an algorithm performs grows as the input size (`n`) becomes very large. We’re not interested in exact counts, because those depend on tiny details that don’t matter in the long run. Instead, we focus on the **dominant term** – the one that grows fastest – and ignore everything else.

Let’s break down the key ideas and then see them in action with actual code.

---

### The Three Golden Rules

1. **Count operations, not seconds**  
   We measure time by counting how many **basic operations** (comparisons, assignments, arithmetic, etc.) the algorithm performs. This makes our analysis independent of machine speed, programming language, and other real‑world factors.

2. **Focus on the highest‑power term**  
   When `n` is huge, the term with the largest exponent (or fastest growth) completely overshadows all others. For example, if an algorithm does `n² + 100n + 500` operations, for `n = 1,000,000` the `n²` part is about 10¹², while the `100n` part is only 10⁸ – it’s like a drop in the ocean. So we say the complexity is **O(n²)**.

3. **Ignore constant coefficients**  
   Whether it’s `5n²` or `1000n²`, the growth rate is still **n²**. Constants are just scaling factors; they don’t change the shape of the growth curve. So we drop them too.

> **Important:** These rules are what make **asymptotic analysis** (Big O notation) so powerful. They let us compare algorithms at a high level without getting lost in small details.

---

### Why n² + n ≈ n² for Large n

Let’s take a concrete example. Suppose an algorithm performs:
- `n²` operations from nested loops
- `n` operations from a single loop
- 100 constant‑time operations

Total operations = `n² + n + 100`.

Now plug in some numbers:

| n   | n²     | n   | 100 | Total   | n² dominates? |
|-----|--------|-----|-----|---------|---------------|
| 10  | 100    | 10  | 100 | 210     | 100 is 48% – not yet |
| 100 | 10,000 | 100 | 100 | 10,200  | 10,000 is 98% |
| 1000| 1,000,000|1000|100|1,001,100| > 99.9% |

As `n` grows, the `n²` term becomes almost the entire total. The smaller terms are negligible. Therefore we say the complexity is **O(n²)**.

---

### Worst‑Case Matters Most

When we analyze complexity, we usually consider the **worst case** – the input that causes the algorithm to do the most work. This gives us an upper bound that we can rely on. For example:

- **Bubble sort** on a reversed array does the maximum number of swaps and comparisons → O(n²).
- **Merge sort** always does the same amount of work regardless of input order → O(n log n).

Sometimes we also talk about best or average case, but worst‑case is the most common guarantee.

---

### Step‑by‑Step: From Code to Complexity

Let’s see how to derive the complexity expression from actual code. We’ll count **unit operations** (like comparisons, assignments) and then simplify.

#### Example 1: Bubble Sort

Here’s a typical implementation:

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n-1):                 # outer loop runs n-1 times
        for j in range(n-1-i):            # inner loop runs (n-1-i) times
            if arr[j] > arr[j+1]:         # 1 comparison
                arr[j], arr[j+1] = arr[j+1], arr[j]  # swap = 3 assignments (if we count carefully)
```

**Counting operations (simplified):**  
We’ll focus on comparisons because they are the dominant operation in sorting.

- The outer loop runs `n-1` times.
- For each outer iteration `i`, the inner loop runs `n-1-i` times.
- So total comparisons =  
  `(n-1) + (n-2) + ... + 1 = n(n-1)/2`  
  = `n²/2 - n/2`.

Now apply the rules:
- Highest power: `n²`
- Ignore coefficient `1/2`
- Ignore lower term `-n/2`

Thus, bubble sort is **O(n²)**.

If we also counted swaps, they’d be of the same order, so the overall complexity remains O(n²).

#### Example 2: Merge Sort

Merge sort is a classic divide‑and‑conquer algorithm. Here’s a simplified recursive version:

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])     # recursive call on left half
    right = merge_sort(arr[mid:])    # recursive call on right half
    return merge(left, right)        # merge the two sorted halves

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):   # each comparison leads to one element being placed
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    # add remaining elements (no more comparisons needed)
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

**Counting operations:**  
We need to express the total number of comparisons as a function of `n`. Let `T(n)` be the number of comparisons for an array of size `n`.

- If `n ≤ 1`, `T(n) = 0` (no comparisons).
- Otherwise, we split into two halves of size `n/2` each. Sorting each half recursively takes `T(n/2)` comparisons.
- Then merging two sorted halves of total size `n` takes at most `n-1` comparisons (because each comparison places one element, and you stop when one list is empty).

So the recurrence is:  
`T(n) = 2 * T(n/2) + (n-1)`

We can solve this (using the Master Theorem or by expanding). For simplicity, let's expand:

`T(n) = 2 * T(n/2) + n` (approximately, ignoring the -1 for large n)
      = 2 * [2 * T(n/4) + n/2] + n = 4 * T(n/4) + n + n
      = 8 * T(n/8) + 3n
      ... after k steps, `T(n) = 2^k * T(n/2^k) + k*n`.

We stop when `n/2^k = 1` → `k = log₂ n`. Then `T(1) = 0`, so:

`T(n) = n * log₂ n`.

Thus merge sort does about **n log n** comparisons.  
Applying our rules: highest power (actually fastest growth among common functions) is `n log n`, which is between linear and quadratic. So we say **O(n log n)**.

Notice that the coefficient (here it’s 1) and any lower terms (like the -1) are ignored.

---

### Nested Loops vs. Single Loops

A simple rule of thumb:

- A single loop over `n` elements → usually O(n)
- Two nested loops (each about `n`) → usually O(n²)
- Three nested loops → O(n³)

But be careful: sometimes loops don’t run exactly `n` times each (like bubble sort’s inner loop shrinks). Still, the highest power remains `n²`.

Also, loops that cut the problem in half each time (like binary search) lead to O(log n).

---

### Why 5n² <<< 5n³ for Large n

Even though both have coefficients of 5, `n³` grows much faster than `n²`. For `n = 100`:

- 5n² = 5 × 10,000 = 50,000 operations
- 5n³ = 5 × 1,000,000 = 5,000,000 operations – 100 times more!

As `n` increases, the gap becomes enormous. That’s why we care about the **order of growth** more than any constant factor.

---

### Summary: Three Major Takeaways

1. **Talk in terms of number of operations, not time.**  
   This makes the analysis universal and independent of hardware.

2. **Focus on the highest‑power term.**  
   For large inputs, only the fastest‑growing part matters; everything else is noise.

3. **Don’t care about coefficients.**  
   Whether it’s `n²/2` or `1000n²`, the growth shape is the same – quadratic.

By following these rules, we can quickly compare algorithms: an O(n log n) algorithm will always beat an O(n²) one for sufficiently large `n`, no matter how small the constants in the O(n²) implementation are. This is the power of asymptotic analysis.