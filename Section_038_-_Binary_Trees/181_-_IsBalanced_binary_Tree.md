## Checking if a Binary Tree is Balanced

### 1. Definition
A binary tree is **balanced** if for every node, the height difference between its left and right subtrees is at most 1.  
Mathematically: `abs(height(left) – height(right)) ≤ 1` for all nodes.

---

### 2. Two Approaches

#### Approach 1: Separate Height and Balance Checks (Naïve)
- Compute the height of left and right subtrees separately (via recursive height function).
- Check the difference.
- Recursively check balance on left and right subtrees.

**Pseudo‑code:**
```python
def height(node):
    if node is None: return 0
    return 1 + max(height(node.left), height(node.right))

def isBalanced(node):
    if node is None: return True
    left_h = height(node.left)
    right_h = height(node.right)
    return abs(left_h - right_h) <= 1 and isBalanced(node.left) and isBalanced(node.right)
```

**Drawback:**  
For each node, `height` traverses the entire subtree, leading to repeated work.  
- Balanced tree: **O(n log n)** time.  
- Skewed tree: **O(n²)** time (like a linked list).

#### Approach 2: Combined Check (Optimized)
Compute height **and** balance in one pass. If any subtree is unbalanced, propagate that information upward without further computation.

**Idea:**  
Return `-1` to indicate an unbalanced subtree; otherwise return the height.  
The recursion:
1. Check left subtree – if it returns `-1`, propagate `-1`.
2. Check right subtree – if it returns `-1`, propagate `-1`.
3. Compare heights – if difference > 1, return `-1`.
4. Otherwise, return `height = 1 + max(left_height, right_height)`.

This ensures each node is visited only **once** → **O(n)** time.

---

### 3. Optimized Code Explanation

```python
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        def getHeightOrUnbalanced(node):
            """
            Returns:
            - height of the subtree if balanced
            - -1 if the subtree is unbalanced
            """
            # Base case: empty node has height 0
            if node is None:
                return 0

            # Recursively check left subtree
            left_height = getHeightOrUnbalanced(node.left)
            if left_height == -1:
                return -1    # left subtree unbalanced → propagate

            # Recursively check right subtree
            right_height = getHeightOrUnbalanced(node.right)
            if right_height == -1:
                return -1    # right subtree unbalanced → propagate

            # Check height difference at current node
            if abs(left_height - right_height) > 1:
                return -1    # current node makes the tree unbalanced

            # Otherwise, return the height of this node
            return max(left_height, right_height) + 1

        # Tree is balanced if the helper does not return -1
        return getHeightOrUnbalanced(root) != -1
```

**Step‑by‑step for a node:**
1. **Base:** Empty node → height 0.
2. **Left subtree:** Recursively get its height. If `-1`, left is unbalanced → whole tree unbalanced → propagate `-1`.
3. **Right subtree:** Same check.
4. **Balance test:** If the height difference > 1, this node is unbalanced → return `-1`.
5. **Return height:** Otherwise, the node is balanced, so compute and return `max(left_height, right_height) + 1`.

---

### 4. Complexity Analysis

| Complexity | Naïve Approach | Optimized Approach |
|------------|----------------|---------------------|
| **Time** | O(n log n) – balanced <br> O(n²) – skewed | **O(n)** – every node visited once |
| **Space** | O(h) recursion stack | O(h) recursion stack |

**Why O(n) for optimized:**  
Each node is processed exactly once. The work per node is constant (a few comparisons and arithmetic). Total work = n * constant = O(n).

**Why O(h) space:**  
Recursion depth equals the tree height `h`. In worst case (skewed tree), `h = n`, so O(n) space. In balanced tree, `h ≈ log n`, so O(log n) space.

---

### 5. Visual Example

Consider the tree:
```
        1
       / \
      2   3
     / \   \
    4   5   6
```

**Optimized recursion flow (root = 1):**

1. Process node 2:
   - Left child 4: returns height 1
   - Right child 5: returns height 1
   - Diff = 0 → balanced → return height 2
2. Process node 3:
   - Left child None: returns 0
   - Right child 6: returns height 1
   - Diff = 1 → balanced → return height 2
3. Process node 1:
   - left_height = 2, right_height = 2, diff = 0 → balanced → return height 3
