```py
from predefinedBSTs import print_bst,create_predefined_bsts_manual

class BSTNode:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None


def find_max(node):
    if(node is None):
        return float("-inf")
    
    left_max = find_max(node.left)
    right_max = find_max(node.right)

    ans = max(left_max,right_max,node.data)
    return ans

def find_min(node):
    if(node is None):
        return float("inf")

    left_min = find_min(node.left)
    right_min = find_min(node.right)

    ans = min(left_min,right_min,node.data)
    return ans
    

def checkBST(root):
    if(root is None):
        return True # Base condition : Empty Tree is a BST
    
    left_max = find_max(root.left)
    right_min = find_min(root.right)

    left_BST = checkBST(root.left)
    right_BST = checkBST(root.right)

    ans = left_BST and right_BST and (left_max < root.data) and (root.data < right_min)

    return ans

def checkBST_Better(root):
    pass






def checkBST_limit(root,minimum,maximum):
    if(root is None):
        return True

    if(root.data < minimum or root.data > maximum):
        return False
    
    ansLeft = checkBST_limit(root.left,minimum,root.data-1)
    ansRight = checkBST_limit(root.right,root.data+1,maximum)

    return ansLeft and ansRight


root1,root2,root3 = create_predefined_bsts_manual()
print(checkBST_limit(root3,float('-inf'),float('inf')))

root4 = BSTNode(5)
root4.left = BSTNode(10)
root4.right = BSTNode(15)

print(checkBST_limit(root4,float('-inf'),float('inf')))
```

## BST Validation Algorithms

The code implements two approaches to verify whether a binary tree satisfies the Binary Search Tree (BST) invariant:

- For every node `N`:
  - All nodes in `N.left` have values **less than** `N.data`.
  - All nodes in `N.right` have values **greater than** `N.data`.

---

### 1. Naive Approach

#### 1.1 `find_max(node)`
```python
def find_max(node):
    if node is None:
        return float("-inf")
    left_max = find_max(node.left)
    right_max = find_max(node.right)
    ans = max(left_max, right_max, node.data)
    return ans
```
- **Purpose**: Returns the maximum value in the subtree rooted at `node`.
- **Algorithm**:
  1. Base case: `None` node returns `-inf` (so that `max` never selects it).
  2. Recursively compute `left_max` and `right_max` for the left and right subtrees.
  3. Return the maximum among `left_max`, `right_max`, and `node.data`.
- **Complexity**: O(n) for a subtree of size `n`, as every node is visited.

#### 1.2 `find_min(node)`
```python
def find_min(node):
    if node is None:
        return float("inf")
    left_min = find_min(node.left)
    right_min = find_min(node.right)
    ans = min(left_min, right_min, node.data)
    return ans
```
- **Purpose**: Returns the minimum value in the subtree.
- **Algorithm**: Symmetric to `find_max`, using `inf` as the neutral element for `min`.

#### 1.3 `checkBST(root)`
```python
def checkBST(root):
    if root is None:
        return True
    left_max = find_max(root.left)
    right_min = find_min(root.right)
    left_BST = checkBST(root.left)
    right_BST = checkBST(root.right)
    ans = left_BST and right_BST and (left_max < root.data) and (root.data < right_min)
    return ans
```
- **Algorithm**:
  1. Base case: empty tree is a BST.
  2. Compute `left_max` (largest value in left subtree) and `right_min` (smallest value in right subtree).
  3. Recursively verify that the left and right subtrees themselves are BSTs.
  4. Combine conditions:
     - Left subtree is a BST (`left_BST`)
     - Right subtree is a BST (`right_BST`)
     - All values in left subtree are less than `root.data` (`left_max < root.data`)
     - All values in right subtree are greater than `root.data` (`root.data < right_min`)
- **Inefficiency**: For each node, `find_max` and `find_min` traverse the entire subtree, leading to repeated passes over nodes. The total time complexity is **O(n²)** in the worst case (e.g., a degenerate tree).

---

### 2. Optimized Approach – Range Propagation

#### 2.1 Principle
Instead of recomputing subtree minima and maxima repeatedly, the invariant can be enforced by passing allowed value ranges down the recursion. At any node, its value must lie within `(min, max)`, and the ranges for its children are tightened accordingly:

- Left child must satisfy `(min, node.data - 1)` (if duplicates are disallowed).
- Right child must satisfy `(node.data + 1, max)`.

#### 2.2 `checkBST_limit(root, minimum, maximum)`
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
- **Parameters**:
  - `minimum`: the smallest allowed value for the current node (inclusive lower bound).
  - `maximum`: the largest allowed value for the current node (inclusive upper bound).
- **Algorithm**:
  1. Base case: `None` → valid.
  2. If the current node’s value falls outside `[minimum, maximum]`, return `False`.
  3. Recursively check the left subtree with an upper bound of `root.data - 1` (all left values must be smaller).
  4. Recursively check the right subtree with a lower bound of `root.data + 1` (all right values must be larger).
  5. Return `True` only if both recursive checks succeed.
- **Initial call**: `checkBST_limit(root, float('-inf'), float('inf'))` allows any values initially.

#### 2.3 Handling Duplicate Values
The code uses strict inequalities: `root.data - 1` and `root.data + 1`. If the tree permits duplicate values (e.g., all values ≤ root in left, ≥ root in right), the bounds would be `minimum` and `root.data` for left, and `root.data` and `maximum` for right. The strict version aligns with the standard BST property that left values are **strictly less** and right values are **strictly greater**.

#### 2.4 Complexity
- **Time**: Each node is visited exactly once → **O(n)**.
- **Space**: O(h) for the recursion stack, where h is tree height (worst O(n)).

---

### 3. Walkthrough with Examples

#### Example 1: Valid BST
Consider the tree rooted at `root3` from `create_predefined_bsts_manual()`.  
`checkBST_limit(root3, -inf, inf)`:

- Root node: value `10`. Passes bounds.
- Left subtree: bounds `(-inf, 9)`. Recursively checked.
- Right subtree: bounds `(11, inf)`. Recursively checked.
All nodes satisfy their ranges → returns `True`.

#### Example 2: Invalid BST
```python
root4 = BSTNode(5)
root4.left = BSTNode(10)
root4.right = BSTNode(15)
```
- Root node `5`: passes `(-inf, inf)`.
- Left child `10`: called with bounds `(-inf, 4)` because left range is `(-inf, 5-1) = (-inf, 4)`. `10` > `4` → returns `False` immediately.  
Thus `checkBST_limit` correctly identifies the violation.

---

### 4. Edge Cases and Limitations

| Scenario | Handling |
|----------|----------|
| Empty tree | Both methods return `True`. |
| Single node | Both return `True`. |
| Tree with duplicate values | Strict bounds (`-1`/`+1`) treat duplicates as invalid. Adjust bounds to `root.data` for inclusive handling. |
| Very large values / overflow | Using `float('inf')` and `float('-inf')` works for any integer. For user-defined types, custom sentinel values may be needed. |
| Deep recursion | Python’s recursion limit may be exceeded for large trees. Iterative stack or manual stack can be used. |

---

### 5. Summary

- **Naive method** (using `find_max`/`find_min`) is conceptually simple but performs redundant traversals, leading to O(n²) time.
- **Range‑based method** (`checkBST_limit`) achieves O(n) time by propagating allowed value ranges downward, checking each node exactly once.
- The optimized approach is the standard solution for BST validation in technical interviews and production code.