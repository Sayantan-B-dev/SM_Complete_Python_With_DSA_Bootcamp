## Table of Contents
1. [Introduction to Recursion and the Sum of n Numbers](#introduction)
2. [Mathematical Recurrence Relation](#mathematical-recurrence)
3. [Recursive Algorithm](#recursive-algorithm)
4. [Step‑by‑Step Example: Sum of First 5 Numbers](#example)
5. [Complexity Analysis](#complexity-analysis)
6. [Comparison with Other Approaches](#comparison)
7. [Code Implementation (Python)](#code-implementation)
8. [Conclusion](#conclusion)

---

## 1. Introduction to Recursion and the Sum of n Numbers
Recursion is a programming technique where a function calls itself to solve a smaller instance of the same problem. It is especially useful for problems that can be defined in terms of themselves. The sum of the first \(n\) natural numbers, often denoted as \(S(n) = 1 + 2 + \dots + n\), is a classic example that can be expressed recursively.

## 2. Mathematical Recurrence Relation
We can define the sum recursively as:

\[
S(n) =
\begin{cases}
1, & \text{if } n = 1 \quad \text{(base case)} \\
n + S(n-1), & \text{if } n > 1 \quad \text{(recursive step)}
\end{cases}
\]

This recurrence relation captures the idea: the sum of the first \(n\) numbers equals \(n\) plus the sum of the first \(n-1\) numbers.

## 3. Recursive Algorithm
Based on the recurrence, a recursive algorithm can be written in pseudocode:

```
function sum(n):
    if n == 1:
        return 1
    else:
        return n + sum(n - 1)
```

The function checks for the base case (\(n = 1\)) and returns 1. Otherwise, it calls itself with the argument \(n-1\) and adds \(n\) to the result.

## 4. Step‑by‑Step Example: Sum of First 5 Numbers
Let’s trace the execution for \(n = 5\):

- `sum(5)` returns `5 + sum(4)`
- `sum(4)` returns `4 + sum(3)`
- `sum(3)` returns `3 + sum(2)`
- `sum(2)` returns `2 + sum(1)`
- `sum(1)` returns `1` (base case)

Now the results unwind:

- `sum(2)` = `2 + 1` = 3
- `sum(3)` = `3 + 3` = 6
- `sum(4)` = `4 + 6` = 10
- `sum(5)` = `5 + 10` = 15

Thus, \(S(5) = 15\), which matches the formula \(\frac{n(n+1)}{2}\).

## 5. Complexity Analysis
- **Time Complexity:** The function makes exactly \(n\) recursive calls, each performing a constant-time addition. Hence, the time complexity is \(O(n)\).
- **Space Complexity:** Recursion uses the call stack. For \(n\) calls, the stack depth is \(n\), so the space complexity is also \(O(n)\). For very large \(n\), this can lead to a stack overflow.

## 6. Comparison with Other Approaches
| Approach          | Formula                          | Time Complexity | Space Complexity | Remarks                                  |
|-------------------|----------------------------------|------------------|-------------------|------------------------------------------|
| **Recursive**     | \(S(n) = n + S(n-1)\)            | \(O(n)\)         | \(O(n)\)          | Simple, but uses stack.                  |
| **Iterative**     | Loop from 1 to n, accumulating   | \(O(n)\)         | \(O(1)\)          | Efficient and avoids recursion overhead. |
| **Mathematical**  | \(S(n) = \frac{n(n+1)}{2}\)      | \(O(1)\)         | \(O(1)\)          | Direct formula, most efficient.          |

The recursive approach is primarily educational; for practical use, the direct formula is best.

## 7. Code Implementation (Python)
Here is a simple Python implementation of the recursive sum:

```python
def recursive_sum(n):
    if n == 1:
        return 1
    return n + recursive_sum(n - 1)

# Example usage
n = 5
print(f"Sum of first {n} numbers is: {recursive_sum(n)}")
```

**Output:**
```
Sum of first 5 numbers is: 15
```

**Note:** For large \(n\), Python’s recursion limit may be exceeded. You can increase it with `sys.setrecursionlimit()`, but it’s safer to use the iterative or formula approach for large inputs.

## 8. Conclusion
The recursive approach to summing the first \(n\) natural numbers beautifully illustrates the core concepts of recursion: a base case stops the recursion, and a recursive step breaks the problem into a smaller one. While not the most efficient method for this particular problem, it serves as an excellent introduction to recursive thinking and recurrence relations. Understanding this simple example paves the way for tackling more complex recursive problems.