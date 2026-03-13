## Time Complexity of Binary Search – Iterative and Recursive Approaches

Binary search is an efficient algorithm for finding a target value in a **sorted** array. It works by repeatedly dividing the search interval in half. Both iterative and recursive implementations have the same logarithmic time complexity. Let's derive it step by step.

---

### 1. Binary Search Algorithm

#### Iterative Version
```python
def binary_search_iterative(arr, target):
    left = 0
    right = len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

#### Recursive Version
```python
def binary_search_recursive(arr, left, right, target):
    if left > right:
        return -1
    mid = (left + right) // 2
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search_recursive(arr, mid + 1, right, target)
    else:
        return binary_search_recursive(arr, left, mid - 1, target)
```

---

### 2. Analyzing the Recursive Approach

Let `T(n)` be the time complexity (number of operations) for an array of size `n` (where `n = right - left + 1`).

**Recurrence relation:**
- Base case: If the array is empty (`left > right`), we do constant work → `T(0) = O(1)`.
- For `n > 0`, we:
  - Compute the middle index (constant time)
  - Compare the target with `arr[mid]` (constant time)
  - Make **one recursive call** on a subarray of size **at most** `n/2` (since we discard half)
  - Thus: `T(n) = T(n/2) + O(1)`

More precisely, we can write:
\[
T(n) = T\left(\frac{n}{2}\right) + k \quad \text{for } n \ge 1,\quad T(0) = c
\]
where `k` and `c` are constants representing the work per call.

#### Solving the Recurrence

We can solve by **iteration** (unfolding):

\[
\begin{align*}
T(n) &= T\left(\frac{n}{2}\right) + k \\
     &= \left[T\left(\frac{n}{4}\right) + k\right] + k = T\left(\frac{n}{4}\right) + 2k \\
     &= T\left(\frac{n}{8}\right) + 3k \\
     &\;\;\vdots \\
     &= T\left(\frac{n}{2^x}\right) + x \cdot k
\end{align*}
\]

We stop when the subproblem size becomes 1 or 0. That is, when \( \frac{n}{2^x} \le 1 \). The smallest `x` satisfying this is \( x = \lceil \log_2 n \rceil \). For large `n`, we can approximate \( x = \log_2 n \). Then:

\[
T(n) = T(1) + (\log_2 n) \cdot k
\]

Both `T(1)` and `k` are constants, so:

\[
T(n) = O(\log n)
\]

#### Using the Master Theorem
The recurrence \( T(n) = T(n/2) + O(1) \) fits the form \( T(n) = aT(n/b) + f(n) \) with \( a = 1, b = 2, f(n) = O(1) \). Then \( n^{\log_b a} = n^{\log_2 1} = n^0 = 1 \). Since \( f(n) = O(1) = \Theta(n^0) \), we are in **Case 2** of the Master Theorem, giving \( T(n) = O(\log n) \).

---

### 3. Analyzing the Iterative Approach

The iterative version does not use recursion, but the number of steps is determined by how many times the `while` loop runs. Each iteration reduces the search interval by half. Starting with `n` elements, after each iteration the interval size becomes roughly `n/2, n/4, n/8, ...` until it becomes 0 (or 1). The number of iterations `x` satisfies:

\[
\frac{n}{2^x} \approx 1 \quad \Rightarrow \quad x \approx \log_2 n
\]

More precisely, the loop runs until `left > right`, which happens after at most \(\lfloor \log_2 n \rfloor + 1\) iterations. Each iteration does constant work (comparison, index update), so total time is \( O(\log n) \).

---

### 4. Step-by-Step Visualization

Consider a sorted array of 16 elements. We want to find the target 42.

**Initial state:**  
Indices: 0 ... 15, left = 0, right = 15, mid = 7.

We compare arr[7] with 42. If arr[7] < 42, we discard left half (0..7) and set left = 8. Now the search space is indices 8..15 (size 8).

**Step 1:** left=8, right=15, mid = (8+15)//2 = 11. Compare. If arr[11] > 42, discard right half, set right = 10. New search space 8..10 (size 3).

**Step 2:** left=8, right=10, mid = 9. Compare. If arr[9] == 42, done. If not, we may need one more step.

After at most 4 steps (since log2(16)=4), we either find the target or determine it's absent.

This halving process is why the complexity is logarithmic.

---

### 5. Mathematical Derivation of Number of Steps

Let the initial number of elements be `n`. After each step, the number of elements reduces to at most `floor(n/2)` or `ceil(n/2)`. In the worst case, we continue until only one element remains (or none). The number of steps `x` satisfies:

\[
\frac{n}{2^x} \le 1 \quad \Rightarrow \quad 2^x \ge n \quad \Rightarrow \quad x \ge \log_2 n
\]

Since `x` is an integer, the maximum number of steps is \( \lceil \log_2 n \rceil \). Thus the time complexity is proportional to \( \log_2 n \).

In Big O notation, we drop the base of the logarithm because all logarithms are related by a constant factor: \( \log_a n = \frac{\log_b n}{\log_b a} \). So we simply write \( O(\log n) \).

---

### 6. Best, Worst, and Average Cases

- **Best case:** The target is exactly at the middle of the array on the first comparison → O(1).
- **Worst case:** The target is not present, or present at one of the extremes after many steps → O(log n).
- **Average case:** For a random target, the expected number of comparisons is also O(log n) (actually about log2 n - 1).

Thus binary search is **Θ(log n)** in the worst and average cases.

---

### 7. Common Misconception

The user's note "k.log=(n+1)logn" is likely a miswriting. The correct expression is that the total time is \( k \cdot \log_2 n + \text{constant} \), where `k` is the work per step. So it is \( O(\log n) \), not linear or quadratic.

---

### 8. Summary

| Approach   | Recurrence/Iteration Count | Complexity |
|------------|----------------------------|------------|
| Recursive  | T(n) = T(n/2) + O(1)       | O(log n)   |
| Iterative  | Loop runs ≈ log₂ n times   | O(log n)   |