4. Final result: `-1` never returned → tree is balanced.

If we insert a node 7 as left child of 6:
```
        1
       / \
      2   3
     / \   \
    4   5   6
           /
          7
```
- Node 6: left_height=1 (from 7), right_height=0 → diff=1 → balanced → height=2
- Node 3: left_height=0, right_height=2 → diff=2 → **return -1** → whole tree unbalanced.

---

### 6. Why the Optimized Version is Better

- **Efficiency:** Eliminates redundant height calculations by combining both tasks in one traversal.
- **Readability:** The helper function makes the intention clear – propagate an “unbalanced” marker.
- **Practicality:** Works for trees of any size without risking O(n²) slowdown.

The key insight is that once we know a subtree is unbalanced, we can stop further checks and immediately declare the whole tree unbalanced. This early termination is what saves time.

---

### 7. Summary

- A tree is balanced if every node satisfies `|height(left) - height(right)| ≤ 1`.
- The optimized approach returns `-1` to signal imbalance, otherwise returns the height.
- Time complexity is **O(n)**, space complexity is **O(h)** (worst‑case O(n) for skewed trees).
- This method is the standard solution used in coding interviews and real‑world applications.

---


## **LeetCode 110 — Balanced Binary Tree**

### **Problem Definition**

A binary tree is considered **height-balanced** if:

> For every node, the absolute difference between the heights of its left and right subtrees is **at most 1**.

---

## **Core Insight**

A naive approach repeatedly computes subtree heights, causing **O(n²)** complexity.

The optimal strategy combines:

* **Height calculation**
* **Balance validation**

into a **single DFS traversal**, achieving **O(n)** time complexity.

---

## **Optimal Approach — Bottom-Up DFS**

### **Key Idea**

* Traverse the tree **post-order** (left → right → node)
* At each node:

  * Get left height
  * Get right height
  * If imbalance detected → propagate failure immediately

---

## **Implementation (Python)**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val                      # Node value
#         self.left = left                    # Left child reference
#         self.right = right                  # Right child reference

class Solution:
    def isBalanced(self, root):
        
        # Helper function returns:
        # - height of subtree if balanced
        # - -1 if subtree is NOT balanced
        def compute_height_or_fail(current_node):
            
            # Base case: empty subtree has height 0
            if current_node is None:
                return 0
            
            # Recursively compute left subtree height
            left_subtree_height = compute_height_or_fail(current_node.left)
            
            # If left subtree already unbalanced, propagate failure
            if left_subtree_height == -1:
                return -1
            
            # Recursively compute right subtree height
            right_subtree_height = compute_height_or_fail(current_node.right)
            
            # If right subtree already unbalanced, propagate failure
            if right_subtree_height == -1:
                return -1
            
            # Check balance condition at current node
            if abs(left_subtree_height - right_subtree_height) > 1:
                return -1  # Not balanced
            
            # Return height of current subtree
            return 1 + max(left_subtree_height, right_subtree_height)
        
        # Tree is balanced if helper does NOT return -1
        return compute_height_or_fail(root) != -1
```

---

## **Execution Flow (Conceptual)**

```
          1
         / \
        2   3
       /
      4
```

### **Step-by-step**

* Node 4 → height = 1
* Node 2 → left=1, right=0 → balanced → height=2
* Node 3 → height=1
* Node 1 → left=2, right=1 → balanced → height=3

---

## **Failure Case**

```
          1
         /
        2
       /
      3
```

* Node 3 → height=1
* Node 2 → height=2
* Node 1 → left=2, right=0 → difference=2 → **unbalanced**

---

## **Complexity Analysis**

| Metric           | Value                      |
| ---------------- | -------------------------- |
| Time Complexity  | **O(n)**                   |
| Space Complexity | **O(h)** (recursion stack) |

* `n` = number of nodes
* `h` = tree height

---

## **Why This Works Efficiently**

* Each node is visited **exactly once**
* Early termination using `-1` avoids unnecessary computation
* Combines validation + height calculation in one pass

---

## **Expected Output Examples**

```python
# Example 1
Input: [3,9,20,null,null,15,7]
Output: True

# Example 2
Input: [1,2,2,3,3,null,null,4,4]
Output: False
```
