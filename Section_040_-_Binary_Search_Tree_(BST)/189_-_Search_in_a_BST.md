## Searching in a BST

Searching in a Binary Search Tree exploits the ordering property to eliminate half of the remaining tree at each step. Given a target value, the search begins at the root and compares the target with the current node’s data:

- If equal, the node is found.
- If target < node.data, the search continues in the left subtree.
- If target > node.data, the search continues in the right subtree.

This process repeats until the value is found or a `None` pointer is reached, indicating the value is not present.

---

### Recursive Implementation

```python
from predefinedBSTs import create_predefined_bsts_manual

root1, root2, root3 = create_predefined_bsts_manual()

def search_in_BST(root, value):
    """
    Recursively search for a value in the BST.
    Returns the node containing the value, or None if not found.
    """
    # Base case: empty subtree or value not present
    if root is None:
        return None

    # Value found at current node
    if root.data == value:
        return root

    # Recursively search the appropriate subtree
    if value < root.data:
        return search_in_BST(root.left, value)
    else:  # value > root.data
        return search_in_BST(root.right, value)
```

**Explanation**:

- **Base case**: If the current node is `None`, the search path exhausted and the value does not exist.
- **Equality check**: If `root.data == value`, the node is returned immediately.
- **Recursive descent**: If `value < root.data`, the function calls itself on `root.left`; otherwise, it calls on `root.right`. The return value from the recursive call propagates upward.

---

### Iterative Implementation (Alternative)

While recursion is natural for tree operations, an iterative version avoids the risk of stack overflow on deep trees.

```python
def search_in_BST_iterative(root, value):
    """
    Iteratively search for a value in the BST.
    Returns the node containing the value, or None if not found.
    """
    current = root
    while current is not None:
        if current.data == value:
            return current
        elif value < current.data:
            current = current.left
        else:
            current = current.right
    return None
```

**Explanation**:

- A loop traverses the tree using a `current` pointer.
- At each step, the same comparison logic determines the direction.
- The loop exits when `current` becomes `None` (value not found) or when the value is matched.

---

### Complexity Analysis

| Aspect        | Complexity        |
|---------------|-------------------|
| Time (average)| O(log n)          |
| Time (worst)  | O(n) (degenerate tree) |
| Space (recursive) | O(h) (stack frames) |
| Space (iterative) | O(1) (no recursion) |

- **h** is the height of the tree (log n in balanced trees, n in worst case).
- The recursive version uses the call stack, which may be limited in Python (default recursion limit ~1000). For production code with large trees, the iterative version is safer.

---

### Edge Cases

- **Empty tree** (`root = None`): Both implementations return `None` immediately.
- **Value not present**: The search exhausts the appropriate path and returns `None`.
- **Duplicate values**: The standard BST property assumes unique keys. If duplicates are stored (e.g., in a separate data structure), the search may return the first encountered node; handling duplicates requires a defined policy.

---

### Example Usage

```python
# Search in root2 (a predefined BST)
node = search_in_BST(root2, 42)
if node:
    print(f"Found node with value {node.data}")
else:
    print("Value not found")
```

The function returns the node object, allowing further operations (e.g., access to left/right children) if needed.