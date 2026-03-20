## 1. Definition and Concept Overview

Postorder traversal visits nodes of a binary tree in left-right-root order. The recursive implementation processes the left subtree, then the right subtree, then the current node. The iterative version presented uses a single stack to simulate a reverse preorder traversal and then reverses the output to achieve the correct order.

The function `postorder_traversal` accepts a `TreeNode` root and returns a list of node values in postorder sequence.

## 2. Core Principles and Internal Mechanics

The algorithm exploits the fact that a reverse postorder (root-right-left) is the mirror of a preorder traversal. By performing a modified preorder that visits root, then right, then left, and then reversing the result, the correct left-right-root order is obtained.

Processing flow:
- Push the root onto the stack.
- Pop nodes from the stack, append their values to the result list.
- Push left child first, then right child. Because the stack is LIFO, the right child will be popped and processed before the left child. This yields the order: root, then right subtree (processed in the same root-right-left pattern), then left subtree.
- After the stack empties, `result` contains the sequence in root-right-left order. Reversing the list produces left-right-root.

No separate `current` pointer is needed; the stack holds all pending nodes.

## 3. Step-by-Step Logical Breakdown

1. If the root is `None`, return an empty list.
2. Initialize `stack` with `root` and `result` as an empty list.
3. While the stack is not empty:
   - Pop the top node from `stack`.
   - Append its value to `result`.
   - If the node has a left child, push it onto `stack`.
   - If the node has a right child, push it onto `stack`.
4. Reverse `result` and return it.

The reversal step transforms the accumulated root-right-left sequence into left-right-root.

## 4. Implementation (Python)

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def postorder_traversal(root: TreeNode) -> list[int]:
    """
    Iterative postorder traversal using a single stack and reversal.
    """
    if not root:
        return []

    stack = [root]
    result = []

    while stack:
        node = stack.pop()
        result.append(node.val)

        # Push left then right so that right is processed before left
        # in the current stack order.
        if node.left:
            stack.append(node.left)
        if node.right:
            stack.append(node.right)

    # Reverse the result to obtain postorder (left-right-root).
    return result[::-1]
```

## 5. Time and Space Complexity Analysis

- **Time Complexity**: O(n), where n is the number of nodes. Each node is pushed onto the stack once, popped once, and appended to the result list. The reversal of the list is also O(n).
- **Space Complexity**: O(n) in the worst case. The stack can hold up to all nodes (e.g., in a skewed tree). The result list also consumes O(n) space. The algorithm does not use recursion, so the call stack overhead is avoided.

## 6. Edge Cases and Failure Scenarios

- **Empty tree**: `root = None`. The early return yields `[]`.
- **Single node**: The stack contains the root; it is popped, value appended, no children. Reversal of a single‑element list returns the same list.
- **Left‑skewed tree**: Each node has only a left child. The algorithm pushes left child, then right (none). The order of processing is root, then left child, then left‑left child, etc. Result after reversal correctly gives leftmost to root.
- **Right‑skewed tree**: Each node has only a right child. The algorithm pushes left (none) then right. Processing order is root, then right child, then right‑right child, etc. Reversal yields the correct postorder (rightmost to root).
- **Large depth**: The stack uses heap memory, avoiding recursion depth limits. However, the algorithm still requires O(n) memory for the stack in worst‑case skewed trees.

## 7. Practical Use Cases

- **Tree deletion**: Postorder traversal ensures children are processed before the parent, making it suitable for deallocation or resource cleanup.
- **Expression evaluation**: Postorder (reverse Polish notation) eliminates the need for parentheses; evaluation uses a stack.
- **Computing subtree properties**: Operations like height, size, or subtree sums often require processing children before the parent.
- **Serialization with null markers**: Postorder sequences combined with null markers can reconstruct a tree, though less common than preorder.

## 8. Limitations and Trade-offs

- **Reversal overhead**: The algorithm requires O(n) additional space for the result list and an extra O(n) pass to reverse. Alternative two‑stack methods exist that produce the postorder directly without reversal, but they have similar space complexity.
- **Order sensitivity**: The correctness depends on pushing left then right. Swapping the order would produce a preorder‑like sequence after reversal (root-left-right), which is incorrect for postorder.
- **Code clarity**: The approach is concise but relies on the reversal trick, which may not be immediately obvious. Direct two‑stack implementations or using a single stack with state tracking (e.g., visited flag) may be more explicit.
- **Memory usage**: The result list is always allocated, even if the caller only needs to process values sequentially. A generator‑based approach would reduce memory for streaming use cases.