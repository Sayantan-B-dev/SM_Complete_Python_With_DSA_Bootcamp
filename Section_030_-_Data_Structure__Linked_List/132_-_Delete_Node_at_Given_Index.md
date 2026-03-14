# Deletion of Node at Specific Index in Singly Linked List (Iterative)

## Definition and Concept Overview

The function `delete_at_index(head, index)` removes the node at position `index` (0‑based) from a singly linked list and returns the head of the modified list. The operation preserves the relative order of the remaining nodes. If the list is empty or the index is invalid (negative or beyond the last node), the function handles the situation without modifying the list.

The algorithm iterates to the node immediately before the target position, adjusts the `next` pointer to bypass the target node, and implicitly discards the removed node (which becomes unreachable and is garbage‑collected).

## Core Principles and Internal Mechanics

- **Traversal**: A temporary pointer `temp` moves from the head to the node at `index - 1` (the predecessor of the target node). A counter tracks the current position.
- **Boundary checks**: Before accessing `temp.next`, the algorithm verifies that `temp` and `temp.next` are not `None`. This prevents dereferencing `None` when the index is out of range.
- **Pointer manipulation**: The node to be deleted is referenced as `nodeToBeDeleted`. Its successor (`nodeAfterDeletedNode`) is stored, and the predecessor’s `next` is updated to point to that successor. The target node is effectively unlinked.
- **Head update**: When `index == 0`, the head itself is the target. The new head becomes `head.next` (which may be `None` if the list had only one node). The original head is no longer referenced.

## Step-by-Step Logical Breakdown with ASCII Diagrams

### Case 1: Empty List
**Condition:** `head == None`  
**Action:** Print error message and return `head` (which is `None`). No diagram needed.

### Case 2: Delete Head (index == 0)
**Before deletion:**
```
head -> [A|*] -> [B|*] -> [C|None]
```
**Operation:** `return head.next`  
**After deletion:**
```
head -> [B|*] -> [C|None]
```
The original head node `[A]` is no longer reachable.

### Case 3: Delete Middle Node (e.g., index = 2 in a 4‑node list)
**Before deletion:**
```
head -> [10|*] -> [20|*] -> [30|*] -> [40|None]
        0         1         2         3
```
Target node: `[30]` at index 2.

**Step 1 – Traverse to predecessor (index = 1):**
```
temp = head
count = 0
while count < index-1 (i.e., 1):
    temp = temp.next   # temp now points to node [20]
    count = 1
```

**Step 2 – Verify predecessor and target exist:**
```
if temp is None or temp.next is None:  # false in this case
```
`temp` is not `None` and `temp.next` (node `[30]`) is not `None`, so deletion is safe.

**Step 3 – Identify nodes involved:**
```
nodeToBeDeleted = temp.next      # points to [30]
nodeAfterDeletedNode = nodeToBeDeleted.next  # points to [40]
```

**Step 4 – Relink predecessor to successor:**
```
temp.next = nodeAfterDeletedNode
```
Now `[20]` points directly to `[40]`.

**After deletion:**
```
head -> [10|*] -> [20|*] -> [40|None]
        0         1         2
```
Node `[30]` is unlinked.

### Case 4: Delete Tail Node (index = last position)
**Before deletion:**
```
head -> [10|*] -> [20|*] -> [30|None]
        0         1         2
```
Target: index 2.

**Step 1 – Traverse to predecessor (index = 1):**
`temp` points to `[20]`.

**Step 2 – Check:**
`temp` is not `None`, `temp.next` (node `[30]`) is not `None` → proceed.

**Step 3 – Identify:**
`nodeToBeDeleted = temp.next` → `[30]`  
`nodeAfterDeletedNode = nodeToBeDeleted.next` → `None`

**Step 4 – Relink:**
`temp.next = nodeAfterDeletedNode` → `[20].next = None`

**After deletion:**
```
head -> [10|*] -> [20|None]
        0         1
```
Tail node removed.

