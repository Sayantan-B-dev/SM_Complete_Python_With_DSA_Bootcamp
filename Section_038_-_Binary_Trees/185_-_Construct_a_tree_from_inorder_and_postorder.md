## Constructing a Binary Tree from Inorder and Postorder Traversals – A Detailed Walkthrough

You've already captured the core ideas in your notes. Now, let's expand them into a full, step‑by‑step guide that mirrors the earlier preorder+inorder explanation. We'll go from the basic concepts to a fully commented Python implementation, focusing on the **data flow** and the **index calculations** that make this work.

---

### 1. Recap: Inorder and Postorder Traversals

- **Inorder (Left → Root → Right):**  
  For any node, all nodes in its left subtree appear before it, and all nodes in its right subtree appear after it. This property is what allows us to split the tree.

- **Postorder (Left → Right → Root):**  
  The **last element** in the postorder sequence is always the root of the current tree (or subtree).  
  Before that root, the elements are grouped as: all nodes of the left subtree (in postorder order), then all nodes of the right subtree (in postorder order).

Given both traversals, we can reconstruct the tree recursively:

1. Take the **last element** of the postorder array → root.
2. Find that root in the inorder array → splits inorder into left and right parts.
3. The **size of the left subtree** tells us how many elements from the start of the postorder belong to the left subtree.
4. Recurse on the left and right subtrees using the appropriate slices.

---

### 2. The Recursive Function – Parameters and Roles

We'll define a recursive function that works on subarrays of the original inorder and postorder arrays.

**Function signature:**  
`build(inorder, postorder, inStart, inEnd, postStart, postEnd)`

- `inorder` and `postorder` are the original lists.
- `inStart`, `inEnd` define the current subtree in the inorder array (**inclusive**).
- `postStart`, `postEnd` define the current subtree in the postorder array (**inclusive**).

**What each parameter represents:**  
- `inStart` … `inEnd` → the inorder values for the current subtree.
- `postStart` … `postEnd` → the postorder values for the current subtree.

---

### 3. Algorithm (Step‑by‑Step)

1. **Base case:** If `inStart > inEnd` or `postStart > postEnd`, there are no nodes → return `None`.
2. **Root value:** The root is `postorder[postEnd]` (last element of the postorder slice).
3. **Create node:** Create a tree node with that value.
4. **Find root in inorder:** Search from `inStart` to `inEnd` for the root value. Let `rootIndex` be its index.
5. **Left subtree size:** `leftSize = rootIndex - inStart`.
6. **Recursively build left subtree:**
   - Inorder slice: `inStart` to `rootIndex - 1`.
   - Postorder slice: `postStart` to `postStart + leftSize - 1` (the first `leftSize` elements of the postorder slice belong to the left subtree).
7. **Recursively build right subtree:**
   - Inorder slice: `rootIndex + 1` to `inEnd`.
   - Postorder slice: `postStart + leftSize` to `postEnd - 1` (the elements after the left subtree up to just before the root).
8. **Attach children** and return the root.

---

### 4. Why the Index Calculations Work – Data Flow Explanation

The key is understanding how the postorder array is structured for a given subtree:

```
[ ... left subtree ... | ... right subtree ... | root ]
```

If we know the left subtree size (`leftSize`), then:
- The left subtree occupies positions `postStart` to `postStart + leftSize - 1`.
- The right subtree occupies positions `postStart + leftSize` to `postEnd - 1`.
- The root is at `postEnd`.

The inorder indices tell us the left subtree size because everything left of the root in inorder belongs to the left subtree.

**Example to internalise:**  
Suppose inorder = `[4,2,5,1,3,6]` and postorder = `[4,5,2,6,3,1]`.  
We'll trace the first call:

- `postEnd` = 5 → root = 1.
- Find 1 in inorder: at index 3 → `rootIndex = 3`.
- `leftSize = 3 - 0 = 3`.
- Left subtree:
  - Inorder: indices 0–2 → `[4,2,5]`
  - Postorder: indices 0–2 → `[4,5,2]`
- Right subtree:
  - Inorder: indices 4–5 → `[3,6]`
  - Postorder: indices 3–4 → `[6,3]` (notice: 6 is before 3 because left subtree of right subtree comes first)

The recursion continues.

---

### 5. Step‑by‑Step Code Writing (Junior Developer Style)

