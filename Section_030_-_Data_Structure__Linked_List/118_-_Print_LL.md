## 1. Definition and Concept Overview

Printing a linked list refers to the operation of traversing each node from the head to the tail and outputting the data stored in each node. The goal is to produce a linear representation of the list’s contents. In a singly linked list, traversal is unidirectional: starting from the head, following the `next` references until `None` is reached.

The operation does not modify the list; it only reads the data. A correct print implementation must preserve the original structure and not lose the reference to the head.

## 2. Core Principles and Internal Mechanics

- **Head Reference**: The only entry point to the list. If the head is lost, the list becomes inaccessible.
- **Temporary Variable**: To traverse without altering the head, a temporary pointer (often named `temp` or `current`) is assigned the head’s value. This pointer is advanced through the list.
- **Termination Condition**: Traversal stops when the pointer becomes `None`. Attempting to access `data` or `next` on `None` raises an `AttributeError`.
- **Null Safety**: Every access to node attributes must be guarded by a check that the node is not `None`.

## 3. Step‑by‑Step Logical Breakdown

**Iterative Print**
1. Accept the head of the list as input.
2. Create a temporary variable `current` and set it to head.
3. While `current` is not `None`:
   - Print `current.data`.
   - Set `current = current.next`.
4. The loop exits when `current` becomes `None`. The original head remains unchanged.

**Recursive Print**
1. Base case: if the node is `None`, return (do nothing).
2. Print the current node’s data.
3. Recursively call the function with `node.next`.

Both methods produce the same output. The iterative method is preferred for large lists to avoid recursion depth limits.

## 4. Implementation (Python)

```python
class Node:
    """Node in a singly linked list."""
    def __init__(self, data):
        self.data = data
        self.next = None

def print_linked_list_iterative(head):
    """
    Prints the data of each node in the list, starting from head.
    Uses a temporary variable to preserve the original head.
    """
    current = head                # temporary pointer
    while current is not None:
        print(current.data)
        current = current.next    # move to next node

def print_linked_list_recursive(node):
    """
    Recursively prints the list. Base case: node is None.
    """
    if node is None:
        return
    print(node.data)
    print_linked_list_recursive(node.next)

# Example usage
if __name__ == "__main__":
    # Build a simple list: 1 -> 2 -> 3 -> None
    first = Node(1)
    second = Node(2)
    third = Node(3)
    first.next = second
    second.next = third
    head = first

    print("Iterative print:")
    print_linked_list_iterative(head)

    print("Recursive print:")
    print_linked_list_recursive(head)
```

**Key Implementation Notes**
- The iterative version uses a local variable `current`; the original `head` is never modified.
- The recursive version also passes the node reference by value, so no external state is altered.
- Both functions correctly handle an empty list (`head is None`) by printing nothing.

## 5. Time and Space Complexity Analysis

- **Time Complexity**: O(n) – each node is visited exactly once.
- **Space Complexity**:
  - Iterative: O(1) – only a single pointer variable is used.
  - Recursive: O(n) in the worst case due to the call stack (one frame per node). For large lists, recursion may cause stack overflow.

## 6. Edge Cases and Failure Scenarios

- **Empty List (head is None)**: The loop condition `current is not None` fails immediately; nothing is printed. Recursive base case handles it gracefully.
- **Single Node**: Both methods print the node’s data and then the pointer becomes `None`; correct.
- **Loss of Head**: If the function modifies the head parameter directly (e.g., `head = head.next` inside the loop), the caller’s head reference is unchanged (Python passes by value), but the local variable loses the original head. More critically, if the function is intended to print the list multiple times and the head is lost, subsequent prints become impossible. This is why a temporary variable is essential.
- **Node with `None` data**: The code prints `None`; this may be acceptable depending on the context.
- **Cyclic List**: If the list contains a cycle, the iterative loop will never terminate. In practice, printing assumes acyclic lists; cycle detection is a separate concern.

## 7. Practical Use Cases

- **Debugging**: Printing the list contents helps verify that nodes are correctly linked after insertions, deletions, or reversals.
- **Visualization**: Displaying the list in a human‑readable format (e.g., `1 -> 2 -> 3 -> None`) for documentation or logging.
- **Testing**: Asserting that the list state matches expected output after operations.

## 8. Limitations and Trade‑offs

- **Iterative vs. Recursive**: Recursive printing is elegant but risks stack overflow for long lists. Iterative printing is safer and more memory‑efficient.
- **Single‑Direction Traversal**: In a singly linked list, printing can only go forward. To print in reverse order, one would need to reverse the list first or use recursion (which implicitly uses the call stack to reverse output).
- **No Formatting Control**: The simple print functions output raw values on separate lines. For structured output (e.g., arrows between nodes), additional string building is required.
- **Loss of Head**: The most common mistake in linked list traversal is modifying the head reference. The documentation must emphasize using a temporary pointer to preserve the head.