# Recursive Deletion of Node at Specified Index in a Singly Linked List

## Conceptual Definition

The function `delete_at_index_recursion(head, index)` removes the node located at a **0-based position** from a singly linked list using a **recursive traversal strategy**. The algorithm progresses through the list by repeatedly calling itself on the next node while decrementing the index until the deletion target is reached.

The recursion transforms the original problem:

> Delete node at index **i** from list starting at **head**

into progressively smaller sub-problems:

> Delete node at index **i-1** from list starting at **head.next**

When the recursive call reaches `index == 0`, the current node is removed by returning its successor. During the **stack unwinding phase**, each previous node reconnects its `next` pointer to the returned node, reconstructing the list without the deleted element.

---

# Internal Mechanics and Recursion Behavior

## Recursive Decomposition

Each recursive invocation performs two operations:

1. Advance to the next node in the list.
2. Decrease the index value by one.

The recursive relation can be described conceptually as:

```
delete(node, i) = node.next = delete(node.next, i-1)
```

The recursive calls continue until the deletion position becomes zero.

---

## Base Case Conditions

### Case 1 — Target Node Located

If `index == 0`, the function removes the current node.

```
return head.next
```

Returning `head.next` effectively **skips the current node**, causing it to be detached from the list.

---

### Case 2 — End of List Reached

If `head is None`, the function has traversed past the last node before reaching the desired index.

```
print("Index is Out of Bounds")
return None
```

This indicates that the requested position does not exist.

---

# Step-By-Step Execution Example

## Example: Delete Node at Index 2

Initial Linked List

```
head
 ↓
[A] → [B] → [C] → [D] → None
 0     1     2     3
```

Target node: **C**

---

## Forward Recursion Phase

### Call 1

```
delete(A,2)
```

Index not zero. Recursive call proceeds.

```
delete(B,1)
```

---

### Call 2

```
delete(B,1)
```

Index not zero. Recursive call continues.

```
delete(C,0)
```

---

### Call 3 (Deletion Occurs)

```
delete(C,0)
```

Target node found.

```
return C.next
```

Returned node is **D**.

---

## Stack Unwinding Phase

### Returning to Call 2

Current node: **B**

```
B.next = D
```

List becomes:

```
[A] → [B] → [D] → None
```

Return **B**

---

### Returning to Call 1

Current node: **A**

```
A.next = B
```

Final list:

```
[A] → [B] → [D] → None
```

Node **C** is now unreachable and effectively deleted.

---

# Recursive Stack Visualization

```
delete(A,2)
   ↓
delete(B,1)
   ↓
delete(C,0)  ← deletion happens here
   ↑
return D
   ↑
B.next = D
   ↑
A.next = B
```

---

# Implementation in Python

```python
class ListNode:
    def __init__(self, value=0, next_node=None):
        # Each node stores data and reference to the next node
        self.value = value
        self.next = next_node


def delete_at_index_recursion(head, index):
    """
    Recursively deletes a node from a singly linked list
    at a specified index position.

    Parameters
    ----------
    head : ListNode
        Head node of the linked list.

    index : int
        Zero based position of the node to remove.

    Returns
    -------
    ListNode
        Updated head of the linked list after deletion.
    """

    # Base Case 1
    # If the list ends before reaching the index
    if head is None:
        print("Index is Out of Bounds")
        return None

    # Base Case 2
    # When the target node is reached
    if index == 0:

        # Skip current node by returning its successor
        # This effectively removes the node
        return head.next

    # Recursive Case
    # Move one node forward while reducing index
    head.next = delete_at_index_recursion(head.next, index - 1)

    # Return current node so previous nodes reconnect properly
    return head
```

---

## Expected Output Example

### Input

```
Linked List:  A → B → C → D
Delete index: 2
```

### Output

```
A → B → D
```

---

# Algorithm Representation

## Recursive Algorithm

```
FUNCTION delete(node, index)

    IF node IS NULL
        PRINT "Index out of bounds"
        RETURN NULL

    IF index == 0
        RETURN node.next

    node.next = delete(node.next, index-1)

    RETURN node
```

---

# Time and Space Complexity

| Complexity Type  | Value | Explanation                                |
| ---------------- | ----- | ------------------------------------------ |
| Time Complexity  | O(n)  | Worst case traversal through entire list   |
| Space Complexity | O(n)  | Recursion stack grows with traversal depth |

Where `n` represents the number of nodes in the linked list.

---

# Edge Cases

## Empty Linked List

```
head = None
```

Function immediately prints error and returns `None`.

---

## Deleting the Head Node

```
delete(head,0)
```

Result:

```
new_head = head.next
```

Original head becomes detached.

---

## Index Equal to List Length

Example:

```
List length = 4
Index = 4
```

Traversal reaches `None` before index reaches zero.

Function prints:

```
Index is Out of Bounds
```

List remains unchanged.

---

## Index Greater Than Length

The recursion proceeds until the list ends. Because the last node already has `next = None`, assignments during unwinding do not modify the list structure.

---

## Negative Index

The algorithm does not validate negative values. The index continues decreasing until the list ends.

This produces unnecessary recursion and eventually triggers the **out-of-bounds base case**.

---

# Limitations

## Recursion Depth Limitation

Python has a default recursion depth limit of approximately **1000 stack frames**. Lists longer than this may cause a `RecursionError`.

---

## Inefficient Bounds Detection

The algorithm does not detect an invalid index early. It must traverse the entire list before realizing the index is invalid.

---

## Mixing Logic and Output

Printing error messages inside the function mixes algorithmic logic with input/output operations. Production implementations typically raise an exception instead.

Example:

```
raise IndexError("Index out of bounds")
```

---

## Mutation Side Effects

The function directly modifies the original linked list. The caller must **capture the returned head** because deletion at index `0` changes the list's starting node.

---

# Practical Educational Value

Recursive linked list operations demonstrate several key programming concepts:

* Recursive problem decomposition
* Pointer re-assignment during stack unwinding
* Structural manipulation of linked data
* Relationship between recursion depth and algorithm complexity