Let's write the Python code incrementally, with extensive comments to explain every decision.

#### Step 1: Node class and function stub

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def build_tree(inorder, postorder, in_start, in_end, post_start, post_end):
    # We'll fill this in
    pass
```

#### Step 2: Base case – when to stop

```python
def build_tree(inorder, postorder, in_start, in_end, post_start, post_end):
    # If no elements in this subtree, return None
    if in_start > in_end or post_start > post_end:
        return None
```

#### Step 3: Extract root from postorder

```python
    # Root is the last element of the current postorder slice
    root_val = postorder[post_end]
    root = TreeNode(root_val)
```

#### Step 4: Find root in inorder

We need the index of `root_val` within `inorder[in_start:in_end+1]`.  
We'll search linearly (we'll optimise later).

```python
    root_index = -1
    for i in range(in_start, in_end + 1):
        if inorder[i] == root_val:
            root_index = i
            break

    if root_index == -1:
        raise ValueError("Invalid traversals: root not found in inorder")
```

#### Step 5: Compute left subtree size

```python
    left_size = root_index - in_start
```

#### Step 6: Recursively build left subtree

We need the slices for left subtree:
- Inorder: from `in_start` to `root_index - 1`
- Postorder: from `post_start` to `post_start + left_size - 1`

We'll store them in variables for clarity:

```python
    left_in_start = in_start
    left_in_end   = root_index - 1
    left_post_start = post_start
    left_post_end   = post_start + left_size - 1

    root.left = build_tree(inorder, postorder,
                           left_in_start, left_in_end,
                           left_post_start, left_post_end)
```

#### Step 7: Recursively build right subtree

- Inorder: from `root_index + 1` to `in_end`
- Postorder: from `post_start + left_size` to `post_end - 1`

```python
    right_in_start = root_index + 1
    right_in_end   = in_end
    right_post_start = post_start + left_size
    right_post_end   = post_end - 1

    root.right = build_tree(inorder, postorder,
                            right_in_start, right_in_end,
                            right_post_start, right_post_end)
```

#### Step 8: Return the root

```python
    return root
```

#### Step 9: Initial call

```python
n = len(inorder)
root = build_tree(inorder, postorder, 0, n-1, 0, n-1)
```

---

### 6. Optimisation: Using a Hash Map for O(1) Lookup

As in the preorder+inorder case, we can pre‑compute a dictionary mapping each value to its index in inorder. This avoids the linear scan inside every recursive call, giving O(n) time.

**Updated code with map:**

```python
class Solution:
    def buildTree(self, inorder, postorder):
        # Build a map from value to its index in inorder
        inorder_index = {val: idx for idx, val in enumerate(inorder)}
        
        def helper(in_start, in_end, post_start, post_end):
            if in_start > in_end or post_start > post_end:
                return None
            
            root_val = postorder[post_end]
            root = TreeNode(root_val)
            
            # O(1) lookup of root index in inorder
            root_idx = inorder_index[root_val]
            
            left_size = root_idx - in_start
            
            root.left = helper(in_start, root_idx - 1,
                               post_start, post_start + left_size - 1)
            root.right = helper(root_idx + 1, in_end,
                                post_start + left_size, post_end - 1)
            return root
        
        return helper(0, len(inorder)-1, 0, len(postorder)-1)
```

---

### 7. Full Example with Trace

Let's run the code on your example:

```python
inorder   = [9, 3, 15, 20, 7]
postorder = [9, 15, 7, 20, 3]
```

**Initial call:**  
`in_start=0, in_end=4, post_start=0, post_end=4`  
Root = 3 (postorder[4]).  
Find 3 in inorder: index 1 → `root_idx=1`.  
`left_size = 1 - 0 = 1`.  
Left subtree: inorder indices 0–0 (`[9]`), postorder indices 0–0 (`[9]`) → node 9.  
Right subtree: inorder indices 2–4 (`[15,20,7]`), postorder indices 1–3 (`[15,7,20]`).

**Right subtree call:**  
`in_start=2, in_end=4, post_start=1, post_end=3`  
Root = 20 (postorder[3]).  
Find 20 in inorder (indices 2–4): index 3 → `root_idx=3`.  
`left_size = 3 - 2 = 1`.  
Left of 20 in inorder: `[15]` (index 2), postorder: indices 1–1 (`[15]`) → node 15.  
Right of 20 in inorder: `[7]` (index 4), postorder: indices 2–2 (`[7]`) → node 7.

Resulting tree:

```
    3
   / \
  9   20
     /  \
    15   7
