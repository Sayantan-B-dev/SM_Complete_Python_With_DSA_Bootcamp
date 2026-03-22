## Algorithm: Print BST Elements in a Given Range

The function `print_bst_in_range(root, low, high)` traverses the BST and prints all node values that lie within the inclusive range `[low, high]`. It uses an **inorder traversal** with pruning to skip subtrees that cannot contain values in the range, thereby achieving efficiency.

---

### 1. Algorithm Logic

```python
def print_bst_in_range(root, low, high):
    if root is None:
        return

    # 1. Go left if there is a possibility of values >= low
    if low < root.data:
        print_bst_in_range(root.left, low, high)

    # 2. Print current node if it falls within the range
    if low <= root.data <= high:
        print(root.data, end=' ')

    # 3. Go right if there is a possibility of values <= high
    if high > root.data:
        print_bst_in_range(root.right, low, high)
```

#### Step‑by‑Step Explanation
- **Base case**: If the current node is `None`, return (nothing to process).
- **Left subtree traversal**:  
  Only traverse the left child if `low < root.data`.  
  Because in a BST, all values in the left subtree are less than `root.data`. If `root.data` is already ≤ `low`, then the entire left subtree consists of values even smaller, none of which can be ≥ `low`. Pruning this branch saves time.
- **Print current node**:  
  Check if `root.data` is between `low` and `high` (inclusive). If yes, print it. The `end=' '` keeps output on the same line with spaces.
- **Right subtree traversal**:  
  Only traverse the right child if `high > root.data`.  
  Since all values in the right subtree are greater than `root.data`, if `root.data` is already ≥ `high`, the right subtree contains only larger values, none of which can be ≤ `high`. Pruning this branch is safe.

The traversal is effectively a **restricted inorder** that visits only nodes whose subtrees may contain values in the range.

---

### 2. Example Walkthrough

Consider a BST built from `[50, 30, 70, 20, 40, 60, 80]` and call `print_bst_in_range(root, 25, 65)`.

- Root `50`:  
  `low=25 < 50` → go left.  
  `25 ≤ 50 ≤ 65` → print `50`.  
  `65 > 50` → go right.

- Left subtree (node `30`):  
  `25 < 30` → go left.  
  `25 ≤ 30 ≤ 65` → print `30`.  
  `65 > 30` → go right.

- Left of `30` (node `20`):  
  `25 < 20`? **False** → left branch skipped (all values in left subtree of 20 would be even smaller).  
  `25 ≤ 20 ≤ 65`? **False** → not printed.  
  `65 > 20` → go right (but `20.right` is `None`).  

- Right of `30` (node `40`):  
  `25 < 40` → go left (`40.left` is `None`).  
  `25 ≤ 40 ≤ 65` → print `40`.  
  `65 > 40` → go right (`40.right` is `None`).

- Right subtree (node `70`):  
  `25 < 70` → go left.  
  `25 ≤ 70 ≤ 65`? **False** → not printed.  
  `65 > 70`? **False** → right branch skipped (all values in right subtree are > 70, which are > 65).  

- Left of `70` (node `60`):  
  `25 < 60` → go left (`60.left` is `None`).  
  `25 ≤ 60 ≤ 65` → print `60`.  
  `65 > 60` → go right (`60.right` is `None`).

**Output**: `30 40 50 60 `

---

### 3. Complexity Analysis

| Metric | Value |
|--------|-------|
| Time   | **O(k + h)** where `k` is the number of nodes printed and `h` is the height of the tree. In the worst case (when the range covers most of the tree), `k ≈ n` and `h` can be up to `n`, giving O(n). But with pruning, it is often much faster. |
| Space  | O(h) due to recursion stack (worst O(n) for skewed trees). |

- **Best case**: If the range is empty or lies outside the tree’s values, the algorithm only traverses the path to the smallest or largest node, resulting in O(h) time.
- **Worst case**: When the range includes all nodes (e.g., `[-inf, inf]`), it becomes a full inorder traversal: O(n).

---

### 4. Edge Cases

| Scenario | Behaviour |
|----------|-----------|
| Empty tree (`root = None`) | Returns without printing anything. |
| `low > high` | No values satisfy `low ≤ root.data ≤ high`. The function will still traverse only necessary branches, but may still traverse some subtrees if the conditions `low < root.data` or `high > root.data` are true. Example: `low=50, high=40`. The root’s condition `low < root.data` may be true, leading to unnecessary left traversal. To avoid this, one could check `low <= high` at the start. |
| Single node | Prints the node if it lies within the range. |
| Large input | Recursion may exceed Python’s recursion depth for very deep trees. An iterative version (using an explicit stack) would be more robust. |

---

### 5. Why This Algorithm Is Efficient

- **Pruning**: By checking `low < root.data` before descending left and `high > root.data` before descending right, the algorithm avoids exploring entire subtrees that cannot contain any values in the range. This is a direct application of the BST property.
- **Inorder output**: Because the traversal follows an inorder pattern, the printed values appear in sorted order, which is often desirable for range queries.

---

### 6. Suggested Improvements

- **Handle invalid range**: Add an early check `if low > high: return` to avoid unnecessary traversal.
- **Iterative version**: Use an explicit stack to eliminate recursion depth concerns.
- **Return list**: Instead of printing directly, the function could collect values in a list and return it, allowing the caller to decide how to use the results.

---

### 7. Practical Use Cases

- **Range queries** in databases or search engines (e.g., find all items with price between $50 and $100).
- **Interval‑based reports** in inventory or financial systems.
- **Geographic information systems** for querying points within a bounding box (using 2D tree variants).

The algorithm is a classic example of how a BST can efficiently support range queries by leveraging its ordering property to prune branches.