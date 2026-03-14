# Search by Index in Singly Linked List

## Definition and Concept Overview

The function `get_at_index(head, index)` retrieves the data stored in the node at the specified zero‑based position within a singly linked list. If the index is valid (i.e., within the range `0` to `length-1`), the function returns the node’s value. If the index is out of bounds (negative or greater than or equal to the list length), an appropriate error indication is returned (in this implementation, the string `"Index out of bounds"`). Both iterative and recursive implementations are presented.

## Core Principles and Internal Mechanics

### Iterative Search by Index
- A temporary pointer `temp` is initialized to `head`.
- A counter `current_index` starts at `0`.
- A `while` loop advances `temp` and increments the counter until either:
  - `temp` becomes `None` (end of list reached, meaning the index was too high), or
  - `current_index` equals the target `index`.
- If the loop exits because `temp` is `None`, the index is out of bounds.
- Otherwise, `temp` points to the desired node, and its `data` is returned.

### Recursive Search by Index
- The recursive function carries the current index as a parameter (decremented on each call) or as an accumulator. A common approach is to pass the target index and decrement it until zero, at which point the current node is the target.
- Base cases:
  1. `head is None`: reached end of list without finding the index → out of bounds.
  2. `index == 0`: current node is the target → return `head.data`.
- Recursive case: call `get_at_index_recursive(head.next, index - 1)` and return its result.
- The recursion depth equals the index value + 1 (if the index is valid), or the list length if out of bounds.

## Step-by-Step Logical Breakdown with ASCII Diagrams

### Iterative Example: Valid Index
List: `[10] → [20] → [30] → [40]`  
Request: `index = 2`

**Initial:**
```
head → [10|*] → [20|*] → [30|*] → [40|None]
temp = head, current_index = 0
```

**Iteration 1:** `current_index (0) < 2`, `temp` not `None` →  
`temp = temp.next` → points to `[20]`, `current_index = 1`

**Iteration 2:** `current_index (1) < 2`, `temp` not `None` →  
`temp = temp.next` → points to `[30]`, `current_index = 2`

**Loop condition:** `current_index == 2`, loop exits. `temp` points to `[30]`.  
Return `temp.data` → `30`.

### Iterative Example: Out‑of‑Bounds Index
Same list, `index = 5`

**Traversal:**
- `temp` moves: `[10]` (idx0), `[20]` (idx1), `[30]` (idx2), `[40]` (idx3), then `temp.next` becomes `None`.
- After moving to `[40]`, `current_index = 3`. Loop continues because `3 < 5` and `temp.next` is not `None`? Wait careful: The loop should check `temp is not None and current_index < index`. But typical implementation uses `while temp is not None and current_index < index` to advance until either condition fails. Let's step correctly:

Standard iterative search by index:
```
temp = head
current_index = 0
while temp is not None and current_index < index:
    temp = temp.next
    current_index += 1
```
So after loop:
- If `temp is None` → index out of bounds.
- Else (temp not None) → we have reached the desired node (since current_index == index).

Now for index=5 on a 4‑node list:

Start: temp=[10], current=0 <5 → temp=[20], current=1
temp=[20], current=1 <5 → temp=[30], current=2
temp=[30], current=2 <5 → temp=[40], current=3
temp=[40], current=3 <5 → temp=None (since [40].next is None), current=4
Now loop condition: `temp is None` → exit. After loop, temp is None → out of bounds.

So correct.

### Recursive Example: Valid Index
List: `[10] → [20] → [30] → [40]`, `index = 2`

Call: `get_at_index_recursive(head, 2)`

1. `(head=[10], index=2)`
   - `head` not `None`, `index != 0` → recursive call `get_at_index_recursive([20], 1)`

2. `(head=[20], index=1)`
   - `head` not `None`, `index != 0` → recursive call `get_at_index_recursive([30], 0)`

3. `(head=[30], index=0)`
   - `index == 0` → return `head.data` → `30`

Returns propagate: call 2 returns `30`, call 1 returns `30`.

### Recursive Example: Out‑of‑Bounds Index
List length 4, `index = 5`