```

---

### 8. Complexity Analysis

- **Time:** O(n) – each node is processed once, and the hash map lookup is O(1).
- **Space:** O(n) – recursion stack (worst‑case O(n) for a skewed tree) plus the output tree.

---

### 9. Common Pitfalls and Tips

- **Off‑by‑one in postorder slices:** The right subtree's postorder ends at `post_end - 1` because we excluded the root. The left subtree's end is `post_start + left_size - 1` (the first `left_size` elements).
- **Base condition order:** Always check both `in_start > in_end` and `post_start > post_end`. Either condition means an empty subtree.
- **Invalid input:** If `root_idx` is not found, raise an error – the traversals are inconsistent.
- **Test with small trees:** Single node, two nodes (left/right), full binary tree, skewed tree.
- **Use meaningful variable names:** `left_size` is more intuitive than `x`.

---

### 10. Final Code with Extensive Comments

```python
from typing import List

class TreeNode:
    """Binary tree node."""
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        """
        Construct a binary tree from inorder and postorder traversals.
        """
        # Pre‑compute inorder indices for O(1) lookup
        inorder_index = {val: i for i, val in enumerate(inorder)}
        
        def helper(in_start: int, in_end: int, post_start: int, post_end: int) -> TreeNode:
            """
            Build subtree from inorder[in_start..in_end] and
            postorder[post_start..post_end].
            """
            # Base case: empty subtree
            if in_start > in_end or post_start > post_end:
                return None
            
            # Root is the last element of postorder segment
            root_val = postorder[post_end]
            root = TreeNode(root_val)
            
            # Find root in inorder
            root_idx = inorder_index[root_val]
            
            # Number of nodes in left subtree
            left_size = root_idx - in_start
            
            # Build left subtree
            root.left = helper(
                in_start,               # inorder start for left subtree
                root_idx - 1,           # inorder end for left subtree
                post_start,             # postorder start for left subtree
                post_start + left_size - 1  # postorder end for left subtree
            )
            
            # Build right subtree
            root.right = helper(
                root_idx + 1,           # inorder start for right subtree
                in_end,                 # inorder end for right subtree
                post_start + left_size, # postorder start for right subtree
                post_end - 1            # postorder end for right subtree
            )
            
            return root
        
        # Initial call covering the entire arrays
        return helper(0, len(inorder)-1, 0, len(postorder)-1)
```

---

### 11. Summary

- **Key idea:** Postorder's last element = root; inorder splits left and right.
- **Index arithmetic:** `left_size` derived from inorder; used to split postorder.
- **Hash map** for O(1) root lookup.
- **Recursion** naturally builds the tree bottom‑up.


---

## **LeetCode 105 — Construct Binary Tree from Preorder and Inorder Traversal**

---

## **1. Problem Definition**

Given two arrays:

* `preorder` → traversal in **Root → Left → Right**
* `inorder` → traversal in **Left → Root → Right**

Construct the original binary tree.

---

## **2. Core Invariants (Non-Negotiable Rules)**

### **Preorder Property**

```text
First element of any preorder segment is ALWAYS the root
```

### **Inorder Property**

```text
Elements left of root → left subtree  
Elements right of root → right subtree
```

---

## **3. Recursive State Definition**

Define function:

```text
build(inStart, inEnd, preStart, preEnd)
```

This function constructs subtree using:

* `inorder[inStart … inEnd]`
* `preorder[preStart … preEnd]`

---

## **4. Root Extraction**

```text
root = preorder[preStart]
```

This is guaranteed by preorder traversal.

---

## **5. Partition Using Inorder**

Find:

```text
rootIndex = index of root in inorder
```

Then:

```text
Left subtree  = inorder[inStart … rootIndex - 1]
Right subtree = inorder[rootIndex + 1 … inEnd]
```

---

## **6. Critical Bridge: Subtree Size**

```text
leftSize = rootIndex - inStart
```

This determines how preorder splits.

---

## **7. Index Mapping (Most Critical Section)**

### **Preorder Structure**

```text
[ ROOT | LEFT SUBTREE | RIGHT SUBTREE ]
```

---

### **Left Subtree**

```text
inorder:  inStart → rootIndex - 1
preorder: preStart + 1 → preStart + leftSize
```

---

### **Right Subtree**

```text
inorder:  rootIndex + 1 → inEnd
preorder: preStart + leftSize + 1 → preEnd
```

---

## **8. Why This Works (Deep Reasoning)**

* Preorder tells **what comes first (root)**
* Inorder tells **how to split structure**
* Left subtree size aligns both traversals
* No ambiguity exists when values are unique

---

## **9. Algorithm (Deterministic Steps)**

1. If range invalid → return `None`
2. Take root from preorder
3. Find root index in inorder
4. Compute left subtree size
5. Recursively build:

   * Left subtree
   * Right subtree
6. Attach children
7. Return root

---

## **10. Pseudocode**

```text
FUNCTION build(inStart, inEnd, preStart, preEnd):

    IF inStart > inEnd OR preStart > preEnd:
        RETURN null

    rootValue = preorder[preStart]
    root = new TreeNode(rootValue)

    rootIndex = find rootValue in inorder

    leftSize = rootIndex - inStart

    root.left = build(
        inStart,
        rootIndex - 1,
        preStart + 1,
        preStart + leftSize
    )

    root.right = build(
        rootIndex + 1,
        inEnd,
        preStart + leftSize + 1,
        preEnd
    )

    RETURN root