### Case 5: Out‑of‑Bounds Index (index >= length)
**Example:** List with 3 nodes, index = 5.
**Traversal loop:**
```
while temp is not None and count < index-1 (4):
    temp = temp.next   # after 3 steps temp becomes None, loop exits
```
**After loop:** `temp` is `None` (or possibly `temp` is the last node but `temp.next` is `None` when `count == index-1` and index is exactly length?). The condition `if temp is None or temp.next is None` catches both:
- If `temp` is `None` (index too large).
- If `temp.next` is `None` (index equals length, i.e., trying to delete after last node).
**Action:** Print error and return `head` unchanged.

## Implementation (Python)

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def delete_at_index(head, index):
    # Handle empty list
    if head is None:
        print("Linked list is empty")
        return head

    # Special case: deleting the first node
    if index == 0:
        # New head becomes the second node (or None if list had one node)
        return head.next

    # Traverse to the node just before the target index
    temp = head
    count = 0

    # Move temp to the (index-1)-th node, stopping if we run out of nodes
    while temp is not None and count < index - 1:
        temp = temp.next
        count += 1

    # Check if index is out of bounds:
    # - temp became None (index > length)
    # - temp.next is None (index == length, trying to delete past the tail)
    if temp is None or temp.next is None:
        print("Index out of bounds")
        return head

    # At this point, temp points to the node before the target
    node_to_delete = temp.next          # the node to be removed
    node_after_deleted = node_to_delete.next  # the node that will follow temp

    # Bypass the deleted node
    temp.next = node_after_deleted

    # Return the (unchanged) head reference
    return head
```

## Time and Space Complexity Analysis

- **Time Complexity**: O(n) worst case, where n is the number of nodes. The traversal loop runs up to `index - 1` steps, which in the worst case (deleting the last node) requires visiting all nodes. The loop also stops early if the index is out of bounds, but that is still bounded by O(n).
- **Space Complexity**: O(1). Only a constant number of pointers (`temp`, `node_to_delete`, `node_after_deleted`) are used, regardless of list size. The algorithm is iterative, so no additional stack space is consumed.

## Edge Cases and Failure Scenarios

- **Empty list** (`head is None`): The function prints an error and returns `None`. Calling code must be prepared to receive `None`.
- **Negative index**: Not explicitly handled; the function will treat it as out of bounds because `count < index - 1` with a negative `index` may cause unexpected behavior. The implementation does not guard against negative indices; it should be assumed that callers supply non‑negative indices.
- **Index 0 deletion**: Handled correctly; returns `head.next` which may be `None` for a single‑node list.
- **Index equal to length** (trying to delete after the last node): The traversal loop stops at the last node (`temp` is the last node, `count == length-1` but `index-1 == length`). Then `temp.next` is `None`, triggering the out‑of‑bounds condition.
- **Index greater than length**: Loop exhausts list, `temp` becomes `None`, out‑of‑bounds condition catches it.
- **Large index**: No special handling; the function will traverse until end or bound.

## Practical Use Cases

- **Implementing a list ADT**: Removing an element at a specific position (e.g., in Python’s `list.pop(i)` equivalent for a custom linked list).
- **Undo/redo functionality**: Removing an operation at a given index in a history list.
- **Text buffer operations**: Deleting a character at a cursor position.
- **Queue/Deque implementations**: When combined with head/tail pointers, deletion at specific indices is less common but may be used in certain scheduling algorithms.

## Limitations and Trade-offs

- **Forward‑only traversal**: Because it is a singly linked list, deletion at an arbitrary index requires traversing from the head each time. For frequent deletions at arbitrary positions, a doubly linked list or an array‑based structure may be more efficient.
- **No random access**: Indexing is not O(1); deletion always costs O(n) in the worst case, even if the index is near the head (though deletion at head is O(1) due to the special case).
- **Boundary handling**: The function prints error messages; in production code, raising exceptions (e.g., `IndexError`) would be more appropriate. The current design mixes I/O with logic, which may be undesirable.
- **Negative indices**: Not supported. Extending the function to accept negative indices (like Python lists) would require additional logic to compute the effective index from the end.