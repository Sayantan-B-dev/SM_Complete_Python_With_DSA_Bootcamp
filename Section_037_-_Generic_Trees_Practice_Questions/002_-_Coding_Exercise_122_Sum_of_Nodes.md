# Sum of All Nodes in an N-ary Tree

## 1. Conceptual Overview

An **N-ary tree** is a tree data structure where each node may have **zero or more children**. This generalizes the binary tree, which restricts each node to at most two children.

Each node typically contains two attributes:

| Attribute  | Description                               |
| ---------- | ----------------------------------------- |
| `val`      | An integer value stored in the node       |
| `children` | A list containing references to child nodes |

Example tree structure:

```
            10
        /    |    \
       5     3     7
     /   \
    2     4
```

The objective is to **compute the total sum of all node values** in the entire tree.

Mathematically, the sum can be expressed as:

```
Total Sum = 
    root.value
    + sum of values in child₁ subtree
    + sum of values in child₂ subtree
    + ...
    + sum of values in childₖ subtree
```

---

## 2. Logical Flow of the Algorithm

Because a tree is recursively defined, a **recursive depth‑first search (DFS)** is the most natural approach.

### Step‑by‑step reasoning

1. **Base case**: If the tree is empty (`root is None`), the sum is zero.
2. **Start** with the value of the current node.
3. **Traverse** each child node recursively, obtaining the sum of its entire subtree.
4. **Accumulate** all subtree sums.
5. **Return** the total accumulated value.

---

## 3. Recursive Flow Visualization

Consider the example tree:

```
            10
        /    |    \
       5     3     7
     /   \
    2     4
```

The recursive calls unfold in this order:

```
sum_of_nodes(10)
    → sum_of_nodes(5)
        → sum_of_nodes(2)
        → sum_of_nodes(4)
    → sum_of_nodes(3)
    → sum_of_nodes(7)
```

The sums bubble up:

```
2 + 4 = 6      (returned from node 5)
6 + 5 = 11     (node 5 adds its own value)
3              (node 3)
7              (node 7)
10 + 11 + 3 + 7 = 31
```

---

## 4. Full Python Implementation

```python
class Node:
    def __init__(self, val=None, children=None):
        """
        Constructor for an N-ary tree node.

        Parameters
        ----------
        val : int
            Value stored in the node.
        children : list
            List of child nodes.
        """
        self.val = val
        # Safely initialize children to avoid shared list references
        self.children = children if children is not None else []


def sum_of_nodes(root):
    """
    Computes the sum of all node values in an N-ary tree.

    Parameters
    ----------
    root : Node
        Root node of the N-ary tree.

    Returns
    -------
    int
        Sum of all node values.
    """
    # Base case: empty tree contributes nothing
    if root is None:
        return 0

    # Include current node's value
    total = root.val

    # Recursively add sums of all child subtrees
    for child in root.children:
        total += sum_of_nodes(child)

    return total


# -------------------------------
# Example Tree Construction
# -------------------------------

# Leaf nodes
node2 = Node(2)
node4 = Node(4)
node3 = Node(3)
node7 = Node(7)

# Intermediate node
node5 = Node(5, [node2, node4])

# Root node
root = Node(10, [node5, node3, node7])

# Compute sum
result = sum_of_nodes(root)

print("Sum of all nodes:", result)
```

---

## 5. Expected Output

```
Sum of all nodes: 31
```

---

## 6. Step‑by‑Step Execution Trace

| Step | Function Call    | Returned Value |
| ---- | ---------------- | -------------- |
| 1    | `sum_of_nodes(2)`  | 2              |
| 2    | `sum_of_nodes(4)`  | 4              |
| 3    | `sum_of_nodes(5)`  | 11 (5+2+4)     |
| 4    | `sum_of_nodes(3)`  | 3              |
| 5    | `sum_of_nodes(7)`  | 7              |
| 6    | `sum_of_nodes(10)` | 31 (10+11+3+7) |

---

## 7. Time Complexity Analysis

| Factor        | Explanation                        |
| ------------- | ---------------------------------- |
| Node visits   | Each node is visited exactly once  |
| Work per node | Constant‑time operations per node  |

Therefore:

```
Time Complexity = O(N)
```

where `N` is the total number of nodes.

---

## 8. Space Complexity Analysis

Space usage is dominated by the **maximum recursion depth** (the height of the tree).

```
Space Complexity = O(H)
```

| Symbol | Meaning            |
| ------ | ------------------ |
| `H`    | Height of the tree |

- Worst case (skewed tree): `H = N`
- Best / balanced case: `H ≈ log N`

---

## 9. Iterative DFS Alternative (Explicit Stack)

Recursion uses the call stack; an iterative version avoids recursion depth limits.

```python
def sum_of_nodes_iterative(root):
    """
    Computes node sum using iterative DFS with an explicit stack.
    """
    if root is None:
        return 0

    stack = [root]
    total = 0

    while stack:
        node = stack.pop()
        total += node.val
        # Push all children onto the stack
        for child in node.children:
            stack.append(child)

    return total


print("Sum using iterative DFS:", sum_of_nodes_iterative(root))
```

**Expected output:**

```
Sum using iterative DFS: 31
```

---

## 10. BFS Alternative (Level Order Traversal)

Breadth‑first search processes nodes level by level using a queue.

```python
from collections import deque

def sum_of_nodes_bfs(root):
    """
    Computes node sum using BFS (level order traversal).
    """
    if root is None:
        return 0

    queue = deque([root])
    total = 0

    while queue:
        node = queue.popleft()
        total += node.val
        for child in node.children:
            queue.append(child)

    return total


print("Sum using BFS:", sum_of_nodes_bfs(root))
```

**Expected output:**

```
Sum using BFS: 31
```

---

## 11. Comparison of Approaches

| Feature               | Recursive DFS | Iterative DFS | BFS          |
| --------------------- | ------------- | ------------- | ------------ |
| Implementation ease   | Very simple   | Moderate      | Moderate     |
| Memory                | Call stack    | Explicit stack| Queue        |
| Traversal order       | Depth‑first   | Depth‑first   | Level order  |
| Stack overflow risk   | Possible      | Avoided       | Avoided      |
| Suitability           | All tree ops  | All tree ops  | Level‑based ops |

All three methods achieve the same goal with `O(N)` time. The choice depends on personal preference and whether recursion depth might be a concern.

---

## 12. Key Study Notes

- **Tree problems often reduce to recursive subproblems** — the recurrence relation is:

  ```
  sum(node) = node.value + sum(child₁) + sum(child₂) + ... + sum(childₖ)
  ```

- The **for‑loop pattern** `for child in node.children:` appears in virtually every N‑ary tree traversal.
- **Base cases** are crucial: always handle `None` (empty tree) gracefully.
- **Complexity** is linear in the number of nodes (`O(N)`) because each node is visited once.
- **Space** is linear in the height (`O(H)`) for recursive DFS, but can be controlled with iterative approaches.
- The same recursive template can be adapted for:
  - Counting nodes
  - Finding maximum / minimum
  - Computing tree height
  - Checking if a value exists
  - Collecting all node values

---

## 13. Final Thought

The sum of all nodes in an N‑ary tree is a fundamental operation that demonstrates the power of recursion and tree traversal. Mastering this simple problem builds intuition for more complex tree algorithms.