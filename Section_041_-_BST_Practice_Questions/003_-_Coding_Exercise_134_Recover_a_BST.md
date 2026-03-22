```py 
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def recover_tree(root):
    first = second = prev = None
    stack = []
    current = root
    while current or stack:
        while current:
            stack.append(current)
            current = current.left
        current = stack.pop()
        if prev and current.val < prev.val:
            if not first:
                first = prev
            second = current
        prev = current
        current = current.right
    if first and second:
        first.val, second.val = second.val, first.val
    return root
```
# Table of Contents

1. **Full Commented Code**
2. **Problem Definition – Recover Binary Search Tree**
3. **Core Concepts from Lower Layer**
   - 3.1 TreeNode Structure
   - 3.2 In‑order Traversal of a BST
   - 3.3 Detecting Swapped Nodes
4. **Algorithm Description**
5. **Step‑by‑Step Walkthrough with Example**
   - 5.1 Initial State
   - 5.2 Traversal and Detection
   - 5.3 Swapping the Values
6. **ASCII Workflow Representations**
   - 6.1 Tree Structure
   - 6.2 Stack and Pointer Evolution
   - 6.3 Decision Diagram for Inversion Detection
7. **Conceptual Recursion Tree (In‑order Traversal)**
8. **Complexity Analysis**
9. **Conclusion**

---

## 1. Full Commented Code

```python
# Definition for a binary tree node.
class TreeNode:
    """
    Represents a single node in a binary tree.

    Attributes:
        val (int): Value stored in the node.
        left (TreeNode): Reference to the left child.
        right (TreeNode): Reference to the right child.
    """
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


def recover_tree(root: TreeNode) -> TreeNode:
    """
    Recovers a Binary Search Tree (BST) where exactly two nodes have been
    swapped by mistake. The function modifies the tree in place by swapping
    the values of the two erroneous nodes.

    The algorithm performs an iterative in‑order traversal using an explicit
    stack. While traversing, it detects the two nodes that violate the
    BST property (i.e., where the previous node's value is greater than the
    current node's value). After the traversal, the values of those two nodes
    are swapped, restoring the BST.

    Parameters:
        root (TreeNode): The root of the BST with two swapped nodes.

    Returns:
        TreeNode: The root of the recovered BST (the original root, unchanged
                  in structure, only values swapped).
    """
    # Variables to hold the two misplaced nodes
    first = None      # The first node that violates the order
    second = None     # The second node that violates the order
    prev = None       # The previously visited node in the in‑order traversal

    # Stack for iterative in‑order traversal
    stack = []
    current = root

    # Begin iterative in‑order traversal (left → root → right)
    while current or stack:
        # Go as far left as possible from the current node
        while current:
            stack.append(current)
            current = current.left

        # Pop the most recent node (the leftmost not yet processed)
        current = stack.pop()

        # Check for violation: previous node's value > current node's value
        if prev and prev.val > current.val:
            # First violation: set 'first' to the previous node
            if not first:
                first = prev
            # For the second violation (or if only one violation), set 'second'
            # to the current node. In case of two violations, the last violation
            # will update 'second' to the later node.
            second = current

        # Move forward: current becomes the previous node for the next step
        prev = current
        # Now move to the right subtree to continue the in‑order traversal
        current = current.right

    # After traversal, swap the values of the two misplaced nodes
    if first and second:
        first.val, second.val = second.val, first.val

    return root
```

---

## 2. Problem Definition – Recover Binary Search Tree

A Binary Search Tree (BST) is a tree where for every node:
- All values in the left subtree are **less than** the node’s value.
- All values in the right subtree are **greater than** the node’s value.

If exactly **two nodes** in such a BST are swapped by mistake (their values are exchanged), the BST property is broken. The task is to **recover the original BST** without changing the structure of the tree – only by swapping the values of the two erroneous nodes.

The given function `recover_tree` accomplishes this using an **iterative in‑order traversal**.

---

## 3. Core Concepts from Lower Layer

### 3.1 TreeNode Structure
Each node is a simple object with three attributes:
- `val` : the integer value stored.
- `left` : reference to the left child (or `None`).
- `right` : reference to the right child (or `None`).

This structure forms the foundation of any binary tree.

