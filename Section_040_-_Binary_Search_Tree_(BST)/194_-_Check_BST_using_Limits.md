```py
class BinaryTreeNode:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

def checkBST_limit(root, minimum,maximum):
    if(root is None):
        return True
    if(root.data<minimum or root.data>maximum):
        return False
    
    ansLeft=checkBST_limit(root.left,minimum,root.data-1)
    ansRight=checkBST_limit(root.right,root.data+1,maximum)

    return ansLeft and ansRight

```
## BST Validation Using Range Propagation (Top‑Down Approach)

The function `checkBST_limit` validates a binary tree by propagating **allowed value ranges** from the root downward. Instead of checking all values in subtrees, it tightens the permissible interval as it descends, ensuring that every node’s value lies within its current allowed range. This achieves O(n) time with a single traversal.

---

### Algorithm Overview

```python
def checkBST_limit(root, minimum, maximum):
    if root is None:
        return True
    if root.data < minimum or root.data > maximum:
        return False
    
    ansLeft = checkBST_limit(root.left, minimum, root.data - 1)
    ansRight = checkBST_limit(root.right, root.data + 1, maximum)
    
    return ansLeft and ansRight
```

- **Initial call**: `checkBST_limit(root, float('-inf'), float('inf'))`  
  The root can have any value, so the initial range is unbounded.

- **Recursive step**:  
  For the current node with value `v`, the range `(min, max)` defines the legal values it can take.  
  If `v` is outside this interval, the tree is invalid.

  The left child must contain only values **strictly less** than `v`, so its allowed maximum becomes `v - 1`. Its minimum remains the same as the current minimum.  
  The right child must contain only values **strictly greater** than `v`, so its allowed minimum becomes `v + 1`. Its maximum remains unchanged.

- **Base case**: An empty node is always valid.

---

### Step‑by‑Step Example

Consider the tree:
```
        10
       /  \
      5    15
     / \     \
    2   7     20
```

Call `checkBST_limit(root, -inf, inf)`:

1. **Root 10**:  
   `-inf < 10 < inf` → OK.  
   Left: range = `(-inf, 9)`. Right: range = `(11, inf)`.

2. **Node 5** (left of 10):  
   Range = `(-inf, 9)`. `-inf < 5 < 9` → OK.  
   Left of 5: range = `(-inf, 4)`. Right of 5: range = `(6, 9)`.

3. **Node 2** (left of 5):  
   Range = `(-inf, 4)`. `-inf < 2 < 4` → OK.  
   Both children are None → returns `True`.

4. **Node 7** (right of 5):  
   Range = `(6, 9)`. `6 < 7 < 9` → OK.  
   Children None → `True`.

5. **Node 15** (right of 10):  
   Range = `(11, inf)`. `11 < 15 < inf` → OK.  
   Left of 15: range = `(11, 14)`. Right of 15: range = `(16, inf)`.

6. **Left child of 15** is None → `True`.  
   **Node 20** (right of 15):  
   Range = `(16, inf)`. `16 < 20 < inf` → OK. Children None → `True`.

All nodes satisfy their ranges → returns `True`.

If we insert a node `6` as left child of `15` (violating BST), the range for `15.left` is `(11, 14)`. `6` is not `> 11`, so the function immediately returns `False` at that node, and the result propagates up.

---

### Complexity Analysis

| Metric | Value |
|--------|-------|
| Time   | O(n) — each node is visited exactly once. |
| Space  | O(h) — recursion stack depth, where h is tree height. |

This is optimal for BST validation because the tree must be traversed at least once.

---

### Edge Cases

| Scenario | Behaviour |
|----------|-----------|
| Empty tree | Returns `True`. |
| Single node | Returns `True`. |
| Duplicate values | The code uses strict inequalities (`-1`, `+1`). Any duplicate in the wrong subtree will fail (e.g., left child with same value would be invalid). |
| `low` > `high` | Can occur if the initial range is reversed, but the initial call uses `-inf` and `inf`. During recursion, the range may become empty (e.g., when `root.data - 1` < `minimum`), but the condition `root.data < minimum or root.data > maximum` will correctly detect violations. |

---

### Why This Approach Is Efficient

- **Single pass**: Unlike the naïve method that recomputed min/max per node, this approach performs only one traversal.
- **No extra storage**: It uses only the recursion stack; no auxiliary data structures.
- **Early exit**: The moment a node’s value falls outside its allowed range, the function returns `False` without traversing further subtrees (though the code as written still recurses into both children; a short‑circuited version could stop early).

---

### Comparison with Other BST Validation Methods

| Method | Time | Space | Approach |
|--------|------|-------|----------|
| **Naïve (min/max per node)** | O(n²) or O(n log n) | O(h) | Bottom‑up, recomputes subtree aggregates repeatedly |
| **Bottom‑up with aggregation** | O(n) | O(h) | Returns (is_bst, min, max) from children; checks `left_max < node < right_min` |
| **Top‑down with range** | O(n) | O(h) | Passes allowed interval down; checks node within bounds |

Both O(n) methods are optimal; the choice between them is a matter of style. The range‑based method is often preferred for its simplicity and early pruning potential.

---

### Limitations

- **Recursion depth**: For very deep trees (e.g., 10⁵ nodes), Python’s recursion limit may be exceeded. An iterative version using an explicit stack can solve this.
- **Strict inequalities**: The code enforces `left < node < right`. If duplicates are allowed (e.g., left ≤ node), the ranges must be adjusted accordingly.

---

### Practical Use Cases

- **Database index validation** after import.
- **Tree testing** in unit tests.
- **Pre‑condition check** before operations that assume BST properties.