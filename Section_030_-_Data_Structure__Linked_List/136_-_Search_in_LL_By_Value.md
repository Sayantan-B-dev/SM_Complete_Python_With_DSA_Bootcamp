# Search for Value in Singly Linked List

## Definition and Concept Overview

Two functions are presented to locate the first occurrence of a given value in a singly linked list:

- `search_by_value(head, value)` – iterative implementation.
- `search_by_value_recursive(head, value)` – recursive implementation.

Both functions traverse the list starting from the head, comparing each node's `data` field with the target `value`. If a match is found, the zero‑based index of that node is returned. If the value is not present in the list, the string `"Not Found"` is returned. The recursive version requires an auxiliary parameter to track the current index; the provided snippet contains a parameter mismatch, which is corrected in the implementation below.

## Core Principles and Internal Mechanics

### Iterative Search
- A temporary pointer `temp` is initialized to `head`.
- An integer `index` starts at `0`.
- A `while` loop continues as long as `temp` is not `None`.
- Inside the loop, `temp.data` is compared with `value`. If equal, the current `index` is returned immediately.
- If not equal, `temp` advances to `temp.next` and `index` increments.
- If the loop exits (i.e., `temp` becomes `None`), the value was not found, and the function returns `"Not Found"`.

### Recursive Search
- The recursive function must carry the current index as an argument because the stack frame needs to know the position of the node being examined.
- Two base cases:
  1. `head is None`: the end of the list is reached without finding the value → return `"Not Found"`.
  2. `head.data == value`: the current node contains the target → return the current index.
- Recursive case: call `search_by_value_recursive(head.next, value, index + 1)` and return whatever that call returns.
- The recursion depth equals the number of nodes visited until the value is found or the list ends.

## Step-by-Step Logical Breakdown with ASCII Diagrams

### Iterative Search Example
List: `[5] → [3] → [8] → [2]`  
Search for `value = 8`.

**Initial state:**
```
head → [5|*] → [3|*] → [8|*] → [2|None]
temp = head, index = 0
```

**Iteration 1:**
- `temp.data` (5) != 8
- `temp = temp.next` → points to `[3]`
- `index = 1`

**Iteration 2:**
- `temp.data` (3) != 8
- `temp = temp.next` → points to `[8]`
- `index = 2`

**Iteration 3:**
- `temp.data` (8) == 8 → return `index` (2)

### Recursive Search Example
Same list, `value = 8`.  
Function call: `search_by_value_recursive(head, 8, 0)` (using corrected signature).

**Call stack:**
1. `(head=[5], value=8, index=0)`  
   - `head.data` (5) != 8, `head` not `None` → recursive call `(head.next, 8, 1)`

2. `(head=[3], value=8, index=1)`  
   - `head.data` (3) != 8 → recursive call `(head.next, 8, 2)`

3. `(head=[8], value=8, index=2)`  
   - `head.data` (8) == 8 → return `2`

**Unwinding:**  
Call 3 returns `2` to call 2, which returns `2` to call 1, which returns `2` to the original caller.

### Search Failure Example
List: `[5] → [3] → [8] → [2]`, `value = 10`.

**Iterative:**
- Loop advances `temp` through all nodes, `index` becomes 4.
- `temp` becomes `None`, loop exits, returns `"Not Found"`.

**Recursive:**
- Calls proceed until `head` becomes `None` at depth 4.
- Base case `head is None` returns `"Not Found"` to the previous frame, which propagates the string upward.

## Implementation (Python)

```python
class ListNode:
    def __init__(self, data=0, next=None):
        self.data = data
        self.next = next

def search_by_value(head, value):
    """
    Iteratively searches for the first node containing `value`.
    Returns the zero-based index if found, otherwise the string "Not Found".
    """
    temp = head
    index = 0

    while temp is not None:
        if temp.data == value:
            return index
        temp = temp.next
        index += 1

    return "Not Found"

def search_by_value_recursive(head, value, index=0):
    """
    Recursively searches for the first node containing `value`.
    The `index` parameter tracks the current position and must be provided
    by the caller (default 0 for external call).
    Returns the index if found, otherwise "Not Found".
    """
    # Base case 1: reached end of list without finding value
    if head is None:
        return "Not Found"

    # Base case 2: current node matches
    if head.data == value:
        return index

    # Recursive case: move to next node, increment index
    return search_by_value_recursive(head.next, value, index + 1)
```

**Note on recursion:** The function signature includes `index` with a default value of `0`, allowing external calls like `search_by_value_recursive(head, value)` without explicitly passing the starting index.

## Time and Space Complexity Analysis

### Iterative Version
- **Time Complexity**: O(n) in the worst case (value not found or found at last node). Best case O(1) if value is at head.
- **Space Complexity**: O(1). Only a constant amount of extra memory (pointer and counter) is used.

### Recursive Version
- **Time Complexity**: O(n) for the same reasons. Each recursive call processes one node.
- **Space Complexity**: O(n) due to the recursion stack. In the worst case (value not found), the stack depth equals the number of nodes. This can lead to stack overflow for very long lists (Python’s default recursion limit is ~1000).

## Edge Cases and Failure Scenarios

- **Empty list (`head is None`)**: Both functions return `"Not Found"` (iterative loop not entered, recursive base case triggers).
- **Value at head**: Returns `0` immediately; iterative version does one comparison, recursive version hits second base case.
- **Value at tail**: Both functions traverse the entire list; iterative uses O(1) space, recursive uses O(n) stack.
- **Value not present**: Both traverse all nodes and return `"Not Found"`. Recursive version consumes stack depth equal to list length.
- **Multiple occurrences**: Only the first occurrence (lowest index) is returned.
- **Data types and equality**: The comparison `temp.data == value` uses Python’s `==` operator. For custom objects, this relies on the `__eq__` method. If `value` is `None`, it will match a node whose `data` is `None`.

## Practical Use Cases

- **Symbol lookup**: Find the position of a key in a linked list representing a symbol table.
- **Index validation**: Determine if a value exists before performing an operation that requires its position.
- **Debugging**: Verify the presence of a particular data item in a list during testing.
- **Search in functional contexts**: Recursive search may be used in languages that optimize tail recursion (though Python does not) or for educational purposes to demonstrate recursion on linked structures.

## Limitations and Trade-offs

- **Return type mixing**: Returning either an integer or a string (`"Not Found"`) is not type‑safe. In production code, returning `-1` or `None` and raising an exception (e.g., `ValueError`) is preferable. The current design forces callers to check the return type.
- **Recursion depth**: The recursive version is unsuitable for large lists due to stack overflow. Python’s recursion limit imposes a hard constraint; the iterative version is always safe.
- **No early termination in recursion for missing value**: Even when the value is not present, recursion continues to the end, consuming stack space. Iterative version also traverses all nodes but without stack overhead.
- **Parameter requirement for recursion**: The recursive function needs an accumulator parameter (`index`), which may be seen as less elegant than a purely functional approach (though it can be hidden with a wrapper).
- **Single occurrence only**: Neither function can locate all indices of a value without modification. If multiple occurrences are needed, the algorithm must be changed to collect results.
- **Assumes list is acyclic**: Both functions assume a standard singly linked list without cycles. If a cycle exists, they will loop indefinitely (iterative) or cause recursion depth error (recursive) as the termination condition `head is None` never occurs.