## Table of Contents
1. [Introduction to Recursion and Counting Digits](#introduction)
2. [Mathematical Recurrence Relation](#mathematical-recurrence)
3. [Recursive Algorithm](#recursive-algorithm)
4. [Step‑by‑Step Example: Digits in 12345](#example)
5. [Complexity Analysis](#complexity-analysis)
6. [Handling Negative Numbers and Zero](#handling-edge-cases)
7. [Comparison with Other Approaches](#comparison)
8. [Code Implementation (Python)](#code-implementation)
9. [Conclusion](#conclusion)

---

## 1. Introduction to Recursion and Counting Digits
Counting the number of digits in a given integer is a fundamental problem that can be solved elegantly with recursion. The idea is to repeatedly divide the number by 10 until it becomes a single digit, counting each division as one digit. This process naturally leads to a recursive definition.

## 2. Mathematical Recurrence Relation
Let \(D(n)\) be the number of digits in a non‑negative integer \(n\). We can define:

\[
D(n) =
\begin{cases}
1, & \text{if } 0 \le n < 10 \quad \text{(base case)} \\
1 + D\left(\left\lfloor \frac{n}{10} \right\rfloor\right), & \text{if } n \ge 10 \quad \text{(recursive step)}
\end{cases}
\]

For negative numbers, we can take the absolute value first, as the sign does not affect the digit count (the minus sign is not a digit). The recurrence remains the same after applying \(|n|\).

## 3. Recursive Algorithm
Based on the recurrence, a recursive algorithm in pseudocode:

```
function countDigits(n):
    if -10 < n < 10:          // handles both positive and negative single-digit numbers
        return 1
    else:
        return 1 + countDigits(n // 10)   // integer division (floor) for positive n
```

For negative numbers, integer division in some languages truncates toward zero, so `n // 10` for negative `n` works correctly (e.g., `-123 // 10 = -12`). However, to avoid complexity, we often take the absolute value first.

## 4. Step‑by‑Step Example: Digits in 12345
Trace the execution for \(n = 12345\):

- `countDigits(12345)` → `1 + countDigits(1234)`
- `countDigits(1234)`  → `1 + countDigits(123)`
- `countDigits(123)`   → `1 + countDigits(12)`
- `countDigits(12)`    → `1 + countDigits(1)`
- `countDigits(1)`     → `1` (base case)

Now unwind:

- `countDigits(12)`  = `1 + 1` = 2
- `countDigits(123)` = `1 + 2` = 3
- `countDigits(1234)` = `1 + 3` = 4
- `countDigits(12345)` = `1 + 4` = 5

Thus, 12345 has 5 digits.

## 5. Complexity Analysis
- **Time Complexity:** The function makes one recursive call per digit, i.e., \(\lfloor \log_{10} n \rfloor + 1\) calls. Each call does constant work. Hence time complexity is \(O(\log_{10} n)\).
- **Space Complexity:** Recursion uses the call stack. The depth of recursion equals the number of digits, so space complexity is also \(O(\log_{10} n)\). For very large numbers (e.g., with millions of digits), this could cause a stack overflow in languages with limited recursion depth.

## 6. Handling Negative Numbers and Zero
- **Negative numbers:** The number of digits ignores the sign. For example, `-123` has 3 digits. We can either take the absolute value before recursing or rely on integer division that works with negatives.
- **Zero:** \(0\) is a special case. By the recurrence above, `0` is less than 10 and greater than -10, so it returns 1, which is correct (the digit `0` counts as one digit). Some definitions might consider `0` to have 0 digits, but in most practical contexts, the numeral "0" has one digit. Our algorithm follows the common convention.

## 7. Comparison with Other Approaches
| Approach          | Formula / Method                      | Time Complexity | Space Complexity | Remarks                                    |
|-------------------|---------------------------------------|------------------|-------------------|--------------------------------------------|
| **Recursive**     | \(D(n) = 1 + D(n/10)\)                | \(O(\log n)\)    | \(O(\log n)\)     | Simple, educational, but uses stack.       |
| **Iterative**     | Loop dividing by 10 until zero        | \(O(\log n)\)    | \(O(1)\)          | More efficient, no recursion overhead.     |
| **Mathematical**  | \(D(n) = \lfloor \log_{10} |n| \rfloor + 1\) | \(O(1)\)          | \(O(1)\)          | Fastest, but may have floating‑point issues. |

The recursive method is excellent for illustrating recursion, but for large inputs the iterative or mathematical formula is preferable.

## 8. Code Implementation (Python)
```python
def count_digits(n):
    # Take absolute value to handle negative numbers
    n = abs(n)
    if n < 10:
        return 1
    return 1 + count_digits(n // 10)

# Example usage
numbers = [0, 5, 123, -4567, 100000]
for num in numbers:
    print(f"count_digits({num}) = {count_digits(num)}")
```

**Output:**
```
count_digits(0) = 1
count_digits(5) = 1
count_digits(123) = 3
count_digits(-4567) = 4
count_digits(100000) = 6
```

**Note:** Python’s recursion limit is typically around 1000, so this function works for numbers with up to about 1000 digits. Beyond that, consider using an iterative approach.

## 9. Conclusion
Counting digits recursively demonstrates a classic “divide and conquer” pattern: reduce the problem size by one digit each step until a trivial base case is reached. The recurrence relation \(D(n) = 1 + D(n/10)\) captures the essence of the problem. While simple, this example reinforces key recursion concepts—base case, recursive step, and stack unwinding—and prepares you for more advanced recursive algorithms.