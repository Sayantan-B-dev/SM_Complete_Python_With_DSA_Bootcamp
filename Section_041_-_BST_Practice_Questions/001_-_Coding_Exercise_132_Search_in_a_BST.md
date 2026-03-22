```py 
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def search_bst(root, val):
    if not root:
        return None
    if root.val == val:
        return root
    if val < root.val:
        return search_bst(root.left, val)
    return search_bst(root.right, val)
```

# Table of Contents

1. **Full Commented Code**
2. **Binary Search Tree (BST) Property**
3. **Algorithm Description**
4. **Step-by-Step Walkthrough with Example**
5. **ASCII Workflow Representation**
6. **Recursion Tree**
7. **Complexity Analysis**
8. **Conclusion**

---

## 1. Full Commented Code

```python
# Definition for a binary tree node.
class TreeNode:
    """
    Represents a single node in a binary tree.
    
    Attributes:
        val (int): The value stored in the node.
        left (TreeNode): Reference to the left child node.
        right (TreeNode): Reference to the right child node.
    """
    def __init__(self, val=0, left=None, right=None):
        self.val = val          # Node value
        self.left = left        # Left child (subtree with smaller values)
        self.right = right      # Right child (subtree with larger values)

def search_bst(root: TreeNode, val: int) -> TreeNode:
    """
    Searches for a node with value 'val' in a Binary Search Tree (BST).
    
    Parameters:
        root (TreeNode): The root node of the BST.
        val (int): The target value to search for.
    
    Returns:
        TreeNode: The node containing the value, or None if not found.
    """
    # Base case: if the current node is None, the value is not in the tree.
    if not root:
        return None
    
    # If the current node's value equals the target, return the node.
    if root.val == val:
        return root
    
    # If the target value is less than the current node's value,
    # recursively search the left subtree (all values there are smaller).
    if val < root.val:
        return search_bst(root.left, val)
    
    # Otherwise, the target value is greater than the current node's value,
    # so recursively search the right subtree (all values there are larger).
    return search_bst(root.right, val)
```

---

## 2. Binary Search Tree (BST) Property

A Binary Search Tree is a binary tree that satisfies the following **invariant** for every node:

- All values in the **left subtree** are **less than** the node’s value.
- All values in the **right subtree** are **greater than** the node’s value.
- Both left and right subtrees are themselves BSTs (recursive definition).

This ordering property allows efficient searching, insertion, and deletion.

---

## 3. Algorithm Description

The `search_bst` function implements a **recursive binary search** on a BST.

**High‑level idea:**
- Start at the root.
- If the root is `None`, the tree is empty → value not found.
- If the current node’s value equals the target → return the node.
- If the target is less than the current node’s value, go left.
- If the target is greater, go right.
- Repeat until the value is found or a leaf’s child is reached.

**Key points:**
- The recursion reduces the problem size by moving one level down at each step.
- At each step, we eliminate approximately half of the remaining tree (if balanced).
- The function returns either a `TreeNode` (found) or `None` (not found).

---

## 4. Step-by-Step Walkthrough with Example

We will use the following BST as an example:

```
         50
        /  \
      30    70
     /  \  /  \
   20  40 60  80
```

We will search for the value `60`.

### Step 1: Initial call
- `search_bst(root, 60)`
- `root` = node with value `50`.
- `val` = `60`.
- Check: `root` is not `None`.
- Compare `root.val` (50) with `val` (60): 50 != 60.
- Since `val` (60) > `root.val` (50), we go right.

### Step 2: Recursive call to right subtree
- `search_bst(root.right, 60)` → `search_bst(node70, 60)`
- `node70.val` = 70.
- Compare 70 with 60: not equal.
- Since 60 < 70, go left.

### Step 3: Recursive call to left subtree of node70
- `search_bst(node70.left, 60)` → `search_bst(node60, 60)`
- `node60.val` = 60.
- Compare: 60 == 60 → found.
- Return `node60`.

### Step 4: Unwinding recursion
- The function returns `node60` back up the call stack.

**Result:** The node containing `60` is returned.

---

## 5. ASCII Workflow Representation

We represent the recursive search process with arrows and brackets.

### Example: Searching for `60` in the BST

```
Step 1: Start at root (50)
        [50]
         |
         v
        (50)  ->  is 50 == 60? No, 60 > 50 -> go right

Step 2: Move to right child (70)
        [50]
          \
          [70]
           |
           v
        (70)  ->  is 70 == 60? No, 60 < 70 -> go left

Step 3: Move to left child (60)
        [50]
          \
          [70]
          /
        [60]
         |
         v
        (60)  ->  is 60 == 60? Yes -> return node

Step 4: Return node (60) back up the chain
        [50] <- returns node60
          \
          [70] <- returns node60
          /
        [60] <- found
```

**Alternative linear flow:**

```
search_bst(root=50, val=60)
    root.val=50, val=60 → 50 != 60 and 60 > 50 → search_bst(root.right=70, 60)
        root.val=70, val=60 → 70 != 60 and 60 < 70 → search_bst(root.left=60, 60)
            root.val=60, val=60 → 60 == 60 → return node60
        return node60
    return node60
```

---

## 6. Recursion Tree

The recursion tree visualizes each call and its subsequent calls. The tree is drawn with **indentation** and **arrows**.

For the search of `60`:

```
search_bst(50, 60)
│
├── 50 != 60, 60 > 50 → call search_bst(70, 60)
│   │
│   ├── 70 != 60, 60 < 70 → call search_bst(60, 60)
│   │   │
│   │   └── 60 == 60 → return node(60)
│   │
│   └── return node(60)
│
└── return node(60)
```

**For a failed search** (e.g., searching for `65`):

```
search_bst(50, 65)
│
├── 50 != 65, 65 > 50 → call search_bst(70, 65)
│   │
│   ├── 70 != 65, 65 < 70 → call search_bst(60, 65)
│   │   │
│   │   ├── 60 != 65, 65 > 60 → call search_bst(None, 65)
│   │   │   │
│   │   │   └── root is None → return None
│   │   │
│   │   └── return None
│   │
│   └── return None
│
└── return None
```

Each call either returns a node (found) or propagates `None` upward.

---

## 7. Complexity Analysis

### Time Complexity
- **Best case:** The target is at the root → O(1).
- **Average case (balanced BST):** Each step discards half of the tree → O(log n), where n is the number of nodes.
- **Worst case (skewed BST):** The tree is essentially a linked list; each step moves down one level → O(n).

### Space Complexity
- **Recursive implementation:** Each recursive call adds a frame to the call stack. In the worst case (skewed tree), the recursion depth is n → O(n) stack space.
- **Iterative version** would use O(1) extra space, but here we use recursion.

---

## 8. Conclusion

The `search_bst` function is a classic example of recursive binary search on a BST. Its correctness relies on the BST property: at each node, we can decide to go left or right based solely on the comparison of the target with the current node’s value. The recursion naturally mirrors the tree structure, and the base case handles when the target is not present. By understanding the step‑by‑step flow and recursion tree, one can grasp how the algorithm efficiently narrows down the search space.