### 3.2 In‑order Traversal of a BST
In‑order traversal visits nodes in the order: **left subtree → node → right subtree**.  
For a BST, this order yields the values in **ascending sorted order**.

Example: In a correct BST, if you list the values as they are visited in‑order, you get a strictly increasing sequence.

### 3.3 Detecting Swapped Nodes
When two nodes are swapped, the in‑order sequence will contain **one or two places** where the order is violated (a larger value appears before a smaller one).  
Specifically:
- If the two swapped nodes are **adjacent** in the in‑order sequence, there will be **one violation**.
- If they are **not adjacent**, there will be **two violations**.

In both cases, the two nodes that need to be swapped are:
- **First violation:** the node that appears **before** the descending pair.
- **Second violation:** the node that appears **after** the descending pair (in the second violation, if present; otherwise the node that appears after the single violation).

The algorithm detects these violations during an in‑order traversal.

---

## 4. Algorithm Description

The algorithm performs an **iterative in‑order traversal** using a stack. It maintains three pointers:
- `prev` : the node visited in the previous step.
- `first` : the first node that violates the order (to be swapped).
- `second` : the second node that violates the order (to be swapped).

**Traversal steps:**
1. Start at the `root`. Use a stack to simulate recursion.
2. Push all left children onto the stack until you reach a leaf.
3. Pop a node – this is the next node in the in‑order sequence.
4. Compare its value with the value of `prev` (if `prev` exists).
   - If `prev.val > current.val`, a violation has occurred.
     - If this is the first violation, set `first = prev`.
     - Always set `second = current` (this captures the second node in the last violation).
5. Update `prev = current` and move to the right subtree.
6. Repeat until the stack is empty and no more nodes remain.

After the traversal, `first` and `second` point to the two nodes whose values must be swapped. Swap their values.

**Why this works:**  
The in‑order traversal normally yields a sorted sequence. When a violation occurs (`prev.val > current.val`), `prev` is a node that is larger than its successor – it must be one of the swapped nodes. The `current` is the node that is smaller than its predecessor – it must be the other swapped node. If there are two violations, the second violation updates `second` to the later node, leaving `first` unchanged. Swapping `first` and `second` restores the correct order.

---

## 5. Step‑by‑Step Walkthrough with Example

We use the following BST where two nodes have been swapped:

```
        3
       / \
      1   4
         / \
        2   5
```

The correct BST would have the values: 3, 1, 4, 2, 5? Actually, for a BST with these nodes, the correct inorder should be 1, 2, 3, 4, 5. But here the inorder is **1, 3, 2, 4, 5**. The nodes `3` and `2` are swapped (their values exchanged). The goal is to swap them back.

### 5.1 Initial State
- `first = None`
- `second = None`
- `prev = None`
- `stack = []`
- `current = root` (node with value 3)

### 5.2 Traversal and Detection

We trace the iterative in‑order traversal.

**Step 1 – Go left from root:**
- Push 3 → stack = [3]
- Move to left child (1)
- Push 1 → stack = [3, 1]
- Move to left child of 1 (None) → exit inner while

**Step 2 – Pop 1:**
- `current = 1` (pop from stack: [3])
- `prev` is None → no violation
- Update `prev = 1`
- Move to right child of 1 → None

**Step 3 – Pop 3:**
- `current = 3` (pop from stack: [])
- `prev` is 1, compare 1.val (1) vs 3.val (3): 1 < 3 → no violation
- Update `prev = 3`
- Move to right child of 3 → node 4

**Step 4 – Go left from node 4:**
- Push 4 → stack = [4]
- Move to left child (2)
- Push 2 → stack = [4, 2]
- Move to left child of 2 (None) → exit inner while

**Step 5 – Pop 2:**
- `current = 2` (pop from stack: [4])
- `prev` is 3, compare 3.val (3) vs 2.val (2): **3 > 2 → violation!**
  - `first` is None → set `first = prev` (node 3)
  - Set `second = current` (node 2)
- Update `prev = 2`
- Move to right child of 2 → None

**Step 6 – Pop 4:**
- `current = 4` (pop from stack: [])
- `prev` is 2, compare 2.val (2) vs 4.val (4): 2 < 4 → no violation
- Update `prev = 4`
- Move to right child of 4 → node 5

**Step 7 – Go left from node 5:**
- Push 5 → stack = [5]
- Move to left child of 5 (None) → exit inner while