Call: `(head=[10],5)` → `([20],4)` → `([30],3)` → `([40],2)` → `(None,1)`

At `(None,1)`: `head is None` → base case returns `"Index out of bounds"`. This string propagates back.

## Implementation (Python)

```python
class ListNode:
    def __init__(self, data=0, next=None):
        self.data = data
        self.next = next

def get_at_index(head, index):
    """
    Iteratively returns the data at the given zero-based index.
    Returns "Index out of bounds" if the index is invalid.
    """
    temp = head
    current = 0

    # Traverse until either end of list or target index reached
    while temp is not None and current < index:
        temp = temp.next
        current += 1

    # After loop, if temp is None, index was too large
    if temp is None:
        return "Index out of bounds"

    # temp points to the desired node
    return temp.data

def get_at_index_recursive(head, index):
    """
    Recursively returns the data at the given zero-based index.
    Returns "Index out of bounds" if the index is invalid.
    """
    # Base case 1: reached end of list before index becomes zero
    if head is None:
        return "Index out of bounds"

    # Base case 2: found the target node
    if index == 0:
        return head.data

    # Recursive case: move to next node, decrement index
    return get_at_index_recursive(head.next, index - 1)
```

## Time and Space Complexity Analysis

### Iterative Version
- **Time Complexity**: O(n) in the worst case when the index is near the end or out of bounds. Best case O(1) when `index == 0`.
- **Space Complexity**: O(1). Only a constant number of variables are used.

### Recursive Version
- **Time Complexity**: O(n) for the same reasons. Each recursive call processes one node.
- **Space Complexity**: O(n) due to the recursion stack. The depth equals `min(index+1, n+1)` (for out‑of‑bounds, depth = n+1). This can cause stack overflow for large lists or large indices.

## Edge Cases and Failure Scenarios

- **Empty list (`head is None`)**: Both functions return `"Index out of bounds"` for any index (including 0).
- **Index 0**: Returns head’s data immediately (iterative loop not entered; recursive hits base case `index==0`).
- **Index equal to length-1**: Returns last node’s data after traversing all nodes.
- **Index equal to length**: Out of bounds; both functions traverse entire list and return error string.
- **Negative index**: Not handled; iterative loop condition `current < index` with negative index will be false immediately (since 0 < negative is false), so loop doesn’t run, `temp` remains head, and since `temp` is not `None`, it incorrectly returns head’s data. Recursive version with negative index: it will keep decrementing index until `head` becomes `None` (if list finite) and then return out of bounds, but if the list is long, it will traverse entire list first. However, negative indices should be considered invalid; this implementation does not guard against them. In practice, a check for `index < 0` should be added.
- **Large index**: Both functions will traverse until end (if out of bounds) or until index reached; recursive depth may be problematic.

## Practical Use Cases

- **Random access emulation**: Although linked lists do not support O(1) random access, this function provides a way to access elements by position when necessary.
- **Implementing list-like interfaces**: When building a custom list class that mimics Python’s `list`, the `__getitem__` method can use this function.
- **Algorithms that need indexed access**: Some algorithms (e.g., selection sort on linked list) require accessing elements at arbitrary positions.
- **Debugging and validation**: Retrieving values at specific positions to verify list structure after modifications.

## Limitations and Trade-offs

- **Linear time access**: Unlike arrays, accessing an element by index in a linked list always requires O(n) time. This is a fundamental limitation of the data structure.
- **No negative index support**: The implementation does not support negative indices (counting from the end). Adding this would require either a two‑pass approach (first compute length, then adjust index) or a recursive approach that counts from the end, which is more complex.
- **Error indication via string**: Returning a string for out‑of‑bounds mixes types and is not robust. In production code, raising an `IndexError` exception or returning `None` (with a separate error flag) is preferable.
- **Recursion depth limit**: The recursive version is unsuitable for long lists or large indices due to Python’s recursion limit. The iterative version is always safe.
- **Assumes acyclic list**: If the list contains a cycle, the iterative version will loop forever (unless a cycle detection mechanism is added), and the recursive version will exceed recursion depth or loop indefinitely (if tail recursion were optimized, but Python does not optimize).