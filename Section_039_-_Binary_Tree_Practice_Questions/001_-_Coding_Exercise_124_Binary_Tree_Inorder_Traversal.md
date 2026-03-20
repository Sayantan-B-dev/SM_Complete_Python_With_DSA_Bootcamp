## 1. Definition and Concept Overview

Inorder traversal visits nodes of a binary tree in left-root-right order. The recursive implementation relies on the call stack to backtrack after processing left subtrees. The iterative approach replicates this behavior using an explicit stack, eliminating recursion overhead and avoiding stack depth limitations.

The function `inorder_traversal` accepts a `TreeNode` root and returns a list of node values in inorder sequence.

## 2. Core Principles and Internal Mechanics

The algorithm maintains two key structures:

- `result`: accumulates node values in traversal order.
- `stack`: holds nodes whose left subtrees have been partially processed, waiting for the node itself to be visited.

The invariant: whenever the stack is non‑empty or `current` points to a node, there remain nodes to be processed. The traversal proceeds in two phases:

1. **Descend left**: From the current node, repeatedly push the node onto the stack and move to its left child. This mimics the recursive call that goes left until `None`.
2. **Process and ascend**: Once `current` becomes `None`, the top of the stack contains the next node to visit (the leftmost unvisited node). Pop it, record its value, and set `current` to its right child. This corresponds to returning from the left recursion, processing the root, and then descending into the right subtree.

The stack effectively stores the nodes that have pending right subtrees, preserving the order of the recursion.

## 3. Step-by-Step Logical Breakdown

1. Initialize `result = []`, `stack = []`, `current = root`.
2. While `current` is not `None` or `stack` is not empty:
   - **Left descent**: While `current` is not `None`, push `current` onto `stack` and set `current = current.left`. This pushes the entire left spine of the subtree rooted at `current`.
   - **Visit**: Pop the top node from `stack` (the leftmost unvisited node), append its value to `result`.
   - **Move right**: Set `current = current.right` to start processing the right subtree of the visited node.
3. Return `result`.

## 4. Implementation (Python)

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def inorder_traversal(root: TreeNode) -> list[int]:
    """
    Iterative inorder traversal using an explicit stack.
    """
    result = []
    stack = []
    current = root

    while current or stack:
        # Traverse left until no left child exists.
        # This pushes all nodes on the path from current to the leftmost leaf.
        while current:
            stack.append(current)
            current = current.left

        # At this point current is None. The top of the stack is the node
        # whose left subtree has been fully processed.
        current = stack.pop()
        result.append(current.val)

        # Move to the right subtree; the while loop will then push its left spine.
        current = current.right

    return result
```

## 5. Time and Space Complexity Analysis

- **Time Complexity**: O(n), where n is the number of nodes. Each node is pushed onto the stack exactly once and popped exactly once.
- **Space Complexity**: O(h), where h is the height of the tree. The stack holds at most the nodes along a single root‑to‑leaf path. In the worst case (skewed tree), h = n, resulting in O(n) space. In a balanced tree, h = O(log n).

## 6. Edge Cases and Failure Scenarios

- **Empty tree**: `root = None`. The while loop condition fails immediately; returns an empty list.
- **Single node**: No left or right children. The inner while loop pushes the root, then pops it, records its value, and moves to `None`. Returns list with one element.
- **Left‑skewed tree**: All nodes have only left children. The inner while loop pushes all nodes onto the stack in descending order. The outer loop then pops them from the stack (LIFO), yielding the correct inorder sequence (since leftmost nodes are processed last when popped). Stack size equals n.
- **Right‑skewed tree**: Each node has only a right child. The inner while loop never executes because `current.left` is always `None`. The algorithm repeatedly pops the current node, records it, and moves to the right child. Stack size remains 0 or 1.
- **Duplicate values**: The algorithm does not rely on uniqueness; duplicates are handled correctly as they are stored in separate nodes.

## 7. Practical Use Cases

- **Binary Search Trees (BST)**: Inorder traversal of a BST yields sorted order. This is used for range queries, validation, or constructing sorted lists.
- **Serialization/Deserialization**: Inorder alone is insufficient to uniquely reconstruct a binary tree, but combined with preorder or postorder it aids in reconstruction algorithms.
- **Expression Trees**: Inorder traversal of an expression tree produces the infix notation (with parentheses).
- **Memory‑constrained environments**: Iterative traversal avoids recursion depth limits, making it suitable for deeply nested trees.

## 8. Limitations and Trade-offs

- **Space vs. recursion**: Both iterative and recursive implementations use O(h) auxiliary space, but the iterative version uses heap‑allocated stack memory, which is often larger and more flexible than the call stack. Recursion may cause stack overflow for very deep trees; iteration does not.
- **Code clarity**: Recursive inorder traversal is more concise and immediately conveys the left‑root‑right pattern. The iterative version requires explicit state management, which can obscure intent.
- **Modification during traversal**: The iterative approach does not allow easy modification of the tree during traversal without careful handling, similar to recursion.
- **No early termination**: The algorithm processes the entire tree; it cannot be easily adapted to stop at a condition without restructuring the loop.