# Checking if an Array is Sorted Using Recursion

## Table of Contents
1. [Definition and Concept Overview](#definition-and-concept-overview)  
2. [Core Principles and Internal Mechanics](#core-principles-and-internal-mechanics)  
3. [Step-by-Step Logical Breakdown](#step-by-step-logical-breakdown)  
   - [Algorithm 1: Head Recursion (Unconditional Recursive Call)](#algorithm-1-head-recursion-unconditional-recursive-call)  
   - [Algorithm 2: Tail Recursion with Early Exit](#algorithm-2-tail-recursion-with-early-exit)  
   - [Recursion Tree Comparison](#recursion-tree-comparison)  
4. [Implementation (Python)](#implementation-python)  
   - [Version 1: Head Recursion](#version-1-head-recursion)  
   - [Version 2: Tail Recursion with Early Exit](#version-2-tail-recursion-with-early-exit)  
5. [Time and Space Complexity Analysis](#time-and-space-complexity-analysis)  
6. [Edge Cases and Failure Scenarios](#edge-cases-and-failure-scenarios)  
7. [Practical Use Cases](#practical-use-cases)  
8. [Limitations and Trade-offs](#limitations-and-trade-offs)  

---

## 1. Definition and Concept Overview

Determining whether an array (or list) is sorted in non‑decreasing order is a fundamental verification task. A recursive solution reduces the problem by checking the order of the first two elements and then recursively verifying the remainder of the array.

Two distinct recursive formulations are examined:

- **Head recursion**: The recursive call is made unconditionally before any comparison. The result from the recursive call is later combined with the comparison of the current pair.  
- **Tail recursion with early exit**: The comparison is performed first. If the current pair violates the sorted order, the function returns `False` immediately without further recursion. Only if the pair is in order does it proceed with the recursive call.

The key difference lies in **when the recursive call occurs** and whether the function can terminate early upon detecting a violation.

---

## 2. Core Principles and Internal Mechanics

### Recursive Decomposition
A list of length `n` is sorted if:

1. The first element is less than or equal to the second element (for non‑decreasing order), and  
2. The sublist starting from the second element is also sorted.

This yields the recurrence:

- Base case: A list of length 0 or 1 is trivially sorted.  
- Recursive case: `is_sorted(lst) = (lst[0] <= lst[1]) and is_sorted(lst[1:])`

### Head vs. Tail in This Context
- **Head recursion** version: The recursive call `is_sorted(lst[1:])` is evaluated first, its result stored, and then combined with the comparison using `and`. This forces the recursion to proceed to the base case regardless of whether an early violation exists.  
- **Tail recursion** version: The comparison is evaluated first. If it fails, the function returns `False`; otherwise, it returns the result of the recursive call (which is the final operation). This allows early termination.

### Early Exit Benefit
For a large unsorted list, the tail version stops at the first out‑of‑order pair, avoiding deep recursion. The head version always traverses the entire list, leading to maximum recursion depth even when the list is unsorted at the very beginning.

---

## 3. Step-by-Step Logical Breakdown

### Algorithm 1: Head Recursion (Unconditional Recursive Call)

**Pseudocode:**
```
function is_sorted_head(lst):
    if length(lst) <= 1:
        return True
    ans = is_sorted_head(lst[1:])          # recursive call first
    if lst[0] <= lst[1]:
        return ans
    else:
        return False
```

**Execution Trace for `[2, 1, 3]` (unsorted at first pair):**
1. Call `is_sorted_head([2,1,3])`  
   - Length > 1 → call `is_sorted_head([1,3])` and wait.  
2. Call `is_sorted_head([1,3])`  
   - Length > 1 → call `is_sorted_head([3])` and wait.  
3. Call `is_sorted_head([3])`  
   - Length == 1 → return `True` to caller ([1,3]).  
4. Resume `is_sorted_head([1,3])`:  
   - Compare 1 <= 3 → True, return `ans` (which is `True`).  
5. Resume `is_sorted_head([2,1,3])`:  
   - Compare 2 <= 1 → False, return `False`.  

Even though the first pair is out of order, the recursion still descends to the base case.

### Algorithm 2: Tail Recursion with Early Exit

**Pseudocode:**
```
function is_sorted_tail(lst):
    if length(lst) <= 1:
        return True
    if lst[0] > lst[1]:                     # violation check first
        return False
    return is_sorted_tail(lst[1:])           # recursive call only if needed
```

**Execution Trace for `[2, 1, 3]`:**
1. Call `is_sorted_tail([2,1,3])`  
   - Length > 1 → check 2 > 1 → True → return `False` immediately.  
   No further recursion occurs.

### Recursion Tree Comparison

**Head recursion tree for `[2,1,3]`:**  
```
is_sorted_head([2,1,3])
├─ is_sorted_head([1,3])
│  ├─ is_sorted_head([3]) → True
│  └─ returns True
└─ returns False
```
Depth = 3 (length of list).

**Tail recursion tree for same input:**  
```
is_sorted_tail([2,1,3]) → returns False (no children)
```
Depth = 1.

For a completely sorted list of length `n`, both versions produce a recursion tree of depth `n` because every pair must be checked.

---

## 4. Implementation (Python)

### Version 1: Head Recursion

```python
def is_sorted_head(lst):
    """
    Checks if lst is sorted in non-decreasing order using head recursion.
    The recursive call is made unconditionally before the comparison.
    """
    # Base case: empty or single-element list is sorted
    if len(lst) <= 1:
        return True

    # Recursive call on the tail (creates a new list slice)
    tail_sorted = is_sorted_head(lst[1:])

    # Now check the first pair
    if lst[0] <= lst[1]:
        return tail_sorted
    else:
        return False
```

**Usage:**
```python
print(is_sorted_head([1, 2, 3]))    # True
print(is_sorted_head([2, 1, 3]))    # False
```

### Version 2: Tail Recursion with Early Exit

```python
def is_sorted_tail(lst):
    """
    Checks if lst is sorted using tail recursion with early exit.
    The comparison is performed first; recursion only proceeds if the
    current pair is in order.
    """
    if len(lst) <= 1:
        return True

    # Immediate violation check
    if lst[0] > lst[1]:
        return False

    # Tail-recursive call (last operation)
    return is_sorted_tail(lst[1:])
```

**Usage:**
```python
print(is_sorted_tail([1, 2, 3]))    # True
print(is_sorted_tail([2, 1, 3]))    # False
```

**Demonstrating the benefit for a large unsorted list:**

```python
# Create a descending list of 10,000 elements (strictly decreasing)
large_descending = [i for i in range(10000, 1, -1)]

# is_sorted_head would raise RecursionError due to depth
# is_sorted_tail returns False immediately without deep recursion
print(is_sorted_tail(large_descending))  # False (no error)
```

---

## 5. Time and Space Complexity Analysis

| Version            | Time Complexity (worst case) | Space Complexity (stack) | Early Exit Benefit          |
|---------------------|-------------------------------|---------------------------|------------------------------|
| Head Recursion      | O(n)                          | O(n)                      | None – always full depth     |
| Tail with Early Exit| O(n)                          | O(n) worst case (sorted)  | O(1) depth on first violation|

- **Time**: Both versions examine each element at most once. The head version always visits every element; the tail version stops early if a violation is found. Worst‑case (sorted list) both are O(n).  
- **Space**: Both use O(n) stack frames in the worst case because Python does not optimize tail recursion. However, the tail version can terminate after O(1) stack usage if an early violation occurs.  
- **Copying overhead**: Both versions use slicing (`lst[1:]`) which creates a new list of size n‑1, n‑2, … leading to O(n²) total copied elements. This is a separate inefficiency discussed in Section 8.

---

## 6. Edge Cases and Failure Scenarios

- **Empty list (`[]`)** – both return `True` (vacuously sorted).  
- **Single element (`[x]`)** – both return `True`.  
- **List with equal elements** – e.g., `[2,2,1]`. The condition `lst[0] > lst[1]` catches violation only if strict decrease is considered unsorted. For non‑decreasing order, equal elements are allowed. The code uses `<=` in head version and `>` in tail version; both correctly treat equal as sorted.  
- **Very large sorted list** – both will raise `RecursionError` when length exceeds Python’s recursion limit (typically ~1000). The early exit does not help in the sorted case.  
- **Non‑list input** – the functions assume a sequence with `len()` and indexing; passing a non‑sequence will raise `TypeError`.  

---

## 7. Practical Use Cases

- **Validation** – ensuring data meets preconditions before processing (e.g., binary search requires sorted input).  
- **Debugging** – quick checks during development.  
- **Educational** – illustrating recursion patterns and the importance of early termination.  
- **Small datasets** – recursion depth is not an issue for n < 1000.  

In production, an iterative loop is preferred for its constant space and no recursion limit:

```python
def is_sorted_iterative(lst):
    return all(lst[i] <= lst[i+1] for i in range(len(lst)-1))
```

---

## 8. Limitations and Trade-offs

- **Recursion depth limit**: Python’s default recursion limit makes any recursive solution impractical for lists longer than about 1000 elements. The tail version mitigates this only when the list is unsorted early.  
- **Slicing overhead**: Both versions create new lists via `lst[1:]`, copying O(n²) elements total. This is extremely inefficient for large lists. A better recursive design uses **index bounds** to avoid copying (see example below).  
- **Tail call optimization absence**: Even though `is_sorted_tail` is tail‑recursive conceptually, Python does not eliminate the stack frames, so space complexity remains O(n) for sorted input.  
- **Readability**: The iterative version is clearer and more efficient for this problem. Recursion is used here for pedagogical purposes.

### Improved Recursive Version with Index Bounds (No Copying)

```python
def is_sorted_bounds(lst, start=0):
    if start >= len(lst) - 1:
        return True
    if lst[start] > lst[start + 1]:
        return False
    return is_sorted_bounds(lst, start + 1)
```

This version uses O(n) stack space (still depth n) but avoids O(n²) copying. It is the preferred recursive form when recursion is required.

**Comparison summary:**

| Version                | Stack Depth | Copying Overhead | Early Exit |
|------------------------|-------------|------------------|------------|
| Head + slicing         | O(n)        | O(n²)            | No         |
| Tail + slicing         | O(n)        | O(n²)            | Yes (if unsorted) |
| Index bounds (tail)    | O(n)        | O(1)             | Yes (if unsorted) |

The simple change from head to tail recursion (with early exit) improves behavior for unsorted inputs but does not solve the fundamental issues of recursion depth and copying. The index‑bound variant is the correct way to implement recursive array algorithms in Python.