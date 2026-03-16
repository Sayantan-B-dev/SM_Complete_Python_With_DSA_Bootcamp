## Level‑Wise Input for a Generic Tree (Breadth‑First Construction)

This document explains how to construct a generic tree by taking input level by level (breadth‑first). It uses a queue to process nodes in the order they are created, ensuring that nodes at the same depth are handled before moving deeper. This approach is intuitive for users and avoids the deep recursion of the previous method.

### Table of Contents
1. [Overview](#overview)
2. [Algorithm](#algorithm)
3. [Step‑by‑Step Visualization](#step‑by‑step-visualization)
4. [Python Implementation](#python-implementation)
5. [Role of the Queue (Deque)](#role-of-the-queue-deque)
6. [Comparison with Recursive Input](#comparison-with-recursive-input)
7. [Time and Space Complexity](#time-and-space-complexity)
8. [Advantages and Disadvantages](#advantages-and-disadvantages)
9. [Use Cases](#use-cases)

---

## Overview

In a level‑wise (or breadth‑first) construction, nodes are created and linked in the order of their level. The root is created first, then all its children, then the children of those children, and so on. This matches the natural level‑order traversal and is easier for a user to provide input interactively because they do not need to know the entire tree structure in advance—they only need to specify the children of each node as they are processed.

The algorithm maintains a queue (first‑in, first‑out) to keep track of nodes whose children have not yet been entered. Initially, the queue contains the root. While the queue is not empty, we dequeue a node, ask for its number of children, create each child, link it, and enqueue the child for later processing.

---

## Algorithm

**Input**: User provides data for each node interactively.
**Output**: Root of the constructed generic tree.

1. **Create the root node**  
   - Prompt the user for the root’s data.
   - Create a `TreeNode` object with that data.
   - Initialize an empty queue (deque) and enqueue the root.

2. **Process nodes in the queue**  
   While the queue is not empty:
   - Dequeue the front node (call it `current`).
   - Ask the user for the number of children `current` has.
   - For each child index `i` from 1 to that number:
     - Prompt for the child’s data.
     - Create a new `TreeNode` with that data.
     - Append the new node to `current.children`.
     - Enqueue the new node (so its children can be entered later).

3. **Return the root** after the queue becomes empty.

---

## Step‑by‑Step Visualization

We will construct the following tree as an example:

```
        10
       /  \
     20    30
    /  \
   40  50
```

### Initial State
- Queue: empty  
- User enters root data: `10`  
- Create root node `10`  
- Enqueue root: queue = `[10]`

### Iteration 1
- Dequeue `10` → current = `10`  
- Prompt: "Enter number of children for 10" → user enters `2`  
- For child 1: enter data `20` → create node `20`, append to `10.children`, enqueue `20`  
- For child 2: enter data `30` → create node `30`, append to `10.children`, enqueue `30`  
- Queue now: `[20, 30]`

### Iteration 2
- Dequeue `20` → current = `20`  
- Prompt: "Enter number of children for 20" → user enters `2`  
- For child 1: data `40` → create `40`, append to `20.children`, enqueue `40`  
- For child 2: data `50` → create `50`, append to `20.children`, enqueue `50`  
- Queue now: `[30, 40, 50]`

### Iteration 3
- Dequeue `30` → current = `30`  
- Prompt: "Enter number of children for 30" → user enters `0`  
- No children created.  
- Queue now: `[40, 50]`

### Iteration 4
- Dequeue `40` → current = `40`  
- Number of children: `0`  
- Queue now: `[50]`

### Iteration 5
- Dequeue `50` → current = `50`  
- Number of children: `0`  
- Queue now: `[]` (empty)

**Tree construction complete.** The queue ensured that nodes were processed in the order they were created, i.e., level by level.

---

## Python Implementation

```python
from commons import TreeNode, print_tree_detailed
from collections import deque

def take_input_level_wise():
    # Step 1: Create root
    root_data = int(input("Enter the root data: "))
    root = TreeNode(root_data)

    # Step 2: Initialize queue with root
    queue = deque([root])

    # Step 3: Process nodes in BFS order
    while queue:
        current_node = queue.popleft()   # dequeue the front node

        num_children = int(input(f"Enter the number of children for node {current_node.data}: "))
        for i in range(num_children):
            child_data = int(input(f"  Enter data for child #{i+1} of node {current_node.data}: "))
            child_node = TreeNode(child_data)
            current_node.children.append(child_node)
            queue.append(child_node)      # enqueue for later processing

    return root

# Example usage
root = take_input_level_wise()
print_tree_detailed(root)
```

**Notes**:
- The code uses `int(input(...))` for simplicity; in practice you might want to handle other data types.
- The `commons` module contains the `TreeNode` class and the `print_tree_detailed` function (which prints each node followed by its children).

---

## Role of the Queue (Deque)

The queue is the core of the level‑wise construction. It serves two purposes:
- **Preserves order**: Nodes are processed in the same order they are created (FIFO). This guarantees that all nodes at level `L` are processed before any node at level `L+1` because children of level‑`L` nodes are enqueued after all level‑`L` nodes have been dequeued.
- **Tracks pending nodes**: It holds nodes whose children have not yet been entered. As we create a child, we immediately enqueue it, so its own children will be prompted later.

A **deque** (double‑ended queue) from the `collections` module is used because it provides O(1) `popleft()` and `append()` operations, which is efficient for this FIFO usage. A simple list with `pop(0)` would be O(n) and is not recommended.

---

## Comparison with Recursive Input

| Feature                  | Recursive Input                          | Level‑wise Input (Queue)                |
|--------------------------|------------------------------------------|-----------------------------------------|
| **Traversal order**      | Depth‑first (preorder)                   | Breadth‑first (level order)             |
| **User interaction**     | Must know deep structure in advance      | Input is given level by level           |
| **Memory usage**         | Call stack (O(depth))                     | Queue (O(width))                         |
| **Risk of recursion limit** | Yes, for deep trees                     | No recursion, safe for any depth        |
| **Ease of correction**   | Difficult – no going back                 | Easier – you only see one node at a time |
| **Implementation complexity** | Simple recursive code                  | Slightly more code (queue management)   |

---

## Time and Space Complexity

- **Time Complexity**: O(N), where N is the total number of nodes. Each node is created once and enqueued/dequeued once. The input operations are constant per node (assuming user input is not counted in algorithmic complexity).
- **Space Complexity**: O(W), where W is the maximum width of the tree (the largest number of nodes at any level). The queue holds at most one entire level at a time. In the worst case (a perfect tree), the last level contains about N/2 nodes, so space is O(N).

---

## Advantages and Disadvantages

### Advantages
- **Intuitive for users**: Input follows a natural top‑to‑bottom, left‑to‑right order.
- **No recursion**: Avoids stack overflow for deep trees.
- **Easy to implement and debug**: The queue logic is straightforward.
- **Flexible**: Can easily be adapted to build trees from other sources (e.g., reading a level‑order list from a file).

### Disadvantages
- **Requires user to know number of children in advance**: At each node, you must specify how many children it will have before entering them. This is the same as in the recursive method.
- **Input is still linear and error‑prone**: If you make a mistake in the child count, you may have to restart.
- **Not suitable for very large trees**: Interactive input becomes tedious for hundreds of nodes.
- **Assumes data is entered in level order**: If the user accidentally provides data in a different order, the tree structure will be incorrect.

---

## Use Cases

Level‑wise construction is commonly used:
- In **educational settings** to let students build small trees interactively.
- When **reading a tree from a file** that stores nodes in level order (e.g., serialized tree formats).
- As a building block for **tree deserialization** algorithms (where the input is a list of values with special markers for missing children).
- In **graphical tree builders** where nodes are added level by level.

Despite its limitations for large interactive input, the level‑wise approach is a fundamental technique for constructing trees breadth‑first and is widely used in practice for non‑interactive tree building from structured data.