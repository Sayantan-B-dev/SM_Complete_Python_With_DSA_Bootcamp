## Optimized BST Validation – Algorithmic Breakdown

### 1. BST Invariant
A binary tree is a valid Binary Search Tree (BST) if for every node:
- All values in the left subtree are **strictly less** than the node’s value.
- All values in the right subtree are **strictly greater** than the node’s value.
- The left and right subtrees themselves are BSTs.

Validating this property requires checking both the local ordering and the recursive structure.

---

### 2. Inefficiency of the Naïve Approach
The naïve method (not shown in the final snippet) typically uses separate functions to compute the minimum and maximum of a subtree:

```python
def find_max(node):
    if node is None: return -inf
    return max(node.data, find_max(node.left), find_max(node.right))

def find_min(node):
    if node is None: return inf
    return min(node.data, find_min(node.left), find_min(node.right))
```

For each node, the validation function would call `find_max(node.left)` and `find_min(node.right)`. Each of these calls traverses the entire subtree again, even though the same information may have been computed earlier.

**Consequences:**
- In a skewed tree, the work becomes Σ(k) for k = 1..n → O(n²).
- In a balanced tree, each node’s value is examined O(log n) times (once per ancestor), leading to O(n log n) total work.
- The algorithm repeatedly traverses overlapping subtrees, violating the principle of not recomputing what can be aggregated.

---

### 3. Optimized Single‑Pass Design
The optimized function `checkBST_optimized` performs a single depth‑first traversal and returns, for each subtree, three values:
- **is_bst**: boolean indicating whether the subtree is a valid BST.
- **min_val**: the smallest value present in the subtree.
- **max_val**: the largest value present in the subtree.

With this aggregated information, a node can validate itself in constant time by comparing:
- `left_max < node.data`  (all left values are smaller)
- `node.data < right_min` (all right values are larger)

Because the recursion processes each node exactly once, the total time is linear.

---

### 4. Step‑by‑Step Algorithm Walkthrough

#### 4.1 Helper Function `validate_subtree`
```python
def validate_subtree(current_node):
```
This inner function encapsulates the recursive logic. It returns a tuple `(is_bst, min_val, max_val)` for the subtree rooted at `current_node`.

---

#### 4.2 Base Case: Empty Subtree
```python
if current_node is None:
    return True, float('inf'), float('-inf')
```
- An empty tree is trivially a BST → `True`.
- **Sentinel values**:  
  - Minimum = `+∞` so that any finite parent’s `node.data < right_min` will be true when the right child is absent.  
  - Maximum = `-∞` so that `left_max < node.data` will be true when the left child is absent.  
  These sentinels make the comparison logic uniform without special‑case branching.

---

#### 4.3 Recursive Traversal
```python
left_is_bst, left_min, left_max = validate_subtree(current_node.left)
right_is_bst, right_min, right_max = validate_subtree(current_node.right)
```
- The function recursively validates the left and right subtrees and obtains their aggregated properties.
- These recursive calls process every node in the tree exactly once.

---

#### 4.4 Validation Condition
```python
current_is_bst = (
    left_is_bst and
    right_is_bst and
    left_max < current_node.data and
    current_node.data < right_min
)
```
- The subtree is a BST only if:
  1. The left subtree is a BST.
  2. The right subtree is a BST.
  3. The largest value in the left subtree is smaller than the current node’s value.
  4. The smallest value in the right subtree is larger than the current node’s value.

---

#### 4.5 Compute Subtree Aggregates
```python
current_min = min(left_min, current_node.data)
current_max = max(right_max, current_node.data)
```
- The minimum of the current subtree is the smaller of the left subtree’s minimum and the current node’s value. (The right subtree’s values are all larger, so they never affect the minimum.)
- The maximum of the current subtree is the larger of the right subtree’s maximum and the current node’s value. (The left subtree’s values are all smaller, so they never affect the maximum.)

Even when the subtree is invalid, the min and max are still computed correctly. This is essential because ancestors may need these values to evaluate their own conditions (e.g., a parent’s `right_min` might come from a subtree that is invalid but still has actual values).

---

#### 4.6 Return
```python
return current_is_bst, current_min, current_max
```
The helper returns the complete information for the caller.

---

#### 4.7 Outer Function
```python
def checkBST_optimized(root_node):
    is_bst_result, _, _ = validate_subtree(root_node)
    return is_bst_result
```
- The outer function calls the helper and discards the min/max values, returning only the boolean result for a clean interface.

---

### 5. Example Walkthrough (Invalid Tree)

Tree:
```
      10
     /  \
    5    15
        /  \
       6    20
```
Root `10`:

- Left subtree (root 5): returns `(True, 5, 5)`.
- Right subtree (root 15): calls `validate_subtree(15.left)` → node `6` returns `(True, 6, 6)`; `15.right` → node `20` returns `(True, 20, 20)`.  
  For node 15:  
  - `left_is_bst = True`, `right_is_bst = True`.  
  - `left_max = 6`, `15.data = 15`, `right_min = 20`.  
  - Condition: `6 < 15` and `15 < 20` → `True`.  
  - Min = `min(6, 15) = 6`, Max = `max(20, 15) = 20`.  
  Returns `(True, 6, 20)`.

Root 10:  
- `left_is_bst = True`, `right_is_bst = True`.  
- `left_max = 5`, `right_min = 6`.  
- Condition: `5 < 10` (true), `10 < 6` (false) → `current_is_bst = False`.  
- Min = `min(5, 10) = 5`, Max = `max(20, 10) = 20`.  
Returns `(False, 5, 20)`.

The outer function returns `False`, correctly identifying the violation.

---

### 6. Complexity Analysis

| Metric                | Value                   |
|-----------------------|-------------------------|
| Time                  | O(n)                    |
| Space (recursion)     | O(h) (h = tree height)  |
| Space (auxiliary)     | O(1)                    |

- Each node is visited exactly once. The work per node is constant (a few comparisons and `min`/`max` operations).
- The recursion stack depth equals the height of the tree. In the worst case (skewed), O(n); in a balanced tree, O(log n).
- No additional data structures are used.

---

### 7. Why This Approach Is Superior

1. **Linear Time**: Eliminates repeated traversals by computing and propagating aggregated values in a single pass.  
2. **No Redundant Work**: Each node’s value is examined only once.  
3. **Clean Information Propagation**: Returning `(is_bst, min, max)` provides a self‑contained description of any subtree, enabling constant‑time validation at the parent.  
4. **Sentinel Values Simplify Logic**: Using `inf` and `-inf` for empty subtrees removes the need for explicit checks for missing children.  
5. **Reusable Pattern**: The same technique (post‑order traversal with aggregated returns) applies to other tree properties, such as checking AVL balance or computing subtree sums.

---

### 8. Edge Cases Handled

| Scenario              | Behaviour                                                                 |
|-----------------------|---------------------------------------------------------------------------|
| Empty tree            | `checkBST_optimized(None)` → `True`                                       |
| Single node           | `checkBST_optimized(leaf)` → `True`                                       |
| Duplicate values      | Using strict inequalities (`<` and `>`) treats duplicates as invalid.     |
| Deep recursion        | For very deep trees, Python’s recursion limit may be exceeded. An iterative version (using an explicit stack) can avoid this. |

---

### 9. Summary
The optimized BST validation algorithm transforms the problem into a single depth‑first traversal that returns a tuple `(is_bst, min, max)` for each subtree. By eliminating redundant min/max computations, it achieves O(n) time complexity, making it suitable for large trees and production use. The pattern of propagating aggregated information is a fundamental technique in tree algorithms.