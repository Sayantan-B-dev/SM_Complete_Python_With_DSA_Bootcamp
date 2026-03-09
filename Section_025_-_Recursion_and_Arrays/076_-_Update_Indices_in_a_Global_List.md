# Finding All Indices of an Element Using a Global List in Recursion

## Table of Contents
1. [Definition and Concept Overview](#definition-and-concept-overview)  
2. [Core Principles and Internal Mechanics](#core-principles-and-internal-mechanics)  
3. [Step-by-Step Logical Breakdown](#step-by-step-logical-breakdown)  
   - [Recurrence Relation](#recurrence-relation)  
   - [Recursion Tree (Pre‑order)](#recursion-tree-preorder)  
   - [Call Stack Diagram](#call-stack-diagram)  
   - [Order Variation: Post‑order](#order-variation-postorder)  
4. [Implementation (Python)](#implementation-python)  
   - [Pre‑order Version (Ascending Indices)](#preorder-version-ascending-indices)  
   - [Post‑order Version (Descending Indices)](#postorder-version-descending-indices)  
5. [Time and Space Complexity Analysis](#time-and-space-complexity-analysis)  
6. [Edge Cases and Failure Scenarios](#edge-cases-and-failure-scenarios)  
7. [Practical Use Cases](#practical-use-cases)  
8. [Limitations and Trade-offs](#limitations-and-trade-offs)  

---

## 1. Definition and Concept Overview

The task is to collect all indices at which a given element `x` appears in an array (list). This version uses a **global list** (module‑level variable) to store the indices, which is updated recursively. The recursion traverses the array element by element using an explicit index parameter, avoiding list slicing. At each step, if the current element equals `x`, the global list is appended with the current index.

The use of a global variable simplifies the function signature (no need to pass a result list) but introduces side effects and statefulness. It is presented here as a pedagogical contrast to the parameter‑passing approach.

Two common orders of appending are possible depending on whether the check occurs **before** the recursive call (pre‑order) or **after** (post‑order). The pre‑order version yields indices in ascending order (the order of traversal); the post‑order version yields indices in descending order (rightmost first).

---

## 2. Core Principles and Internal Mechanics

- **Base case**: When `index == len(arr)`, the recursion terminates. No further elements are examined.
- **Recursive case**: 
  - **Pre‑order**: Check `arr[index] == x` and append to global list if true, then recurse with `index + 1`.
  - **Post‑order**: Recurse first with `index + 1`, then check and append if true.
- **Global state**: The global list exists outside the function; all recursive calls share the same list object. Appends modify it in place.
- **Call stack**: Each recursive call pushes a frame containing the local `index`. Frames are popped in reverse order of calls. The global list is not stored on the stack; only a reference is used.

Because the list is global, no value needs to be returned; the final state of the global list after the top‑level call contains all indices.

---

## 3. Step-by-Step Logical Breakdown

### Recurrence Relation
Define `update_global(arr, x, i)` as a procedure that updates a global list `ans` with indices ≥ i where `arr[k] == x`.

- Base: if `i == len(arr)`, do nothing.
- Recursive (pre‑order):
  1. If `arr[i] == x`, append `i` to `ans`.
  2. Call `update_global(arr, x, i + 1)`.
- Recursive (post‑order):
  1. Call `update_global(arr, x, i + 1)`.
  2. If `arr[i] == x`, append `i` to `ans`.

### Recursion Tree (Pre‑order)
Example: `arr = [3, 2, 5, 2, 8, 2, 1]`, `x = 2`. Global list initially `[]`.

```
update_global(arr, 2, 0)
├─ arr[0]==2? false
│  └─ update_global(..., 1)
│     ├─ arr[1]==2? true → append 1 → global = [1]
│     └─ update_global(..., 2)
│        ├─ arr[2]==2? false
│        └─ update_global(..., 3)
│           ├─ arr[3]==2? true → append 3 → global = [1, 3]
│           └─ update_global(..., 4)
│              ├─ arr[4]==2? false
│              └─ update_global(..., 5)
│                 ├─ arr[5]==2? true → append 5 → global = [1, 3, 5]
│                 └─ update_global(..., 6)
│                    ├─ arr[6]==2? false
│                    └─ update_global(..., 7) → base case
```

Final global list: `[1, 3, 5]` (ascending).

### Call Stack Diagram (Pre‑order)
Time progresses downward. Each box represents a stack frame with its index. Appends occur at the marked frames.

```
Frame i=0 (arr[0]≠2) → calls i=1
└─ Frame i=1 (arr[1]==2) → append 1, then call i=2
   └─ Frame i=2 (arr[2]≠2) → call i=3
      └─ Frame i=3 (arr[3]==2) → append 3, then call i=4
         └─ Frame i=4 (arr[4]≠2) → call i=5
            └─ Frame i=5 (arr[5]==2) → append 5, then call i=6
               └─ Frame i=6 (arr[6]≠2) → call i=7
                  └─ Frame i=7 → base case, returns
                  ← returns to i=6
               ← returns to i=5
            ← returns to i=4
         ← returns to i=3
      ← returns to i=2
   ← returns to i=1
← returns to i=0
```

Appends happen at frames 1, 3, 5 in that order → global becomes `[1, 3, 5]`.

### Order Variation: Post‑order
If the check is placed after recursion, the tree changes: recursion descends to the base case first, then appends on the way back.

**Recursion tree (post‑order) for same input:**

```
update_global(arr, 2, 0)
├─ update_global(..., 1)
│  ├─ update_global(..., 2)
│  │  ├─ update_global(..., 3)
│  │  │  ├─ update_global(..., 4)
│  │  │  │  ├─ update_global(..., 5)
│  │  │  │  │  ├─ update_global(..., 6)
│  │  │  │  │  │  ├─ update_global(..., 7) → base case
│  │  │  │  │  │  └─ arr[6]==2? false
│  │  │  │  │  └─ arr[5]==2? true → append 5 → global = [5]
│  │  │  │  └─ arr[4]==2? false
│  │  │  └─ arr[3]==2? true → append 3 → global = [5, 3]
│  │  └─ arr[2]==2? false
│  └─ arr[1]==2? true → append 1 → global = [5, 3, 1]
└─ arr[0]==2? false
```

Final global list: `[5, 3, 1]` (descending).

---

## 4. Implementation (Python)

### Pre‑order Version (Ascending Indices)

```python
# Global list to store indices (must be cleared before each use)
ans_list = []

def update_indices_global_pre(arr, x, index):
    """
    Appends all indices where arr[index:] contains x to the global list ans_list.
    Pre‑order traversal: check current, then recurse.
    Results in ascending order.
    """
    # Base case: end of array
    if index == len(arr):
        return

    # Check current element before recursing
    if arr[index] == x:
        ans_list.append(index)

    # Recurse to the next index
    update_indices_global_pre(arr, x, index + 1)

# Usage
update_indices_global_pre([3, 2, 5, 2, 8, 2, 1], 2, 0)
print(ans_list)   # Output: [1, 3, 5]
```

### Post‑order Version (Descending Indices)

```python
ans_list = []   # Reset global list

def update_indices_global_post(arr, x, index):
    """
    Appends indices where arr[index:] contains x to global list ans_list.
    Post‑order traversal: recurse first, then check current.
    Results in descending order.
    """
    if index == len(arr):
        return

    # Recurse first
    update_indices_global_post(arr, x, index + 1)

    # Check current element after recursion
    if arr[index] == x:
        ans_list.append(index)

# Usage
update_indices_global_post([3, 2, 5, 2, 8, 2, 1], 2, 0)
print(ans_list)   # Output: [5, 3, 1]
```

**Important**: The global list must be reset (e.g., reassigned to `[]`) before each new call to avoid contamination from previous runs. The examples above assume a fresh start.

---

## 5. Time and Space Complexity Analysis

| Metric               | Value                        |
|----------------------|------------------------------|
| Time Complexity      | O(n)                         |
| Stack Space          | O(n)                         |
| Auxiliary Space      | O(k) for result list (k occurrences) |

- **Time**: Exactly `n` recursive calls, each performing O(1) work (comparison and possibly an append). Appends to a list are amortized O(1).  
- **Stack space**: Each call adds a frame; depth = n+1 → O(n). Python does not optimize tail calls.  
- **Auxiliary space**: The global list stores k indices; in worst case (all elements equal), k = n, so O(n) additional space. No extra copying of the array occurs.

---

## 6. Edge Cases and Failure Scenarios

- **Empty array**: `index == 0 == len(arr)` → base case triggers immediately; global list remains empty.  
- **Element not present**: No indices appended; global list empty.  
- **Single element**: If it matches, one index appended; recursion depth 1.  
- **Very large array**: Recursion depth may exceed Python’s default limit (~1000) → `RecursionError`. Iterative solution required for large inputs.  
- **Element appears at all positions**: All indices 0..n-1 are appended; order depends on pre/post choice.  
- **Non‑comparable types**: Comparison `arr[index] == x` works for any type; incompatible types yield `False` (e.g., `2 == "2"`).  
- **Global list not reset**: If the same global list is reused without clearing, indices from previous calls persist, leading to incorrect results.

---

## 7. Practical Use Cases

- **Educational demonstration** of recursion with global state and traversal order (pre‑ vs. post‑order).  
- **Small‑scale data processing** where recursion depth is not a concern and global variables are acceptable (e.g., classroom exercises).  
- **Illustrating side effects** and their implications in recursive functions.  
- **Building block** for recursive algorithms that need to accumulate results without returning them (though returning is generally cleaner).

In production, global variables are avoided due to hidden dependencies and difficulty in testing. The parameter‑passing pattern (e.g., passing a result list) is preferred.

---

## 8. Limitations and Trade-offs

- **Recursion depth**: Python’s recursion limit restricts input size to about 1000.  
- **Global state**: The function is not reentrant; concurrent calls interfere. Requires manual resetting.  
- **Side effects**: Modifying a global variable makes the function impure, reducing predictability and testability.  
- **Order dependence**: The caller must be aware of whether pre‑order or post‑order is used, as this determines the order of indices.  
- **Stack memory**: Even with tail‑recursive appearance, Python allocates stack frames for each call, leading to O(n) memory usage.  
- **No early termination**: The entire array is always traversed, even if early termination is possible (not applicable here as we need all indices).

The recursive approach with a global list is primarily a learning tool. For any real‑world application, an iterative loop with a local list is superior:

```python
def find_all_indices_iter(arr, x):
    return [i for i, val in enumerate(arr) if val == x]
```