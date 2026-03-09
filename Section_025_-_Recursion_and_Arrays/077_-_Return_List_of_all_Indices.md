# Returning All Indices as a New List Using Recursion

## Table of Contents
1. [Definition and Concept Overview](#definition-and-concept-overview)  
2. [Core Principles and Internal Mechanics](#core-principles-and-internal-mechanics)  
3. [Step-by-Step Logical Breakdown](#step-by-step-logical-breakdown)  
   - [Recurrence Relation](#recurrence-relation)  
   - [Recursion Tree](#recursion-tree)  
   - [Call Stack and List Construction](#call-stack-and-list-construction)  
4. [Implementation (Python)](#implementation-python)  
   - [Base Function (Insert at Front)](#base-function-insert-at-front)  
   - [Alternative: Append and Reverse](#alternative-append-and-reverse)  
5. [Time and Space Complexity Analysis](#time-and-space-complexity-analysis)  
6. [Edge Cases and Failure Scenarios](#edge-cases-and-failure-scenarios)  
7. [Practical Use Cases](#practical-use-cases)  
8. [Limitations and Trade-offs](#limitations-and-trade-offs)  

---

## 1. Definition and Concept Overview

Given an array (list) `arr` and a target element `x`, the goal is to produce a **new list** containing all indices `i` such that `arr[i] == x`, in **ascending order**. The function `ReturnAllIndicesAsAList` implements this using recursion with an explicit index parameter, avoiding list slicing. It builds the result list **on the way back** from the base case by inserting each found index at the beginning of the list returned by the recursive call.

This approach is a form of **post‑order processing**: the recursive call is made first, then the current element is examined, and if it matches, its index is prepended to the list obtained from the deeper recursion. The insertion at the front ensures that indices appear in increasing order despite being collected from right to left.

---

## 2. Core Principles and Internal Mechanics

- **Base case**: When `index == len(arr)`, the function returns an empty list `[]`.
- **Recursive case**: 
  1. Recursively call `ReturnAllIndicesAsAList(arr, x, index + 1)` and store the result in `smallList`.
  2. If `arr[index] == x`, insert `index` at the beginning of `smallList` using `smallList.insert(0, index)`.
  3. Return `smallList`.

Because the recursion proceeds to the end of the array first, the deepest call (largest index) returns first. As the stack unwinds, indices are added from rightmost to leftmost. Inserting each new index at position `0` reverses this order, yielding the correct ascending sequence.

**Why `insert(0)` not `append`?**  
- `append` would add indices in the order they are encountered during unwinding, i.e., from largest to smallest → descending order.  
- `insert(0)` forces each newly found index to become the new first element, so the smallest index (found last) ends up at the front, and the largest index ends up at the back.

**Real‑life analogy**:  
Imagine a stack of papers numbered from 1 to N. You inspect them from top to bottom, but whenever you find a marked page, you place it on top of a separate pile. After examining all pages, your separate pile contains the marked pages in the order they were found (top to bottom). However, if you instead placed each found page at the **bottom** of the pile, the final pile would be in reverse order. Here, the recursion explores from left to right, but builds the list from right to left; inserting at the front is like placing each found page at the bottom of the new pile, achieving the original order.

---

## 3. Step-by-Step Logical Breakdown

### Recurrence Relation
Let `f(arr, x, i)` be the list of indices ≥ i where `arr[k] == x`.

- `f(arr, x, i) = []` if `i == len(arr)`
- else:  
  `rest = f(arr, x, i+1)`  
  if `arr[i] == x`:  
    `rest.insert(0, i)`  
  return `rest`

### Recursion Tree
Example: `arr = [3, 2, 5, 2, 8, 2, 1]`, `x = 2`.  
The tree shows the returned lists at each level.

```
f(arr,2,0)
│
└─ rest = f(arr,2,1) → [1,3,5] after insertion
   │
   └─ rest = f(arr,2,2) → [3,5]
      │
      └─ rest = f(arr,2,3) → [3,5] (arr[2]≠2, no insertion)
         │
         └─ rest = f(arr,2,4) → [5]
            │
            └─ rest = f(arr,2,5) → [5] (arr[4]≠2)
               │
               └─ rest = f(arr,2,6) → []
                  │
                  └─ rest = f(arr,2,7) → [] (base case)
                     ↑
                  arr[6]==2? false → returns []
                  ↑
               arr[5]==2? true → insert 5 at front → returns [5]
               ↑
            arr[4]==2? false → returns [5]
            ↑
         arr[3]==2? true → insert 3 at front → returns [3,5]
         ↑
      arr[2]==2? false → returns [3,5]
      ↑
   arr[1]==2? true → insert 1 at front → returns [1,3,5]
   ↑
arr[0]==2? false → returns [1,3,5]
```

Final list: `[1, 3, 5]`.

### Call Stack and List Construction
Time moves downward; each box shows the local `index` and the state of `smallList` before returning.

```
Frame i=7: returns []
Frame i=6: gets [] from i=7, arr[6]!=2 → returns []
Frame i=5: gets [] from i=6, arr[5]==2 → insert 5 → [5] → returns [5]
Frame i=4: gets [5] from i=5, arr[4]!=2 → returns [5]
Frame i=3: gets [5] from i=4, arr[3]==2 → insert 3 → [3,5] → returns [3,5]
Frame i=2: gets [3,5] from i=3, arr[2]!=2 → returns [3,5]
Frame i=1: gets [3,5] from i=2, arr[1]==2 → insert 1 → [1,3,5] → returns [1,3,5]
Frame i=0: gets [1,3,5] from i=1, arr[0]!=2 → returns [1,3,5]
```

---

## 4. Implementation (Python)

### Base Function (Insert at Front)

```python
def return_all_indices(arr, x, index=0):
    """
    Returns a new list containing all indices where arr[index:] contains x.
    Indices are in ascending order.
    Uses post-order recursion and inserts matching indices at the front.
    """
    # Base case: end of array
    if index == len(arr):
        return []

    # Recursively get indices from the rest of the array
    rest_indices = return_all_indices(arr, x, index + 1)

    # If current element matches, prepend its index
    if arr[index] == x:
        rest_indices.insert(0, index)   # insert at beginning

    return rest_indices

# Example
result = return_all_indices([3, 2, 5, 2, 8, 2, 1], 2)
print(result)   # Output: [1, 3, 5]
```

### Alternative: Append and Reverse

A more efficient way to build the list in ascending order is to **append** during unwinding (which gives descending order) and then **reverse** at the top level. This avoids O(n²) shifts.

```python
def return_all_indices_append_reverse(arr, x, index=0):
    """
    Returns a new list of indices in ascending order.
    Uses post-order recursion with append (gives descending), then reverses.
    """
    if index == len(arr):
        return []

    rest = return_all_indices_append_reverse(arr, x, index + 1)

    if arr[index] == x:
        rest.append(index)   # append at end (yields descending order)

    return rest

def get_indices_ascending(arr, x):
    # Get descending list and reverse
    desc = return_all_indices_append_reverse(arr, x)
    return desc[::-1]   # or desc.reverse() and return desc

# Example
print(get_indices_ascending([3, 2, 5, 2, 8, 2, 1], 2))   # [1, 3, 5]
```

**Note**: The `insert(0)` version is shown for conceptual clarity, but the `append`+`reverse` version is preferred for performance.

---

## 5. Time and Space Complexity Analysis

| Metric               | `insert(0)` version                | `append`+`reverse` version         |
|----------------------|-------------------------------------|-------------------------------------|
| Time Complexity      | O(n²) worst case                    | O(n)                                |
| Stack Space          | O(n)                                | O(n)                                |
| Auxiliary Space      | O(k) for result (k occurrences)     | O(k) for result                     |

- **Time (insert version)**: Each recursive call may perform an `insert(0)` operation, which shifts all existing elements in the list. If there are `k` matches, the total number of shifts is roughly `1 + 2 + ... + k = O(k²)`. In the worst case (all elements match), `k = n`, giving O(n²). The non‑matching levels contribute O(1) each, so overall O(n²).
- **Time (append version)**: Each `append` is amortized O(1). Collecting all matches takes O(n). The final reverse is O(k). Total O(n).
- **Stack space**: Both versions use O(n) stack frames due to recursion depth.
- **Auxiliary space**: The result list stores k indices; in worst case k = n, so O(n).

---

## 6. Edge Cases and Failure Scenarios

- **Empty array**: `index == 0 == len(arr)` → base case returns `[]`.
- **Element not present**: No matches → returns `[]` after full traversal.
- **Single element**: Returns `[0]` if match, else `[]`.
- **Very large array**: Recursion depth may exceed Python’s default limit (~1000) → `RecursionError`.
- **Element appears at all positions**: Returns `[0, 1, …, n-1]` in ascending order.
- **Non‑comparable types**: Comparison `arr[index] == x` works for any type; incompatible types yield `False` (e.g., `2 == "2"`).

---

## 7. Practical Use Cases

- **Educational** – demonstrates post‑order recursion, list building, and the cost of `insert(0)`.
- **Functional programming exercises** – pure function returning a new list without side effects.
- **Small data sets** – where recursion depth and quadratic cost are not prohibitive.
- **Stepping stone** to understanding how recursion can accumulate results.

In real‑world Python, an iterative list comprehension is simpler and efficient:

```python
def find_all_indices(arr, x):
    return [i for i, val in enumerate(arr) if val == x]
```

---

## 8. Limitations and Trade-offs

- **Recursion depth**: Python’s recursion limit restricts input size. For lists longer than ~1000, iteration must be used.
- **Quadratic time** (insert version): Inserting at the front repeatedly makes this implementation impractical for large lists or many matches. The `append`+`reverse` alternative is linear and should be preferred if recursion is required.
- **Stack memory**: Even with tail‑recursive appearance, Python allocates stack frames linearly.
- **Purity**: The function is pure (no side effects) but at the cost of performance in the insert version.
- **Order reliance**: The logic depends on post‑order processing; any change to the order of operations would break the ascending order guarantee.

The primary value of this code is pedagogical: it illustrates how recursion can build a list in reverse order and then correct it via insertion, highlighting the trade‑off between simplicity and efficiency.