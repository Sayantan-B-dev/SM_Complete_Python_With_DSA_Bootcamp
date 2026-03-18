## Table of Contents
1. [Binary Tree Node Definition](#1-binary-tree-node-definition)
2. [Recursive Print Function (Depth-First)](#2-recursive-print-function-depth-first)
   - 2.1 Algorithm Explanation
   - 2.2 Step-by-Step Execution on Sample Tree
   - 2.3 Recursion Tree Visualization
3. [Predefined Binary Tree Inputs](#3-predefined-binary-tree-inputs)
   - 3.1 Tree Structures in ASCII
4. [Level-Order Print Function (Breadth-First)](#4-level-order-print-function-breadth-first)
   - 4.1 Algorithm Explanation
   - 4.2 Step-by-Step Execution with Queue States
5. [Complete Example Usage](#5-complete-example-usage)
6. [Complexity Analysis](#6-complexity-analysis)
7. [Conclusion](#7-conclusion)

---

## 1. Binary Tree Node Definition

The fundamental building block of any binary tree is the node. In Python, we define a class `BinaryTreeNode` that holds the data and references to left and right children. A single node by itself is a valid binary tree (a leaf).

```python
class BinaryTreeNode:
    """
    Represents a node in a binary tree.
    Each node stores a piece of data and pointers to its left and right children.
    If a child does not exist, the pointer is None.
    """
    def __init__(self, data):
        """
        Constructor for a new node.
        :param data: The value to store in this node (can be any type).
        """
        self.data = data      # The node's value
        self.left = None      # Reference to the left child (BinaryTreeNode or None)
        self.right = None     # Reference to the right child (BinaryTreeNode or None)

    def __repr__(self):
        """Official string representation for debugging."""
        return f"BinaryTreeNode({self.data})"
```

**Explanation:**
- `data` holds the node's value. It could be a number, string, or any object.
- `left` and `right` are initially `None`, meaning the node has no children.
- The `__repr__` method helps when printing the node object directly, showing its data.

**What we can do with a node:**
- Create a leaf: `node = BinaryTreeNode(5)`
- Link nodes: `node.left = BinaryTreeNode(3)` attaches a left child.
- Navigate: `node.left.data` accesses the left child's value.

---

## 2. Recursive Print Function (Depth-First)

The function `print_binary_tree(root)` recursively prints each node in a **preorder** manner (root first, then left subtree, then right subtree). For each node, it prints the node's data, its left child's data (or `None`), and its right child's data (or `None`).

### 2.1 Algorithm Explanation

**Algorithm:**
1. If the current node (`root`) is `None`, return immediately (base case).
2. Print the current node's data followed by a colon and a space.
3. If a left child exists, print `"L->"` followed by the left child's data and a comma; otherwise print `"L->None,"`.
4. If a right child exists, print `"R->"` followed by the right child's data and a newline; otherwise print `"R->None"` and a newline.
5. Recursively call `print_binary_tree` on the left child.
6. Recursively call `print_binary_tree` on the right child.

This is a **depth-first traversal** because it goes deep into the left subtree before processing the right subtree.

### 2.2 Step-by-Step Execution on Sample Tree

Consider the tree built by:
```python
root = BinaryTreeNode(1)
root.left = BinaryTreeNode(2)
root.right = BinaryTreeNode(3)
```

**ASCII representation:**
```
    1
   / \
  2   3
```

We call `print_binary_tree(root)`. The steps are:

- **Step 1:** `root` is node(1). Not `None`. Print `"1: "`.
- **Step 2:** Left child exists (node(2)). Print `"L->2, "`.
- **Step 3:** Right child exists (node(3)). Print `"R->3\n"`. Now output: `1: L->2, R->3`
- **Step 4:** Recursive call on left child: `print_binary_tree(root.left)` where `root.left` is node(2).

Now inside node(2):
- Node(2) is not `None`. Print `"2: "`.
- Left child of node(2) is `None`. Print `"L->None,"`.
- Right child of node(2) is `None`. Print `"R->None\n"`. Output so far: `1: L->2, R->3\n2: L->None,R->None`
- Recursively call left child of node(2): `print_binary_tree(None)` → returns immediately.
- Recursively call right child of node(2): `print_binary_tree(None)` → returns.

Back to node(1) context. After left subtree recursion finishes, it proceeds to right child recursion: `print_binary_tree(root.right)` with node(3).

Inside node(3):
- Node(3) not `None`. Print `"3: "`.
- Left child `None` → `"L->None,"`.
- Right child `None` → `"R->None\n"`. Output: `1: L->2, R->3\n2: L->None,R->None\n3: L->None,R->None`
- Recursive calls on its children (both `None`) return.

**Final output:**
```
1: L->2, R->3
2: L->None,R->None
3: L->None,R->None
```

### 2.3 Recursion Tree Visualization

A **recursion tree** shows the order of function calls. For the above tree, the recursion tree is:

```
print_binary_tree(1)
├─ print_binary_tree(2)
│  ├─ print_binary_tree(None) [left of 2]
│  └─ print_binary_tree(None) [right of 2]
└─ print_binary_tree(3)
   ├─ print_binary_tree(None) [left of 3]
   └─ print_binary_tree(None) [right of 3]
```

Each call prints the node's info before recursing further. This is preorder traversal. The recursion depth equals the height of the tree (here height=2, root at level 1).

---

## 3. Predefined Binary Tree Inputs

The module `predefinedBT` (simulated below) provides a function `predefined_binary_tree_inputs()` that returns three binary trees of increasing complexity.

```python
def predefined_binary_tree_inputs():
    """
    Creates and returns three binary trees with different structures.
    Returns:
        root1, root2, root3 : BinaryTreeNode objects representing the roots.
    """

    # ---------- Tree 1 ----------
    # Height = 3, 6 nodes
    root1 = BinaryTreeNode(1)
    root1.left = BinaryTreeNode(2)
    root1.right = BinaryTreeNode(3)
    root1.left.left = BinaryTreeNode(4)
    root1.left.right = BinaryTreeNode(5)
    root1.right.right = BinaryTreeNode(6)
    # Structure:
    #       1
    #     /   \
    #    2     3
    #   / \     \
    #  4   5     6

    # ---------- Tree 2 ----------
    # Height = 4, 8 nodes
    root2 = BinaryTreeNode(10)
    root2.left = BinaryTreeNode(20)
    root2.right = BinaryTreeNode(30)
    root2.left.left = BinaryTreeNode(40)
    root2.left.right = BinaryTreeNode(50)
    root2.right.left = BinaryTreeNode(60)
    root2.right.right = BinaryTreeNode(70)
    root2.left.left.left = BinaryTreeNode(80)
    # Structure:
    #       10
    #     /    \
    #   20      30
    #  /  \    /  \
    # 40   50 60  70
    # /
    #80

    # ---------- Tree 3 ----------
    # Height = 5, 12 nodes
    root3 = BinaryTreeNode(100)
    root3.left = BinaryTreeNode(200)
    root3.right = BinaryTreeNode(300)
    root3.left.left = BinaryTreeNode(400)
    root3.left.right = BinaryTreeNode(500)
    root3.right.left = BinaryTreeNode(600)
    root3.right.right = BinaryTreeNode(700)
    root3.left.left.left = BinaryTreeNode(800)
    root3.left.left.right = BinaryTreeNode(900)
    root3.right.right.left = BinaryTreeNode(1000)
    root3.right.right.right = BinaryTreeNode(1100)
    # Structure:
    #        100
    #      /     \
    #    200     300
    #   /  \     /  \
    # 400  500  600  700
    # / \        /  \
    #800 900   1000 1100

    return root1, root2, root3
```

### 3.1 Tree Structures in ASCII

**Tree 1:**
```
       1
     /   \
    2     3
   / \     \
  4   5     6
```

**Tree 2:**
```
       10
     /    \
   20      30
  /  \    /  \
 40   50 60  70
 /
80
```

**Tree 3:**
```
        100
      /     \
    200     300
   /  \     /  \
 400  500  600  700
 / \        /  \
800 900   1000 1100
```

---

## 4. Level-Order Print Function (Breadth-First)

Level-order traversal visits nodes level by level, from left to right. It uses a queue (FIFO) to keep track of nodes to visit.

```python
from collections import deque

def print_level_wise(root):
    """
    Prints the binary tree level by level using a queue.
    For each node, it prints its data and the data of its left and right children
    (or None if absent), then enqueues its children for later processing.
    """
    if root is None:
        return

    queue = deque([root])   # Start with the root node in the queue

    while queue:
        current_node = queue.popleft()   # Remove the front node
        print(f"{current_node.data}: ", end="")

        # Process left child
        if current_node.left:
            print(f"L->{current_node.left.data}", end=", ")
            queue.append(current_node.left)   # Enqueue left child
        else:
            print("L->None", end=", ")

        # Process right child
        if current_node.right:
            print(f"R->{current_node.right.data}")
            queue.append(current_node.right)  # Enqueue right child
        else:
            print("R->None")
```

### 4.1 Algorithm Explanation

1. If the tree is empty (`root is None`), do nothing.
2. Initialize a queue and enqueue the root node.
3. While the queue is not empty:
   - Dequeue the front node (call it `current_node`).
   - Print the node's data.
   - Print the left child's data (or `None`) and enqueue the left child if it exists.
   - Print the right child's data (or `None`) and enqueue the right child if it exists.
4. The process continues until all nodes are processed.

This ensures nodes are processed in increasing level order, and within each level, left to right.

### 4.2 Step-by-Step Execution with Queue States

Let's apply `print_level_wise` to **Tree 1** (root1).

**Initial Queue:** `[1]`

**Iteration 1:**
- Dequeue `1` → print `"1: "`.
- Left child exists (`2`) → print `"L->2, "`, enqueue `2`.
- Right child exists (`3`) → print `"R->3"`, enqueue `3`.
Queue now: `[2, 3]`
Output line: `1: L->2, R->3`

**Iteration 2:**
- Dequeue `2` → print `"2: "`.
- Left child exists (`4`) → print `"L->4, "`, enqueue `4`.
- Right child exists (`5`) → print `"R->5"`, enqueue `5`.
Queue now: `[3, 4, 5]`
Output line: `2: L->4, R->5`

**Iteration 3:**
- Dequeue `3` → print `"3: "`.
- Left child is `None` → print `"L->None, "`.
- Right child exists (`6`) → print `"R->6"`, enqueue `6`.
Queue now: `[4, 5, 6]`
Output line: `3: L->None, R->6`

**Iteration 4:**
- Dequeue `4` → print `"4: "`.
- Left child `None` → `"L->None, "`.
- Right child `None` → `"R->None"`.
Queue now: `[5, 6]`
Output line: `4: L->None, R->None`

**Iteration 5:**
- Dequeue `5` → print `"5: "`.
- Left child `None` → `"L->None, "`.
- Right child `None` → `"R->None"`.
Queue now: `[6]`
Output line: `5: L->None, R->None`

**Iteration 6:**
- Dequeue `6` → print `"6: "`.
- Left child `None` → `"L->None, "`.
- Right child `None` → `"R->None"`.
Queue now: `[]`
Output line: `6: L->None, R->None`

Queue empty → stop.

**Final output:**
```
1: L->2, R->3
2: L->4, R->5
3: L->None, R->6
4: L->None, R->None
5: L->None, R->None
6: L->None, R->None
```

Notice that leaf nodes (4,5,6) show `L->None, R->None` as expected.

---

## 5. Complete Example Usage

Combining all pieces, we can test the printing functions on the predefined trees.

```python
# Import or define the classes and functions as above
if __name__ == "__main__":
    # Get the three trees
    root1, root2, root3 = predefined_binary_tree_inputs()

    print("Recursive print of Tree 1:")
    print_binary_tree(root1)
    print("\nLevel-order print of Tree 1:")
    print_level_wise(root1)

    print("\nRecursive print of Tree 2:")
    print_binary_tree(root2)
    print("\nLevel-order print of Tree 2:")
    print_level_wise(root2)

    print("\nRecursive print of Tree 3:")
    print_binary_tree(root3)
    print("\nLevel-order print of Tree 3:")
    print_level_wise(root3)
```

This will display both traversal styles for each tree, allowing comparison.

---

## 6. Complexity Analysis

### Recursive Print (`print_binary_tree`)
- **Time Complexity:** O(n) where n is the number of nodes. Each node is visited exactly once.
- **Space Complexity:** O(h) where h is the height of the tree. This is the maximum depth of the recursion stack. In the worst case (skewed tree), h = n, so O(n). In a balanced tree, h = log n, so O(log n).

### Level-Order Print (`print_level_wise`)
- **Time Complexity:** O(n) because each node is processed once.
- **Space Complexity:** O(w) where w is the maximum width of the tree (number of nodes at a level). In the worst case, w can be up to n/2 (roughly), so O(n). The queue holds at most one level's nodes.

---

## 7. Conclusion

Binary trees are hierarchical structures where each node has at most two children. Printing a binary tree can be done in multiple ways:
- **Depth-first (recursive)** traverses deep into subtrees first. The provided recursive function prints in preorder.
- **Breadth-first (level-order)** uses a queue to print level by level.

The node class is the foundation, and the `predefined_binary_tree_inputs` function offers ready-to-use trees for testing. Understanding these printing mechanisms is essential for debugging and visualizing tree structures in algorithms.

By following the step-by-step execution and recursion trees, one can see exactly how the code flows and how the output is generated. This knowledge extends to more complex tree operations like searching, insertion, and deletion.