```

---

## **11. Fully Annotated Python Implementation**

```python
# Definition for a binary tree node
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val                  # Node value
        self.left = left                # Left child pointer
        self.right = right              # Right child pointer


class Solution:
    def buildTree(self, preorder, inorder):
        
        # Step 0: Precompute inorder indices for O(1) lookup
        # Without this, each search becomes O(n), leading to O(n^2)
        inorder_index_map = {
            value: index for index, value in enumerate(inorder)
        }
        
        # Recursive function to construct subtree
        def build_subtree(in_start, in_end, pre_start, pre_end):
            

            # Base Case:
            # If no elements exist in this segment

            if in_start > in_end or pre_start > pre_end:
                return None
            

            # Step 1: Root comes from preorder

            root_value = preorder[pre_start]
            root_node = TreeNode(root_value)
            

            # Step 2: Find root position in inorder

            inorder_root_index = inorder_index_map[root_value]
            

            # Step 3: Compute left subtree size

            left_subtree_size = inorder_root_index - in_start
            

            # Step 4: Construct LEFT subtree
            #
            # preorder segment:
            #   pre_start + 1 → pre_start + left_subtree_size
            #
            # inorder segment:
            #   in_start → inorder_root_index - 1

            root_node.left = build_subtree(
                in_start,
                inorder_root_index - 1,
                pre_start + 1,
                pre_start + left_subtree_size
            )
            

            # Step 5: Construct RIGHT subtree
            #
            # preorder segment:
            #   pre_start + left_subtree_size + 1 → pre_end
            #
            # inorder segment:
            #   inorder_root_index + 1 → in_end

            root_node.right = build_subtree(
                inorder_root_index + 1,
                in_end,
                pre_start + left_subtree_size + 1,
                pre_end
            )
            

            # Step 6: Return constructed node

            return root_node
        
        # Initial call covering full arrays
        return build_subtree(
            0,
            len(inorder) - 1,
            0,
            len(preorder) - 1
        )
```

---

## **12. Execution Example**

```python
preorder = [3,9,20,15,7]
inorder  = [9,3,15,20,7]
```

---

### **Construction Flow**

```text
Root = 3

Inorder split:
[9] | 3 | [15,20,7]

Left subtree:
Root = 9

Right subtree:
Root = 20
Split:
[15] | 20 | [7]
```

---

## **13. Final Tree Structure**

```text
        3
       / \
      9   20
         /  \
        15   7
```

---

## **14. Complexity Analysis**

| Metric           | Value |
| ---------------- | ----- |
| Time Complexity  | O(n)  |
| Space Complexity | O(n)  |

---

## **15. Critical Takeaways**

* Preorder determines **root immediately**
* Inorder determines **subtree boundaries**
* Left subtree size aligns both traversals precisely
* Index-based recursion avoids array copying
* Hashmap is essential for optimal performance
