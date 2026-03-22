```py 
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def find_predecessor_successor(root, key):
    predecessor = None
    successor = None
    current = root
    while current:
        if current.val == key:
            if current.left:
                temp = current.left
                while temp.right:
                    temp = temp.right
                predecessor = temp.val
            if current.right:
                temp = current.right
                while temp.left:
                    temp = temp.left
                successor = temp.val
            break
        elif key < current.val:
            successor = current.val
            current = current.left
        else:
            predecessor = current.val
            current = current.right
    return predecessor, successor
```
# Table of Contents

1. **Full Commented Code**
2. **Understanding Predecessor and Successor in a BST**
3. **Algorithm Description**
4. **Step‑by‑Step Walkthrough with Example**
5. **ASCII Workflow Representation**
6. **Traversal Logic as a Decision Tree**
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
        val (int): Value stored in the node.
        left (TreeNode): Reference to the left child.
        right (TreeNode): Reference to the right child.
    """
    def __init__(self, val=0, left=None, right=None):
        self.val = val          # Node value
        self.left = left        # Left subtree (values smaller)
        self.right = right      # Right subtree (values larger)

def find_predecessor_successor(root: TreeNode, key: int) -> tuple:
    """
    Finds the in‑order predecessor and successor of a given key in a BST.

    The in‑order predecessor is the largest node smaller than the key.
    The in‑order successor is the smallest node larger than the key.

    Parameters:
        root (TreeNode): Root of the BST.
        key (int): The target value.

    Returns:
        tuple: (predecessor, successor) where each is the value (int) or None.
    """
    predecessor = None          # Will hold the predecessor value
    successor = None            # Will hold the successor value
    current = root              # Start traversal from the root

    # Traverse the tree while we still have a node to examine
    while current:
        # Case 1: current node's value equals the key
        if current.val == key:
            # Predecessor: the rightmost node in the left subtree
            if current.left:
                temp = current.left
                # Move as far right as possible in the left subtree
                while temp.right:
                    temp = temp.right
                predecessor = temp.val

            # Successor: the leftmost node in the right subtree
            if current.right:
                temp = current.right
                # Move as far left as possible in the right subtree
                while temp.left:
                    temp = temp.left
                successor = temp.val

            # We've found the key; break out of the loop
            break

        # Case 2: key is smaller than current node's value
        elif key < current.val:
            # The current node is a candidate successor
            # (because it is greater than key and we are moving left)
            successor = current.val
            current = current.left      # Go left to find a smaller node

        # Case 3: key is greater than current node's value
        else:
            # The current node is a candidate predecessor
            # (because it is smaller than key and we are moving right)
            predecessor = current.val
            current = current.right     # Go right to find a larger node

    # Return the found values (None if not found)
    return predecessor, successor
```

---

## 2. Understanding Predecessor and Successor in a BST

In a Binary Search Tree, the **in‑order traversal** (left – root – right) visits nodes in sorted ascending order.

- **Predecessor** of a given key: the node that would appear **immediately before** the key in an in‑order traversal.  
  Equivalently, the largest value in the tree that is **strictly less than** the key.

- **Successor** of a given key: the node that would appear **immediately after** the key in an in‑order traversal.  
  Equivalently, the smallest value in the tree that is **strictly greater than** the key.

If the key is not present in the tree, the predecessor and successor are defined as the nearest values that would surround it if it were inserted.

The algorithm uses the BST property to locate the key and, along the way, keeps track of candidate predecessor and successor nodes.

---

## 3. Algorithm Description

The algorithm is **iterative** and performs a single traversal of the BST from the root, following the standard search rules. It maintains two variables:

- `predecessor`: the largest value encountered so far that is **less than** the key.
- `successor`: the smallest value encountered so far that is **greater than** the key.

**Traversal logic:**

1. Start at the root, with `predecessor` and `successor` initialized to `None`.
2. While `current` is not `None`:
   - If `current.val == key`:
     - The actual predecessor is the rightmost node in the left subtree (if any).
     - The actual successor is the leftmost node in the right subtree (if any).
     - Break the loop because we have found the exact node.
   - Else if `key < current.val`:
     - `current.val` is greater than the key, so it becomes a **candidate successor**.
     - Move left to find a smaller value that might be closer to the key.
   - Else (`key > current.val`):
     - `current.val` is smaller than the key, so it becomes a **candidate predecessor**.
     - Move right to find a larger value that might be closer to the key.
3. Return the final `predecessor` and `successor`.

This approach works whether the key exists in the tree or not. If the key is not present, the algorithm will traverse to a `None` leaf, and the last recorded candidates become the final predecessor and successor.

---

## 4. Step‑by‑Step Walkthrough with Example

We use the following BST:

```
         50
        /  \
      30    70
     /  \  /  \
   20  40 60  80
```

We will trace the algorithm for two cases:

### Case A: Key = 60 (present in the tree)

**Step 1:**  
- `current = 50`, `predecessor = None`, `successor = None`
- Compare: `60 > 50` → go right.
- `predecessor = 50` (since 50 < 60, it's a candidate predecessor).
- `current = 70`

**Step 2:**  
- `current = 70`, `predecessor = 50`, `successor = None`
- Compare: `60 < 70` → go left.
- `successor = 70` (since 70 > 60, it's a candidate successor).
- `current = 60`

**Step 3:**  
- `current = 60`, `predecessor = 50`, `successor = 70`
- `current.val == key` (60 == 60):
  - Left subtree exists (40). Find rightmost in left subtree:
    - `temp = 40` → `temp.right`? None → predecessor = 40.
  - Right subtree exists (None). So successor remains 70.
- Break.

**Result:** `(40, 70)`

---

### Case B: Key = 55 (not present in the tree)

**Step 1:**  
- `current = 50`, `predecessor = None`, `successor = None`
- `55 > 50` → go right.
- `predecessor = 50`
- `current = 70`

**Step 2:**  
- `current = 70`, `predecessor = 50`, `successor = None`
- `55 < 70` → go left.
- `successor = 70`
- `current = 60`

**Step 3:**  
- `current = 60`, `predecessor = 50`, `successor = 70`
- `55 < 60` → go left.
- `successor = 60`
- `current = None`

**Step 4:**  
- Loop ends (current is None). Return `(50, 60)`.

**Result:** `(50, 60)` – the predecessor is the largest value less than 55, which is 50; the successor is the smallest value greater than 55, which is 60.

---

## 5. ASCII Workflow Representation

We illustrate the traversal path using arrows and brackets. For **key = 60**:

```
Start: root = [50]
predecessor = None, successor = None

    [50]  (current)
      |
      | key 60 > 50  →  predecessor = 50
      v
    [70]  (current)
      |
      | key 60 < 70  →  successor = 70
      v
    [60]  (current)
      |
      | key == 60
      |   left subtree exists → predecessor = rightmost(40) = 40
      |   right subtree None → successor stays 70
      v
    Done
```

For **key = 55** (not present):

```
Start: root = [50]
predecessor = None, successor = None

    [50]  (current)
      |
      | key 55 > 50  →  predecessor = 50
      v
    [70]  (current)
      |
      | key 55 < 70  →  successor = 70
      v
    [60]  (current)
      |
      | key 55 < 60  →  successor = 60
      v
    None  (current)
      |
      | loop ends
      v
Return (predecessor=50, successor=60)
```

---

## 6. Traversal Logic as a Decision Tree

Although the algorithm is iterative, we can view the decision path as a tree of comparisons. The following ASCII diagram shows the traversal for a generic search, with candidate updates:

```
                       (root)
                         |
                         v
                ┌────────┴────────┐
                | current.val ?   |
                └────────┬────────┘
         key < current.val   key > current.val
                |                   |
                v                   v
         successor = current   predecessor = current
                |                   |
                v                   v
          go left               go right
                |                   |
                v                   v
        (repeat until found)   (repeat until found)
                |
                v
         when current.val == key:
           - predecessor = rightmost(current.left)
           - successor = leftmost(current.right)
```

The path taken is a single branch from the root to the node (or to the leaf where the key would be inserted). Every node visited updates either `predecessor` or `successor` because it is either on the left side of the search path (candidate successor) or on the right side (candidate predecessor).

---

## 7. Complexity Analysis

### Time Complexity
- **Best case:** The key is at the root. O(1) to find it, plus possibly O(log n) to find the rightmost left or leftmost right if subtrees exist. Overall O(log n) for a balanced tree.
- **Average case:** The search follows a path of length O(log n) (balanced BST). At the end, we might traverse a subtree to find the predecessor/successor, which also takes O(log n) in a balanced tree. So overall O(log n).
- **Worst case:** The tree is skewed (like a linked list). The search path can be O(n). Finding the predecessor/successor in a skewed subtree can also be O(n). Overall O(n).

### Space Complexity
- The algorithm uses **iterative** traversal with only a few variables. No recursion stack is used.
- Space complexity: O(1) auxiliary space.

---

## 8. Conclusion

The `find_predecessor_successor` function efficiently computes the in‑order predecessor and successor of a given key in a BST using a single pass. It leverages the BST property to update candidate values as it traverses down the tree. When the key is found, it directly extracts the predecessor from the left subtree’s rightmost node and the successor from the right subtree’s leftmost node. If the key is absent, the algorithm still returns the correct surrounding values. The iterative nature ensures constant extra memory, and the algorithm runs in time proportional to the height of the tree.