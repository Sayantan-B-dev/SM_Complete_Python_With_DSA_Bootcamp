```py
from predefinedBSTs import print_bst

class BSTNode:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None



def print_binary_tree(node):
    if node is None:
        return
    
    # Format: Node: L->LeftChild, R->RightChild
    print(f"{node.data}:", end=" ")
    
    if node.left:
        print(f"L->{node.left.data}", end=", ")
    else:
        print("L->None", end=", ")
    
    if node.right:
        print(f"R->{node.right.data}")
    else:
        print("R->None")
    
    # Recur for the left and right children
    print_binary_tree(node.left)
    print_binary_tree(node.right)

def sortedListToBST(l1):
    if(len(l1)==0):
        return None
    
    mid = len(l1)//2 #finding the middle index

    rootData = l1[mid]
    root = BSTNode(rootData)

    root.left = sortedListToBST(l1[:mid])
    root.right = sortedListToBST(l1[mid+1:])
    return root



root = sortedListToBST([1,2,3,4,5,6,7])
print_binary_tree(root)
```

## Algorithm Overview

The function `sortedListToBST` constructs a **height‑balanced** Binary Search Tree from a sorted array (or list) using a divide‑and‑conquer approach.  
The core principle: the middle element of the current subarray becomes the root, ensuring that the number of elements in the left and right subtrees differs by at most one. Recursively applying this to the left and right halves produces a BST that is balanced (height ≈ log₂ n).

---

## Step‑by‑Step Execution (Example: `[1, 2, 3, 4, 5, 6, 7]`)

1. **Initial call**: `sortedListToBST([1,2,3,4,5,6,7])`  
   - `len(l1) = 7` → `mid = 7 // 2 = 3`  
   - `rootData = l1[3] = 4`  
   - Create root node with value `4`.

2. **Left subtree**: `sortedListToBST(l1[:3])` → `[1,2,3]`  
   - `len = 3` → `mid = 1` → `rootData = 2`  
   - Node `2` created.  
     - Left of `2`: `sortedListToBST([1])`  
       - `len = 1` → `mid = 0` → `rootData = 1` → node `1` with `None` children.  
     - Right of `2`: `sortedListToBST([3])` → node `3` with `None` children.  
   - Subtree rooted at `2` is built.

3. **Right subtree**: `sortedListToBST(l1[4:])` → `[5,6,7]`  
   - Similar process yields node `6` as root, with left child `5` and right child `7`.

4. The resulting BST:
   ```
        4
       / \
      2   6
     / \ / \
    1  3 5 7
   ```

---

## Code Walkthrough

```python
def sortedListToBST(l1):
    if len(l1) == 0:
        return None
```

- **Base case**: If the input list is empty, return `None` (no node to create).

```python
    mid = len(l1) // 2
```

- Compute the middle index. For even‑length lists, integer division picks the left‑middle element, which still yields a balanced tree (the height difference between left and right is at most 1).

```python
    rootData = l1[mid]
    root = BSTNode(rootData)
```

- Extract the middle element and create the root node.

```python
    root.left = sortedListToBST(l1[:mid])
    root.right = sortedListToBST(l1[mid+1:])
```

- Recursively build the left subtree from the slice before `mid` and the right subtree from the slice after `mid`.  
  Slicing creates new list copies, which increases memory usage but simplifies the logic. For production, indices (start, end) are preferred to avoid copying.

```python
    return root
```

- Return the constructed node, which becomes part of the parent’s subtree.

---

## Complexity Analysis

| Metric            | Value                     |
|-------------------|---------------------------|
| Time              | O(n)                      |
| Space (recursive) | O(log n) (call stack)     |
| Space (lists)     | O(n log n) due to slicing |

- **Time**: Each node is created exactly once, and each element is processed once per level of recursion. The total work is O(n).
- **Space**:  
  - Recursion stack depth = O(log n).  
  - **Memory inefficiency**: List slicing (`l1[:mid]`) creates new sublists at each recursion level, leading to O(n log n) total allocated memory. A more efficient implementation would use indices to avoid copies.

---

## Edge Cases

- **Empty list**: Returns `None` (empty tree).
- **Single element**: Creates a leaf node.
- **Even length list** (e.g., `[1,2,3,4]`):  
  `mid = 2` → root = `3`. Left subtree from `[1,2]` will have root `1` with right child `2`; right subtree from `[4]` is leaf `4`. Tree is still balanced (height difference ≤ 1).
- **Duplicate values**: The algorithm works with duplicates, but the resulting BST may violate the strict ordering property (duplicates on one side). To preserve BST semantics, duplicates should be placed consistently (e.g., all ≤ root on left) and the algorithm adjusted accordingly.

---

## Observations

- The function builds a **balanced** BST because the middle element is always chosen, ensuring the left and right subtree sizes differ by at most 1.
- The resulting tree is not necessarily perfectly balanced for all input sizes, but its height is guaranteed to be ⌊log₂ n⌋ or ⌈log₂ n⌉.
- This approach is commonly used to initialize self‑balancing trees from sorted data or to create test cases.