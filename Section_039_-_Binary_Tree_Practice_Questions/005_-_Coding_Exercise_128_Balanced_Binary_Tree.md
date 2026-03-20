## Table of Contents

1. Full Code with Detailed Inline Comments  
2. Overview and Definition of Balanced Binary Tree  
3. Core Principles and Internal Mechanics  
   - 3.1 The Sentinel Value (-1)  
   - 3.2 Early Termination  
4. Step-by-Step Algorithm Breakdown with ASCII Workflow  
   - 4.1 Example Tree Structure  
   - 4.2 Walkthrough of `height` Recursion  
5. Recursion Tree Representation  
   - 5.1 Recursion Tree for the Example  
   - 5.2 Stack Unwinding and Propagation of -1  
6. Time and Space Complexity Analysis  
7. Edge Cases and Failure Scenarios  
8. Practical Use Cases  
9. Limitations and Trade-offs  

---

## 1. Full Code with Detailed Inline Comments

```python
class TreeNode:
    """
    Binary tree node definition.
    """
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def is_balanced(root: TreeNode) -> bool:
    """
    Determine if a binary tree is height-balanced.

    A height-balanced binary tree is defined as a tree in which the depths
    of the two subtrees of every node never differ by more than 1.
    """
    def height(node: TreeNode) -> int:
        """
        Returns the height of the subtree rooted at `node` if it is balanced,
        otherwise returns -1.

        The height of an empty tree (node is None) is 0.
        For a non‑empty node, the function computes the height of its left and
        right children recursively. If either child returns -1 (indicating an
        imbalance somewhere deeper), the function immediately returns -1,
        propagating the failure upward. If both children are balanced, it
        checks the height difference at the current node. If the absolute
        difference exceeds 1, it returns -1. Otherwise, it returns the height
        of the current node: 1 + max(left_height, right_height).
        """
        # Base case: empty subtree.
        if not node:
            return 0

        # Recursively compute left subtree height.
        left_height = height(node.left)
        # If left subtree is imbalanced, propagate -1 upward.
        if left_height == -1:
            return -1

        # Recursively compute right subtree height.
        right_height = height(node.right)
        # If right subtree is imbalanced, propagate -1 upward.
        if right_height == -1:
            return -1

        # Check balance condition at this node.
        if abs(left_height - right_height) > 1:
            return -1

        # Return the height of this node.
        return 1 + max(left_height, right_height)

    # A tree is balanced if the height function does not return -1.
    return height(root) != -1
```

---

## 2. Overview and Definition of Balanced Binary Tree

A binary tree is **height‑balanced** (or simply “balanced”) if for every node in the tree, the heights of its left and right subtrees differ by at most 1. This definition is the basis for AVL trees and many other self‑balancing structures.

The function `is_balanced` implements a recursive algorithm that computes the height of each subtree while simultaneously verifying the balance condition. It returns `True` if the tree satisfies the condition, otherwise `False`.

---

## 3. Core Principles and Internal Mechanics

### 3.1 The Sentinel Value (-1)

The algorithm uses a **sentinel value** (-1) to signal that a subtree is not balanced. When `height` returns -1, it means the subtree rooted at the given node violates the balance condition either at that node or somewhere deeper.

Returning -1 instead of a numeric height serves two purposes:
- It carries an error condition up the recursion chain.
- It avoids separate bookkeeping (e.g., returning a tuple of (height, is_balanced)).

### 3.2 Early Termination

The recursion halts as soon as an imbalance is detected:
1. If `height(node.left)` returns -1, the function immediately returns -1 without exploring `node.right`.
2. Similarly, if `node.right` returns -1 after `node.left` was balanced, the function returns -1.
3. At the current node, if the absolute height difference exceeds 1, -1 is returned.

This **short‑circuiting** prevents unnecessary computation once an imbalance is found.

---

## 4. Step-by-Step Algorithm Breakdown with ASCII Workflow

Consider the following tree:

```
        A (val=1)
       / \
      B   C
     / \   \
    D   E   F
   /         \
  G           H
```

Where:
- Node A (root)
- B left child of A
- C right child of A
- D left child of B
- E right child of B
- F right child of C
- G left child of D
- H right child of F

We compute `height(A)`.

### 4.1 Workflow of `height` Calls

The recursion proceeds depth‑first, left‑to‑right.

**Step 1**: `height(A)`
   - Calls `height(B)` (left child).

**Step 2**: `height(B)`
   - Calls `height(D)` (left child).

**Step 3**: `height(D)`
   - Calls `height(G)` (left child).

**Step 4**: `height(G)`
   - `G.left` and `G.right` are `None`. Base case: `height(None) = 0`.
   - `left_height = 0`, `right_height = 0`.
   - `abs(0 - 0) <= 1` → balanced.
   - Returns `1 + max(0, 0) = 1` to `height(D)`.

**Step 5**: Back in `height(D)`, `left_height = 1`.
   - Calls `height(D.right)` = `height(None) = 0`.
   - `right_height = 0`.
   - `abs(1 - 0) = 1` ≤ 1 → balanced.
   - Returns `1 + max(1, 0) = 2` to `height(B)`.

**Step 6**: In `height(B)`, `left_height = 2`.
   - Calls `height(E)` (right child).

**Step 7**: `height(E)`
   - Both children `None`. Returns `1 + max(0,0) = 1` to `height(B)`.

**Step 8**: Back in `height(B)`, `right_height = 1`.
   - `abs(2 - 1) = 1` ≤ 1 → balanced.
   - Returns `1 + max(2, 1) = 3` to `height(A)`.

