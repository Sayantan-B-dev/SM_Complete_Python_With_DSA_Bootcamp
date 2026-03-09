# Finding All Indices of an Element in an Array Using Recursion

## Table of Contents
1. [Definition and Concept Overview](#definition-and-concept-overview)  
2. [Core Principles and Internal Mechanics](#core-principles-and-internal-mechanics)  
3. [Step-by-Step Logical Breakdown](#step-by-step-logical-breakdown)  
   - [Recurrence Relation](#recurrence-relation)  
   - [Recursion Tree (Print All Indices)](#recursion-tree-print-all-indices)  
4. [Implementation (Python)](#implementation-python)  
   - [Version 1: Print Indices](#version-1-print-indices)  
   - [Version 2: Update a Provided List](#version-2-update-a-provided-list)  
   - [Version 3: Update a Global List](#version-3-update-a-global-list)  
   - [Version 4: Return a New List (Accumulator Pattern)](#version-4-return-a-new-list-accumulator-pattern)  
5. [Time and Space Complexity Analysis](#time-and-space-complexity-analysis)  
6. [Edge Cases and Failure Scenarios](#edge-cases-and-failure-scenarios)  
7. [Practical Use Cases](#practical-use-cases)  
8. [Limitations and Trade-offs](#limitations-and-trade-offs)  

---

## 1. Definition and Concept Overview

Given an array (list) and a target element `x`, the objective is to find **all indices** at which `x` occurs. Four common output strategies are considered:

1. **Print** each index directly to the console.  
2. **Update a list** provided as an argument (passed by reference) with the indices.  
3. **Update a global list** (module‑level variable) with the indices.  
4. **Return a new list** containing all indices.

The recursive solution processes the array element by element, using an **index parameter** to avoid expensive list slicing. A helper function encapsulates the recursion, allowing the main function to present a clean interface (only array and target). The base case occurs when the index reaches the array length.

---

## 2. Core Principles and Internal Mechanics

### Recursive Decomposition
Let `find_all(arr, x, i)` be a function that processes the subarray from index `i` to the end. The problem is decomposed as:

- If `i == len(arr)`, do nothing (base case).  
- Otherwise, recursively process the rest (`i+1`), then (or before) check if `arr[i] == x` and record the index accordingly.

The order of the recursive call and the check determines whether indices are recorded in **forward** or **reverse** order. Recording **after** the recursive call (post‑order) yields indices in increasing order because the recursion goes to the end first and then records on the way back. Recording **before** the recursive call (pre‑order) yields indices in decreasing order.

### Index Parameter vs. Slicing
Using an explicit `index` parameter eliminates the O(n²) copying overhead of slicing. The array itself is never modified or copied; only an integer index is passed, making the recursion both space‑efficient (stack frames still O(n)) and time‑efficient (O(n) total operations).

### Accumulator Pattern for Returning a List
To return a new list, an accumulator can be built during recursion. The efficient approach uses a mutable list (passed by reference or updated in place) to collect indices, avoiding repeated list concatenation. A pure functional version that creates new lists at each level is O(n²) and shown only for conceptual contrast.

---

## 3. Step-by-Step Logical Breakdown

### Recurrence Relation
Let `f(arr, x, i)` be a procedure that records all indices ≥ i where `arr[k] == x`.

- Base: if `i == len(arr)`, do nothing (or return empty list).  
- Recursive: first call `f(arr, x, i+1)` to process the rest, then (or before) check `arr[i]` and record if equal.

### Recursion Tree (Print All Indices)
Consider `arr = [3,2,5,2,8,2,1]`, `x = 2`. The tree for the **post‑order** printing version (print after recursive call) is shown below. Numbers in parentheses are the current index.

```
print_all_helper([3,2,5,2,8,2,1], 2, 0)
│
└─ print_all_helper(..., 1)
   │
   └─ print_all_helper(..., 2)
      │
      └─ print_all_helper(..., 3)
         │
         └─ print_all_helper(..., 4)
            │
            └─ print_all_helper(..., 5)
               │
               └─ print_all_helper(..., 6)
                  │
                  └─ print_all_helper(..., 7) → base case (index==len)
                     ↑ (returns)
                  check arr[6]==2? (1==2? false) → no print
                  ↑ (returns)
               check arr[5]==2? (2==2? true) → print 5
               ↑ (returns)
            check arr[4]==2? (8==2? false) → no print
            ↑ (returns)
         check arr[3]==2? (2==2? true) → print 3
         ↑ (returns)
      check arr[2]==2? (5==2? false) → no print
      ↑ (returns)
   check arr[1]==2? (2==2? true) → print 1
   ↑ (returns)
check arr[0]==2? (3==2? false) → no print
↑ (returns)
```

Output order: `5 3 1` (descending indices). If the print statement is placed **before** the recursive call, the output becomes `1 3 5` (ascending). Both are valid; the choice depends on requirement.

For returning a list in ascending order, pre‑order accumulation (appending before recursion) yields ascending indices naturally. Post‑order accumulation yields descending; a final reverse can fix it if ascending is required.

---

## 4. Implementation (Python)

All implementations use a helper function with an `index` parameter to avoid slicing. The main function provides a clean interface.

### Version 1: Print Indices

```python
def print_all_indices(arr, x):
    """
    Prints all indices where x appears in arr.
    Uses a helper with index parameter to traverse recursively.
    """
    def helper(i):
        # Base case: end of array
        if i == len(arr):
            return
        # Recursively process the rest first (post-order)
        helper(i + 1)
        # Check current element after returning from deeper calls
        if arr[i] == x:
            print(i)

    helper(0)

# Example
print_all_indices([3, 2, 5, 2, 8, 2, 1], 2)
# Output: 5 3 1 (each on new line)
```

To print in ascending order, move the check before the recursive call:

```python
def print_all_indices_ascending(arr, x):
    def helper(i):
        if i == len(arr):
            return
        if arr[i] == x:
            print(i)
        helper(i + 1)
    helper(0)
# Output: 1 3 5
```

### Version 2: Update a Provided List

A list is passed by reference and mutated to collect indices. This is efficient and returns no value.

```python
def collect_indices_inplace(arr, x, result_list):
    """
    Appends all indices of x in arr to result_list.
    result_list must be a mutable list.
    """
    def helper(i):
        if i == len(arr):
            return
        # Pre-order for ascending indices
        if arr[i] == x:
            result_list.append(i)
        helper(i + 1)

    helper(0)

# Usage:
res = []
collect_indices_inplace([3, 2, 5, 2, 8, 2, 1], 2, res)
print(res)  # Output: [1, 3, 5]
```

If descending order is needed, use post‑order:

```python
def collect_indices_descending(arr, x, result_list):
    def helper(i):
        if i == len(arr):
            return
        helper(i + 1)
        if arr[i] == x:
            result_list.append(i)
    helper(0)
# result_list becomes [5,3,1]
```

### Version 3: Update a Global List

A global (module‑level) variable holds the result. This approach is generally discouraged due to side effects but is shown for completeness.

```python
# Global list (must be cleared before each call)
global_indices = []

def collect_indices_global(arr, x):
    global global_indices
    global_indices = []  # reset

    def helper(i):
        if i == len(arr):
            return
        # Pre-order for ascending indices
        if arr[i] == x:
            global_indices.append(i)
        helper(i + 1)

    helper(0)

# Usage:
collect_indices_global([3, 2, 5, 2, 8, 2, 1], 2)
print(global_indices)  # Output: [1, 3, 5]
```

### Version 4: Return a New List (Accumulator Pattern)

Two sub‑versions: one builds the list by concatenation (inefficient) and one uses an accumulator passed by reference (efficient). The efficient version mimics Version 2 but returns the list instead of mutating an external one.

#### Efficient (using a mutable accumulator)

```python
def find_all_indices(arr, element):
    """
    Returns a list of all indices where element occurs in arr.
    Uses an internal mutable list as accumulator.
    """
    result = []

    def helper(index):
        if index == len(arr):
            return
        if arr[index] == element:
            result.append(index)
        helper(index + 1)

    helper(0)
    return result

# Example
print(find_all_indices([3, 2, 5, 2, 8, 2, 1], 2))  # [1, 3, 5]
```

#### Pure functional (inefficient, creates new lists)

For educational purposes, a version that returns a new list without mutation:

```python
def find_all_pure(arr, element, index=0):
    """
    Returns a list of indices (starting from index) where element occurs.
    Creates new lists by concatenation – O(n²) copying.
    """
    if index == len(arr):
        return []
    rest_indices = find_all_pure(arr, element, index + 1)
    if arr[index] == element:
        return [index] + rest_indices   # Prepend (produces ascending)
    else:
        return rest_indices
```

This version is O(n²) because each prepend creates a new list. It is shown only to illustrate the concept.

---

## 5. Time and Space Complexity Analysis

| Version                          | Time Complexity | Stack Space | Additional Memory |
|-----------------------------------|-----------------|-------------|-------------------|
| Print indices                     | O(n)            | O(n)        | O(1)              |
| Update provided list (in‑place)   | O(n)            | O(n)        | O(k) for result list |
| Update global list                | O(n)            | O(n)        | O(k) for global list |
| Return new list (efficient)       | O(n)            | O(n)        | O(k) for result list |
| Return new list (pure concatenation) | O(n²)        | O(n)        | O(n²) temporary lists |

- **Time**: All efficient versions examine each element once, performing O(1) work per element (comparison and possibly an append). The pure concatenation version copies lists at each level, leading to O(n²) total operations.  
- **Stack space**: Recursion depth = n+1 → O(n). Python’s lack of tail‑call optimization prevents stack reuse.  
- **Additional memory**: Result storage requires O(k) where k is the number of occurrences. In the worst case (all elements equal), k = n, so O(n) extra space for the result.

---

## 6. Edge Cases and Failure Scenarios

- **Empty array**: Returns/prints nothing; result list empty.  
- **Element not present**: No indices recorded.  
- **Element appears once**: Single index recorded.  
- **Element appears at all positions**: All indices from 0 to n-1 are recorded.  
- **Very large array**: Recursion depth limit (~1000) will cause `RecursionError`. Iterative solution is required for large n.  
- **Non‑comparable types**: Comparison `arr[i] == x` works for any type, but if types differ, it may return `False` without error (e.g., `2 == "2"` is `False`).  
- **Modifying the array during recursion**: The implementations assume the array is not mutated; doing so would lead to incorrect indices.

---

## 7. Practical Use Cases

- **Search operations** in small datasets where recursion is acceptable.  
- **Educational demonstrations** of recursion, helper functions, and accumulator patterns.  
- **Functional programming** contexts where recursion is idiomatic.  
- **Building blocks** for more complex recursive algorithms (e.g., finding all paths in a tree that match a condition).

In production Python, an iterative loop with `enumerate` is simpler and avoids recursion depth limits:

```python
def find_all_iterative(arr, x):
    return [i for i, val in enumerate(arr) if val == x]
```

---

## 8. Limitations and Trade-offs

- **Recursion depth**: Python’s recursion limit (typically 1000) restricts input size. Iteration is preferred for large arrays.  
- **Stack memory**: Even with index parameters, each call consumes a stack frame, leading to O(n) space.  
- **Global variables**: Using a global list (Version 3) introduces hidden state and is error‑prone in concurrent or repeated use.  
- **Pure functional concatenation**: Extremely inefficient due to O(n²) copying; should be avoided.  
- **Order of indices**: Pre‑order vs. post‑order determines output order; the choice must match the requirement. The examples above demonstrate both.

The most robust and Pythonic recursive approach is **Version 2 (update provided list)** or **Version 4 (return new list with mutable accumulator)** because they are efficient, avoid globals, and clearly separate concerns. For any real‑world use, the iterative list comprehension is superior.