# Largest Value in Each Level of an N‑ary Tree

## 1. Problem Overview

We are given an **N‑ary tree**, where each node can have zero or more children. Each node stores an integer value. The task is to find the **largest value** present on each level (row) of the tree and return these values in order from the root level to the deepest level.

For example, consider the tree:

```
                1
          /     |     \
         3      2      4
       /   \
      5     6
```

Levels:

| Level | Nodes       | Largest Value |
|-------|-------------|---------------|
| 0     | 1           | 1             |
| 1     | 3, 2, 4     | 4             |
| 2     | 5, 6        | 6             |

Expected output: `[1, 4, 6]`

---

## 2. Intuition and Approach

Because the problem asks for **per‑level** information, the natural choice is **Breadth‑First Search (BFS)**, also known as **level‑order traversal**. BFS processes nodes level by level, making it straightforward to aggregate information for each level.

### Key idea:
- Use a queue to hold nodes of the current level.
- For each level, iterate over all nodes currently in the queue (which represent that level).
- Track the maximum value seen among those nodes.
- After finishing the level, append that maximum to the result list.
- Enqueue all children of the processed nodes to prepare for the next level.

This approach visits every node exactly once and uses `O(1)` extra work per node.

---

## 3. Algorithm Steps

1. **Edge case**: If the root is `None`, return an empty list.
2. Initialize a queue (using `collections.deque`) with the root node.
3. Initialize an empty result list `result`.
4. While the queue is not empty:
   - Let `level_size = len(queue)` (number of nodes at the current level).
   - Initialize `level_max` to negative infinity (`float('-inf')`).
   - For _ in range(level_size):
     - Pop the front node from the queue.
     - Update `level_max` with the node’s value.
     - Enqueue all children of this node.
   - Append `level_max` to `result`.
5. Return `result`.

---

## 4. Python Implementation

```python
from collections import deque

class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children if children is not None else []

def largest_values(root):
    """
    Returns a list of the largest value in each level of the N-ary tree.
    """
    if not root:
        return []

    queue = deque([root])
    result = []

    while queue:
        level_size = len(queue)
        level_max = float('-inf')

        for _ in range(level_size):
            node = queue.popleft()
            level_max = max(level_max, node.val)

            # Add all children to the queue
            for child in node.children:
                queue.append(child)

        result.append(level_max)

    return result


# -------------------------------
# Example usage
# -------------------------------

# Build the example tree
node5 = Node(5)
node6 = Node(6)
node3 = Node(3, [node5, node6])
node2 = Node(2)
node4 = Node(4)
root = Node(1, [node3, node2, node4])

print(largest_values(root))   # Output: [1, 4, 6]
```

**Expected output**:

```
[1, 4, 6]
```

---

## 5. Complexity Analysis

- **Time Complexity**: `O(N)` – each node is processed exactly once.
- **Space Complexity**: `O(W)` where `W` is the maximum width (number of nodes in the widest level) of the tree. In the worst case (a perfectly balanced tree where the last level contains about `N/2` nodes), the queue can hold up to `O(N)` nodes.

---

## 6. Alternative Approach: DFS

While BFS is the most intuitive, a **DFS** solution is also possible by tracking depths.

```python
def largest_values_dfs(root):
    result = []

    def dfs(node, depth):
        if not node:
            return
        if depth == len(result):
            result.append(node.val)
        else:
            result[depth] = max(result[depth], node.val)

        for child in node.children:
            dfs(child, depth + 1)

    dfs(root, 0)
    return result
```

DFS also runs in `O(N)` time, but it uses recursion stack space of `O(H)` where `H` is the tree height.

---

## 7. BFS vs DFS Comparison

| Method | Traversal Order | Space (worst case) | Suitability for level problems |
|--------|-----------------|---------------------|---------------------------------|
| BFS    | Level by level  | O(W) ≈ O(N)         | Naturally fits                  |
| DFS    | Depth first     | O(H) ≈ O(N) (skewed)| Can be adapted, but less direct |

Both are acceptable; the choice depends on coding preference and whether recursion depth might be a concern.

---

## 8. Key Takeaways

- **Level‑oriented problems** (largest value, average, right side view) are best solved with **BFS**.
- The **queue‑based level‑order traversal** pattern is fundamental:  
  `while queue: for _ in range(len(queue)): process node; add children`
- The same pattern can be adapted for many variants (e.g., zigzag order, collecting all nodes per level).
- Always handle the empty tree base case.

This problem is a classic demonstration of how BFS can be used to aggregate per‑level data in a tree. Mastering it builds a strong foundation for more complex tree traversal challenges.