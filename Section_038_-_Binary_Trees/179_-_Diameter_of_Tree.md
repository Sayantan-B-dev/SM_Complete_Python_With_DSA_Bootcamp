## Diameter of a Binary Tree

### 1. What is the Diameter?

The **diameter** of a binary tree is the **length of the longest path** between any two nodes in the tree.  
The length is usually measured by the **number of edges** along the path.  

- For a single node tree, the diameter is **0** (no edges).  
- For a tree with two nodes (root and one child), the diameter is **1** (one edge between them).  

The path does **not** have to pass through the root; it can lie entirely within a subtree.  

---

### 2. Why `diameter = left_height + right_height` is Wrong

A common mistake is to assume the diameter is simply the sum of the heights of the left and right subtrees of the root.  
**Why it's wrong:**  
- The longest path might **not** pass through the root. It could be entirely inside the left subtree or inside the right subtree.  
- Example: a left‑skewed tree where the longest path is a chain inside the left subtree, not crossing the root.  

Thus, the diameter is the **maximum** of:
1. The diameter of the left subtree.  
2. The diameter of the right subtree.  
3. The **longest path that passes through the root** = `height(left)` + `height(right)`.  

---

### 3. Recursive Approach (Your Code)

You provided this code:

```python
from predefinedBT import predefined_binary_tree_inputs

def height(root):
    if(root is None):
        return 0
    left_height = height(root.left)
    right_height = height(root.right)
    heightOfTree = 1 + max(left_height, right_height)
    return heightOfTree

def diameter_of_a_tree(root):
    if(root is None):
        return 0
    leftHeight = height(root.left)
    rightHeight = height(root.right)

    left_diameter = diameter_of_a_tree(root.left)
    right_diameter = diameter_of_a_tree(root.right)

    ans = max(left_diameter, right_diameter, leftHeight + rightHeight)
    return ans
```

**How it works:**  
For each node, it computes:
- `leftHeight` and `rightHeight` (calling `height()` recursively).  
- `left_diameter` and `right_diameter` (calling itself on left and right).  
- Then takes the maximum of the three candidates.  

**Key observation:** The function `height()` is called **multiple times** for the same nodes because `diameter_of_a_tree` calls `height` for each node, and `height` itself recursively traverses the subtree. This leads to repeated calculations.

---

### 4. Complexity Analysis

#### Time Complexity (Your Code)
Let’s derive the recurrence.

- For a node, we call `height` on its left and right subtrees.  
- The function `height` visits every node in that subtree exactly once.  
- So at the root, we call `height` on the whole tree → O(n) work.  
- At the next level (roots of left and right subtrees), we again call `height` on their subtrees → each of size roughly n/2, so O(n/2) + O(n/2) = O(n) again.  
- This pattern repeats for each level of recursion.  

In a **balanced tree** (height log n), the total work is:  
O(n) + 2 × O(n/2) + 4 × O(n/4) + … = O(n log n) – not O(n).  
In a **skewed tree** (like a linked list), it becomes O(n²) because at the root we traverse n nodes, then at the next node we traverse n‑1 nodes, etc.  

**Mathematically:**  
Let T(n) = time for diameter on n nodes.  
T(n) = T(L) + T(R) + O(size of left subtree) + O(size of right subtree)  
= T(L) + T(R) + O(n).  
For a skewed tree, T(n) = T(n‑1) + O(n) → T(n) = O(n²).  
For a balanced tree, T(n) = 2T(n/2) + O(n) → T(n) = O(n log n).  

Your code is **O(n log n)** for balanced and **O(n²)** for skewed – not O(n) as you mentioned.  

#### Space Complexity
- The recursion stack depth equals the height of the tree.  
- For a balanced tree: O(log n).  
- For a skewed tree: O(n).  

---

### 5. Optimized O(n) Approach

We can compute both height and diameter in one postorder traversal.  

```python
def diameter_optimized(root):
    # Returns (height, diameter)
    if root is None:
        return (0, 0)
    left_height, left_diam = diameter_optimized(root.left)
    right_height, right_diam = diameter_optimized(root.right)
    height = 1 + max(left_height, right_height)
    diameter = max(left_diam, right_diam, left_height + right_height)
    return (height, diameter)

def diameter_of_tree(root):
    return diameter_optimized(root)[1]
```

This avoids repeated height calculations. Time: O(n), space: O(h).  

---

### 6. Visual Example

Consider the tree:

```
        1
       / \
      2   3
     / \   \
    4   5   6
```

- **Left subtree height** = 2 (nodes 2‑4 or 2‑5).  
- **Right subtree height** = 2 (nodes 3‑6).  
- Path through root: left height + right height = 2 + 2 = 4 edges (e.g., 4‑2‑1‑3‑6).  
- Left subtree diameter: path from 4 to 5 = 2 edges.  
- Right subtree diameter: only 3‑6 = 1 edge.  
- Overall diameter = 4.  

**ASCII Diagram:**
```
     1
    / \
   2   3
  / \   \
 4   5   6

Diameter path: 4 -> 2 -> 1 -> 3 -> 6  (4 edges)
```

---

### 7. Mathematical Complexity Comparison

| Tree Type | Your Code (Naïve) | Optimized Code |
|-----------|------------------|----------------|
| Balanced (n nodes) | O(n log n) | O(n) |
| Skewed (n nodes) | O(n²) | O(n) |

**Example: Skewed tree (like a chain)**  
For n = 5:  
Your code calls `height` on node 1 (5 nodes), node 2 (4 nodes), … → total calls = 5+4+3+2+1 = 15 → ~n²/2.  
Optimized code visits each node once → 5 operations.

**Example: Balanced tree (n = 7)**  
Your code: each level does O(n) work across the level; total ~ n log n = 7×2.8 ≈ 20.  
Optimized: 7 operations.

---

### 8. Conclusion

The diameter is the longest path between any two nodes in a binary tree.  
The recursive formula `max(left_diam, right_diam, left_height + right_height)` is correct, but your implementation recomputes heights multiple times, leading to O(n log n) or O(n²) time.  
The optimized O(n) solution computes both height and diameter in a single pass.  

**Key takeaway:** Always be aware of overlapping subproblems in tree recursions; use postorder or memoization to achieve linear time.