**Step 9**: In `height(A)`, `left_height = 3`.
   - Calls `height(C)` (right child).

**Step 10**: `height(C)`
   - `C.left` is `None`, returns 0.
   - Calls `height(F)` (right child).

**Step 11**: `height(F)`
   - `F.left` is `None`, returns 0.
   - Calls `height(H)` (right child).

**Step 12**: `height(H)`
   - Both children `None`. Returns 1 to `height(F)`.

**Step 13**: Back in `height(F)`, `left_height = 0`, `right_height = 1`.
   - `abs(0 - 1) = 1` ≤ 1 → balanced.
   - Returns `1 + max(0, 1) = 2` to `height(C)`.

**Step 14**: Back in `height(C)`, `left_height = 0`, `right_height = 2`.
   - `abs(0 - 2) = 2` > 1 → imbalance detected.
   - Returns **-1** to `height(A)`.

**Step 15**: `height(A)` receives `right_height = -1`. The earlier `left_height` was 3, but the sentinel triggers immediate return of -1.

**Step 16**: `height(root)` returns -1. `is_balanced` returns `False`.

### 4.2 ASCII Representation of Call Flow

Below is a condensed view of the recursive calls, with arrows indicating depth:

```
height(A) 
├─ height(B)
│  ├─ height(D)
│  │  ├─ height(G) → 1
│  │  └─ height(None) → 0
│  │  → returns 2
│  └─ height(E) → 1
│  → returns 3
└─ height(C)
   ├─ height(None) → 0
   └─ height(F)
      ├─ height(None) → 0
      └─ height(H) → 1
      → returns 2
   → imbalance detected → returns -1
→ returns -1
```

---

## 5. Recursion Tree Representation

A recursion tree visualizes each call, its children, and the returned values.

### 5.1 Recursion Tree for the Example

```
                          height(A)
                         /        \
                    height(B)    height(C)
                    /      \         \
              height(D)  height(E)  height(F)
              /      \               /      \
         height(G) height(None) height(None) height(H)
           |
      height(None)
```

Return values propagate upward:

```
height(None) = 0
height(G) = 1 + max(0,0) = 1
height(None) = 0
height(D) = 1 + max(1,0) = 2

height(E) = 1 + max(0,0) = 1
height(B) = 1 + max(2,1) = 3

height(None) = 0
height(None) = 0
height(H) = 1 + max(0,0) = 1
height(F) = 1 + max(0,1) = 2
height(C) = imbalance at node C → -1

height(A) = -1 (due to right subtree returning -1)
```

### 5.2 Stack Unwinding and Propagation of -1

When `height(C)` returns -1, the call stack unwinds:
1. `height(C)` returns -1 to `height(A)`.
2. `height(A)` receives -1 and immediately returns -1 to the top‑level call.
3. The top‑level call (`is_balanced`) checks `height(root) != -1`, sees `-1 != -1` is false, so returns `False`.

---

## 6. Time and Space Complexity Analysis

- **Time Complexity**: O(n), where n is the number of nodes. Each node is visited at most once because the recursion terminates early when an imbalance is found. In a perfectly balanced tree, every node is visited exactly once.
- **Space Complexity**: O(h), where h is the height of the tree. The recursion depth equals the height, and the call stack stores one frame per level. In a skewed tree, h = n, so space is O(n). In a balanced tree, h = O(log n), so space is O(log n).

---

## 7. Edge Cases and Failure Scenarios

| Case | Input | Expected Output | Notes |
|------|-------|-----------------|-------|
| Empty tree | `root = None` | `True` | Base case: `height(None) = 0`, not -1. |
| Single node | `root = TreeNode(1)` | `True` | Both children `None` → heights 0,0 → balanced. |
| Skewed tree (left only) | `1 -> 2 -> 3 -> ...` | `False` | At the deepest node, heights differ by >1. |
| Balanced but not perfectly | e.g., left height 2, right height 2 | `True` | Difference ≤ 1 at every node. |
| Imbalance at leaf’s parent | Node has left height 2, right height 0 | `False` | Detected at that node; early return. |
| Large tree exceeding recursion limit | Very deep (e.g., 2000 levels) | RecursionError | Python default recursion limit (~1000) will be exceeded. Iterative approach required. |

---

## 8. Practical Use Cases

- **AVL tree validation**: Ensuring an AVL tree maintains its balance property after insertions/deletions.
- **Data structure invariants**: Checking whether a binary tree satisfies the height‑balanced property for correctness of algorithms that rely on it.
- **Performance diagnostics**: Detecting degenerate trees that degrade search/insert time complexity from O(log n) to O(n).
- **Interview and competitive programming**: Common problem to test understanding of recursion and tree properties.

---

## 9. Limitations and Trade-offs

- **Recursion depth**: As noted, Python’s recursion limit may be exceeded for deep trees. An iterative post‑order traversal using an explicit stack can avoid this, though it increases code complexity.
- **Single‑purpose return value**: The sentinel -1 conflates two pieces of information (height and balanced status) into one integer. While efficient, it requires careful handling and can obscure intent for readers unfamiliar with the pattern.
- **No early exit on imbalance detection for entire tree**: Once -1 is returned, the algorithm stops exploring other branches. This is desirable but means the function cannot report which node caused the imbalance without additional bookkeeping.
- **Assumes binary tree nodes with `left` and `right` attributes**: The implementation is tightly coupled to the `TreeNode` class; modifications are required for other tree representations (e.g., adjacency lists).