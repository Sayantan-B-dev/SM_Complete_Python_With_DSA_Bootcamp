# Counting Nodes in an N-ary Tree

## 1. Conceptual Overview

An **N-ary tree** is a hierarchical data structure where each node can have **zero or more children**. Unlike a binary tree, which restricts each node to at most two children, N-ary trees accommodate any number of child nodes per parent.

Each node typically consists of two components:

| Component  | Purpose                                            |
| ---------- | -------------------------------------------------- |
| `val`      | Stores the value associated with the node          |
| `children` | A list (or collection) containing references to child nodes |

Example conceptual structure:

```
        1
      / | \
     2  3  4
    / \
   5   6
```

The objective is to **determine the total number of nodes present in the entire tree**.

Important observations about node counting:

- Every node contributes **exactly one** to the total count.
- The traversal must **visit every node exactly once** to ensure an accurate count.
- A **recursive approach** naturally aligns with the recursive definition of a tree.

---

## 2. Logical Flow of the Algorithm

The recursive strategy mirrors the tree’s definition.

### Base Case

If the root node is `None`, the tree is empty.

```
return 0
```

### Recursive Case

1. Count the current node itself.
2. Recursively count all nodes in every child subtree.
3. Sum all these values.

### Mathematical Representation

```
Total Nodes =
    1  (the current node)
    + count(nodes in child₁ subtree)
    + count(nodes in child₂ subtree)
    + ...
    + count(nodes in childₖ subtree)
```

---

## 3. Visual Recursive Traversal

Consider the example tree:

```
        1
      / | \
     2  3  4
    / \
   5   6
```

Traversal order (DFS pre-order):

```
count_nodes(1)
    → count_nodes(2)
        → count_nodes(5)
        → count_nodes(6)
    → count_nodes(3)
    → count_nodes(4)
```

Node visit sequence:

```
1 → 2 → 5 → 6 → 3 → 4
```

Total nodes counted:

```
6
```

---

## 4. Full Python Implementation

```python
# Definition for a Node in an N-ary tree
class Node:
    def __init__(self, val=None, children=None):
        """
        Constructor for the Node class.

        Parameters:
        val       -> value stored in the node
        children  -> list containing references to child nodes
        """
        self.val = val
        # If children list is not provided, initialize an empty list
        # This avoids unintended shared list references across nodes
        self.children = children if children is not None else []


def count_nodes(root):
    """
    Counts the total number of nodes present in an N-ary tree.

    Parameters
    ----------
    root : Node
        Root node of the N-ary tree.

    Returns
    -------
    int
        Total number of nodes in the tree.
    """
    # Base case: empty tree
    if root is None:
        return 0

    # Start count with the current node
    total_nodes = 1

    # Recursively count nodes in each child subtree
    for child in root.children:
        total_nodes += count_nodes(child)

    return total_nodes


# -------------------------------
# Example Tree Construction
# -------------------------------

# Leaf nodes
node5 = Node(5)
node6 = Node(6)

# Intermediate nodes
node2 = Node(2, [node5, node6])
node3 = Node(3)
node4 = Node(4)

# Root node
root = Node(1, [node2, node3, node4])

# Count total nodes
result = count_nodes(root)

print("Total Nodes:", result)
```

---

## 5. Expected Output

```
Total Nodes: 6
```

---

## 6. Step-by-Step Execution Trace

| Step | Function Call  | Returned Value |
| ---- | -------------- | -------------- |
| 1    | `count_nodes(1)` | 6              |
| 2    | `count_nodes(2)` | 3              |
| 3    | `count_nodes(5)` | 1              |
| 4    | `count_nodes(6)` | 1              |
| 5    | `count_nodes(3)` | 1              |
| 6    | `count_nodes(4)` | 1              |

Final computation:

```
1 + 3 + 1 + 1 = 6
```

---

## 7. Time Complexity Analysis

| Factor        | Explanation                        |
| ------------- | ---------------------------------- |
| Node visits   | Every node is visited exactly once |
| Work per node | Constant-time operations per node  |

Therefore:

```
Time Complexity = O(N)
```

Where:

```
N = total number of nodes
```

---

## 8. Space Complexity Analysis

Space usage is determined by the **maximum recursion depth** (the height of the tree).

In the worst case, the tree degenerates into a chain, causing the recursion depth to equal N.

```
Space Complexity = O(H)
```

Where:

| Symbol | Meaning            |
| ------ | ------------------ |
| `H`    | Height of the tree |

- Worst case:  `H = N`
- Balanced tree: `H ≈ log N`

---

## 9. Iterative Alternative Using Stack (DFS)

Recursive solutions rely on the **call stack**. An iterative DFS implementation avoids recursion depth limitations.

```python
def count_nodes_iterative(root):
    """
    Iterative method to count nodes using stack-based DFS traversal.
    """
    if root is None:
        return 0

    stack = [root]
    count = 0

    while stack:
        current_node = stack.pop()
        count += 1
        # Push all children onto the stack
        for child in current_node.children:
            stack.append(child)

    return count


print("Total Nodes (Iterative):", count_nodes_iterative(root))
```

---

## 10. Iterative Expected Output

```
Total Nodes (Iterative): 6
```

---

## 11. Recursive vs. Iterative Comparison

| Feature             | Recursive DFS           | Iterative DFS         |
| ------------------- | ----------------------- | --------------------- |
| Implementation      | Simpler, more concise   | Slightly longer       |
| Memory              | Uses call stack         | Uses explicit stack   |
| Readability         | Very clear              | Slightly more verbose |
| Stack Overflow Risk | Possible for deep trees | Avoided               |

---

## 12. BFS Alternative (Level Order Traversal)

A breadth‑first search (BFS) using a queue can also count nodes, though it is not necessary for this simple task.

```python
from collections import deque

def count_nodes_bfs(root):
    """
    Counts nodes using Breadth‑First Search traversal.
    """
    if root is None:
        return 0

    queue = deque([root])
    count = 0

    while queue:
        current = queue.popleft()
        count += 1
        for child in current.children:
            queue.append(child)

    return count


print("Total Nodes (BFS):", count_nodes_bfs(root))
```

Expected Output:

```
Total Nodes (BFS): 6
```

---

## 13. Key Study Notes

Essential concepts to retain:

- Trees are **naturally recursive structures**.
- Node counting is a **complete traversal problem** — every node must be visited.
- DFS (depth‑first) is typically the most straightforward approach.
- BFS works equally well but may be less intuitive for counting.
- Always handle the **empty tree** (`root is None`) as a base case.
- Avoid **shared mutable defaults** in constructors (e.g., using `children=None` then creating a new list).

Core formula to remember:

```
Nodes = 1 + sum(nodes in each child subtree)
```

---

## 14. Key Interview Takeaways

In technical interviews, you may be evaluated on:

- **Traversal strategy selection** (DFS vs. BFS) and its trade‑offs.
- **Understanding tree recursion** and how to translate it into code.
- **Edge cases** such as empty trees or single‑node trees.
- **Complexity analysis** (time O(N), space O(H)).
- **Iterative alternatives** when recursion depth could be a problem.

Mastering these points will help you confidently solve a wide variety of tree‑related problems.