**Step 8 – Pop 5:**
- `current = 5` (pop from stack: [])
- `prev` is 4, compare 4.val (4) vs 5.val (5): 4 < 5 → no violation
- Update `prev = 5`
- Move to right child of 5 → None

**Step 9 – End**: stack empty, current None → loop ends.

**Result:**  
`first = node(3)`, `second = node(2)`

### 5.3 Swapping the Values
Swap the values: `first.val` (3) and `second.val` (2) are exchanged. The tree becomes:

```
        2
       / \
      1   4
         / \
        3   5
```

Now the inorder is **1, 2, 3, 4, 5** – a valid BST.

---

## 6. ASCII Workflow Representations

### 6.1 Tree Structure

```
Initial Tree (with swapped nodes 3 and 2):
        3
       / \
      1   4
         / \
        2   5
```

### 6.2 Stack and Pointer Evolution

We represent the state after each pop operation (the moment we examine a node). The table shows the values of `current`, `prev`, `first`, `second`, and the stack content.

| Step | Action                     | `current` (val) | `prev` (val) | Violation? | `first` | `second` | Stack (top last) |
|------|----------------------------|-----------------|--------------|------------|---------|----------|------------------|
| 1    | Push leftmost path         | –               | None         | –          | None    | None     | [3,1]            |
| 2    | Pop 1                      | 1               | None         | No         | None    | None     | [3]              |
| 3    | Pop 3                      | 3               | 1            | No         | None    | None     | []               |
| 4    | Push left from 4           | –               | –            | –          | None    | None     | [4,2]            |
| 5    | Pop 2                      | 2               | 3            | **Yes**    | 3       | 2        | [4]              |
| 6    | Pop 4                      | 4               | 2            | No         | 3       | 2        | []               |
| 7    | Push left from 5           | –               | –            | –          | 3       | 2        | [5]              |
| 8    | Pop 5                      | 5               | 4            | No         | 3       | 2        | []               |

### 6.3 Decision Diagram for Inversion Detection

```
     Start
       |
       v
   [prev = None]
       |
       v
   Visit node (in‑order)
       |
       v
   Compare prev.val and current.val
       |
       +-- prev.val > current.val ?
             |
             +-- Yes --> First violation? → set first = prev (if not set)
             |            Always set second = current
             |
             +-- No  --> Continue
       |
       v
   Move to next node
```

---

## 7. Conceptual Recursion Tree (In‑order Traversal)

Although the implementation is iterative, the in‑order traversal can be understood recursively. The recursion tree shows the order of function calls if we had a recursive function `inorder(node)`. For our example, the recursive calls would be:

```
inorder(3)
├── inorder(1)
│   ├── inorder(None)
│   ├── visit(1)
│   └── inorder(None)
├── visit(3)
└── inorder(4)
    ├── inorder(2)
    │   ├── inorder(None)
    │   ├── visit(2)
    │   └── inorder(None)
    ├── visit(4)
    └── inorder(5)
        ├── inorder(None)
        ├── visit(5)
        └── inorder(None)
```

The order of `visit` calls is exactly the sequence we processed: `1 → 3 → 2 → 4 → 5`. The violations occur when `visit(2)` is called immediately after `visit(3)` and `prev` (which holds the value 3) is greater than `current` (2).

The iterative stack mimics this recursion depth‑first.

---

## 8. Complexity Analysis

- **Time Complexity:** O(n), where n is the number of nodes. Each node is pushed onto the stack once and popped once. The while loops traverse each edge at most twice (once when descending left, once when moving right). Constant work per node.
- **Space Complexity:** O(h), where h is the height of the tree, due to the stack. In the worst case (skewed tree), h = n, leading to O(n) space. In a balanced tree, h = O(log n).

---

## 9. Conclusion

The `recover_tree` function efficiently repairs a BST with two swapped nodes by leveraging an iterative in‑order traversal. It detects the out‑of‑order pairs by comparing each visited node with its predecessor. The algorithm stores the first and last nodes that violate the sorted order, then swaps their values, restoring the BST property in O(n) time and O(h) extra space. The iterative approach avoids recursion depth issues and clearly shows the stack‑based traversal. This method is both elegant and practical for fixing corrupted BSTs without altering the tree structure.