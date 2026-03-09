# First and Last Index of an Element in an Array Using Recursion

## Table of Contents
1. [Definition and Concept Overview](#definition-and-concept-overview)  
2. [Core Principles and Internal Mechanics](#core-principles-and-internal-mechanics)  
3. [Step-by-Step Logical Breakdown](#step-by-step-logical-breakdown)  
   - [First Index – Recursive Relation and Tree](#first-index--recursive-relation-and-tree)  
   - [Last Index – Recursive Relation and Tree](#last-index--recursive-relation-and-tree)  
4. [Implementation (Python)](#implementation-python)  
   - [First Index of Element](#first-index-of-element)  
   - [Last Index of Element](#last-index-of-element)  
5. [Time and Space Complexity Analysis](#time-and-space-complexity-analysis)  
6. [Edge Cases and Failure Scenarios](#edge-cases-and-failure-scenarios)  
7. [Practical Use Cases](#practical-use-cases)  
8. [Limitations and Trade-offs](#limitations-and-trade-offs)  

---

## 1. Definition and Concept Overview

Given an array (list) and a target element `x`, the task is to find:

- **First index**: The smallest index `i` such that `arr[i] == x`. If `x` is not present, return -1.  
- **Last index**: The largest index `i` such that `arr[i] == x`. If `x` is not present, return -1.

Recursive solutions operate by reducing the problem size, examining one element at a time, and combining results appropriately. Two distinct strategies are required because:

- For the **first index**, the search proceeds from left to right; the first occurrence found must be reported with its original index.  
- For the **last index**, the search must examine the entire array to ensure the rightmost occurrence is identified; recursion can be structured to check the current element after exploring the rest.

The challenge lies in **preserving the original index** as the array is sliced. Slicing removes elements from the front, so the index in the current slice must be translated to the original index by adding the number of skipped elements.

---

## 2. Core Principles and Internal Mechanics

### Recursive Decomposition
Let `first(arr, x)` return the first index of `x` in `arr`.

- Base case: if `arr` is empty → return -1.
- If the first element equals `x` → return 0 (index in the current slice).
- Otherwise, recursively search in `arr[1:]`. Let `result` be the value returned from the recursion.
  - If `result == -1`, propagate -1.
  - Else, return `result + 1` (adjusting for the removed first element).

For `last(arr, x)`:

- Base case: if `arr` is empty → return -1.
- First, recursively search in `arr[1:]` to get the last index in the tail. Let `result` be that value.
- Then check the current first element:
  - If `result != -1`, it means `x` exists in the tail; that index (adjusted) is later than current position, so return `result + 1`.
  - If `result == -1` and `arr[0] == x`, then the first element is the only occurrence (or the last occurrence because no later one exists) → return 0.
  - Otherwise, return -1.

Alternatively, a **right‑to‑left** recursive approach (reducing from the end) can be used, but the above method processes from left to right, relying on the post‑recursion check to determine if a later occurrence exists.

### Index Translation
Because each recursive call slices off the first element, the index returned from a subcall refers to a position within the smaller slice. To convert it to the original array index, add the number of elements removed so far. In the implementation, this is done by adding 1 each time the recursion unwinds and a valid result is passed upward.

---

## 3. Step-by-Step Logical Breakdown

### First Index – Recursive Relation and Tree

**Recurrence**:
- `first([], x) = -1`
- `first(arr, x) = 0` if `arr[0] == x`
- else: `let r = first(arr[1:], x); return -1 if r == -1 else r + 1`

**Example**: Find first index of `2` in `[3,2,5,2]`.

```
Call: first([3,2,5,2], 2)
├─ arr[0]=3 ≠2 → call first([2,5,2], 2)
│  ├─ arr[0]=2 ==2 → return 0
│  └─ result = 0, not -1 → return 0+1 = 1
└─ result = 1, not -1 → return 1+1 = 2
```

**Recursion tree (ASCII)**:
```
first([3,2,5,2], 2)
│
└─ first([2,5,2], 2)
   │
   └─ (arr[0]==2) → returns 0
      ↓
   returns 0+1 = 1
   ↓
returns 1+1 = 2
```

The final answer is `2`.

**Important**: The base case returning `0` when the first element matches is correct because at that recursion level, the matching element is at position 0 of the current slice. The addition of 1 during unwinding reconstructs the original index.

### Last Index – Recursive Relation and Tree

**Recurrence**:
- `last([], x) = -1`
- `let r = last(arr[1:], x)`
- if `r != -1` → `x` exists later; return `r + 1`
- else if `arr[0] == x` → this is the only/last occurrence; return `0`
- else return `-1`

**Example**: Find last index of `2` in `[3,2,5,2]`.

```
Call: last([3,2,5,2], 2)
├─ call last([2,5,2], 2)
│  ├─ call last([5,2], 2)
│  │  ├─ call last([2], 2)
│  │  │  ├─ call last([], 2) → returns -1
│  │  │  ├─ r = -1, arr[0]==2 → return 0
│  │  │  └─ returns 0
│  │  ├─ r = 0 (from child) → since r != -1, return 0+1 = 1
│  │  └─ returns 1
│  ├─ r = 1 → return 1+1 = 2
│  └─ returns 2
├─ r = 2 → return 2+1 = 3
└─ returns 3
```

**Recursion tree (ASCII)**:
```
last([3,2,5,2], 2)
│
└─ last([2,5,2], 2)
   │
   └─ last([5,2], 2)
      │
      └─ last([2], 2)
         │
         └─ last([], 2) → -1
            ↓
         arr[0]==2 → returns 0
         ↓
      r=0 → returns 1
      ↓
   r=1 → returns 2
   ↓
r=2 → returns 3
```

The final answer is `3`.

**Observation**: The recursion always goes to the base case (empty list) before any checking, ensuring that the last occurrence is identified. The addition of 1 during unwinding accumulates the offset.

---

## 4. Implementation (Python)

### First Index of Element

```python
def first_index(arr, x):
    """
    Returns the first index of x in arr, or -1 if not found.
    Uses recursion with slice and index adjustment.
    """
    # Base case: empty list
    if len(arr) == 0:
        return -1

    # If first element matches, it is the first occurrence in this slice
    if arr[0] == x:
        return 0

    # Recursively search in the tail
    result = first_index(arr[1:], x)

    # If not found in tail, propagate -1; otherwise adjust index
    if result == -1:
        return -1
    else:
        return result + 1
```

### Last Index of Element

```python
def last_index(arr, x):
    """
    Returns the last index of x in arr, or -1 if not found.
    Uses recursion to check the tail first, then the current element.
    """
    # Base case: empty list
    if len(arr) == 0:
        return -1

    # Recursively find last index in the tail
    result = last_index(arr[1:], x)

    # If x exists in the tail, that index is later (after adjustment)
    if result != -1:
        return result + 1

    # Otherwise, check if current element is x
    if arr[0] == x:
        return 0

    # Not found anywhere
    return -1
```

**Example usage:**
```python
arr = [3, 2, 5, 2, 8, 2, 1]
print(first_index(arr, 2))   # Output: 1
print(first_index(arr, 10))  # Output: -1
print(first_index(arr, 1))   # Output: 6

print(last_index(arr, 2))    # Output: 5
print(last_index(arr, 10))   # Output: -1
print(last_index(arr, 3))    # Output: 0
```

---

## 5. Time and Space Complexity Analysis

| Operation          | Time Complexity | Space Complexity (stack) | Copying Overhead |
|---------------------|-----------------|---------------------------|------------------|
| First index         | O(n)            | O(n)                      | O(n²)            |
| Last index          | O(n)            | O(n)                      | O(n²)            |

- **Time**: Both functions examine each element at most once. Each recursive call processes one element and performs constant work (comparisons and additions).  
- **Stack space**: Python allocates a stack frame per recursive call. Depth = n+1 → O(n).  
- **Copying overhead**: Using slicing (`arr[1:]`) creates a new list each call, copying O(n) elements per level, totaling O(n²) copied elements. This is the dominant inefficiency.

A more efficient recursive implementation would use **index bounds** to avoid copying (see Limitations).

---

## 6. Edge Cases and Failure Scenarios

- **Empty list**: Returns -1 correctly.  
- **Element not present**: Returns -1 after full traversal.  
- **Element appears once**: First and last indices are the same.  
- **Element appears multiple times**: First index returns the leftmost; last index returns the rightmost.  
- **Very long list**: For `n > ~1000`, `RecursionError` occurs due to Python’s recursion limit.  
- **Element at the beginning**: First index returns 0; last index returns after full recursion.  
- **Element at the end**: First index must traverse entire list; last index returns n-1 after full recursion.  
- **List with non‑comparable types**: Comparison `arr[0] == x` works for any type, but if `x` is of a different type, it may return `False` without error (e.g., `2 == "2"` is `False`).

---

## 7. Practical Use Cases

- **Search operations** in small, unsorted lists where recursion is used for pedagogical purposes.  
- **Implementing recursive `indexOf` and `lastIndexOf`** in functional programming contexts.  
- **Building blocks for more complex recursive algorithms** (e.g., searching in nested structures).  
- **Educational tool** to demonstrate recursion, stack unwinding, and index adjustment techniques.

In production, iterative linear search or built‑in `list.index()` (for first occurrence) is preferred. For last occurrence, a reverse loop is simple and efficient.

---

## 8. Limitations and Trade-offs

- **Recursion depth**: Python’s recursion limit (typically 1000) limits input size.  
- **Slicing overhead**: O(n²) copying makes the implementation impractical for large lists.  
- **Lack of tail‑call optimization**: Even if the last index algorithm could be written tail‑recursively, Python does not optimize it, so stack depth remains O(n).  
- **Readability**: The index adjustment logic (`result + 1`) may be confusing to beginners.  
- **Alternative**: An **index‑bound** recursive version eliminates copying and is the correct way to implement recursion on arrays:

```python
def first_index_bounds(arr, x, start=0):
    if start >= len(arr):
        return -1
    if arr[start] == x:
        return start
    return first_index_bounds(arr, x, start + 1)

def last_index_bounds(arr, x, start=0):
    if start >= len(arr):
        return -1
    result = last_index_bounds(arr, x, start + 1)
    if result != -1:
        return result
    if arr[start] == x:
        return start
    return -1
```

These versions use O(n) stack space but **no copying**, making them suitable for educational recursion when input size is within recursion limits.

Despite these limitations, the slice‑based versions clearly illustrate the core ideas of recursion and index translation. They are valuable for learning but should be replaced with iterative or index‑bound recursion in any performance‑sensitive context.