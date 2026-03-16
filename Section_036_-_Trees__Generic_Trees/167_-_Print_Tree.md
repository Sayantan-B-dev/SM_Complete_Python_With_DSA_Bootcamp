## Printing a Tree (Generic Tree)

The provided code defines a simple recursive function to print all nodes in a generic tree. This section explains how it works, its limitations, and how to improve it.

### Basic Recursive Print (Preorder Traversal)

```python
def print_tree(root):
    if root is None:          # Edge case: empty tree
        return
    print(root.data)          # Visit the root
    for child in root.children:
        print_tree(child)      # Recursively visit each child
```

This function performs a **preorder traversal**: it prints the current node's data first, then recursively prints each child subtree. The base case handles a `None` root, which is useful when the tree is empty.

#### Example Output

For the first tree built:

```
root = TreeNode(1)
child1 = TreeNode(2)
child2 = TreeNode(3)
child3 = TreeNode(4)
root.children.append(child1)
root.children.append(child2)
root.children.append(child3)
print_tree(root)
```

Output:
```
1
2
3
4
```

For the second tree:

```
root = TreeNode(1)
child1 = TreeNode(2)
child2 = TreeNode(3)
child3 = TreeNode(4)
root.children.append(child1)
child1.children.append(child2)
root.children.append(child3)
print_tree(root)
```

Output:
```
1
2
3
4
```
(Note: Here `child2` is a child of `child1`, not of root. The output is the same sequence because the recursion still visits all nodes; the order is: root 1, then child1 (2), then child1's child (3), then root's other child (4).)

### Problems with the Simple Print Function

1. **No structural information** – The output is just a flat list of node values. It does not show which nodes are children of which, making it hard to understand the tree's hierarchy.
2. **Recursion depth** – For very deep trees (e.g., a degenerate tree where each node has one child), recursion can cause a stack overflow if the tree depth exceeds Python's recursion limit (usually ~1000).
3. **Limited traversal** – Only preorder is shown; other traversals (postorder, level‑order) require different code.

### Improved Printing with Indentation

To visualize the tree structure, you can pass an additional parameter that tracks the current depth and print indentation accordingly.

```python
def print_tree_indented(root, level=0):
    if root is None:
        return
    print("  " * level + str(root.data))
    for child in root.children:
        print_tree_indented(child, level + 1)
```

**Example output for the second tree** (with `level` starting at 0):

```
1
  2
    3
  4
```

This clearly shows that `3` is a child of `2`, which is a child of `1`, and `4` is another direct child of `1`.

### Level‑Order Traversal (Breadth‑First)

If you need to print nodes level by level, use a queue (iterative approach) to avoid recursion entirely.

```python
from collections import deque

def print_level_order(root):
    if root is None:
        return
    queue = deque([root])
    while queue:
        node = queue.popleft()
        print(node.data, end=' ')
        for child in node.children:
            queue.append(child)
    print()
```

This prints nodes in breadth‑first order: root, then all nodes at depth 1, then depth 2, etc. For the second tree, output would be: `1 2 4 3`.

### Handling Deep Trees Iteratively (Preorder)

To avoid recursion depth limits, you can simulate preorder traversal with an explicit stack.

```python
def print_tree_iterative(root):
    if root is None:
        return
    stack = [root]
    while stack:
        node = stack.pop()
        print(node.data)
        # Push children in reverse order to maintain original left‑to‑right order
        for child in reversed(node.children):
            stack.append(child)
```

### Summary

- The simple recursive function works for small trees but lacks structural output.
- Adding indentation makes the hierarchy visible.
- For deep trees, use iterative approaches (stack or queue) to avoid recursion limits.
- Different traversals serve different purposes: preorder for copying, postorder for deletion, level‑order for breadth‑first search.

The choice of printing method depends on what information you need to convey about the tree.