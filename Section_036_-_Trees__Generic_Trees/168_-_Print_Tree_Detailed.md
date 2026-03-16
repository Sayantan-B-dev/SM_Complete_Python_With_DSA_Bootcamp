```py
class TreeNode:
    def __init__(self,data):
        self.data = data
        self.children = []


root = TreeNode(1)

child1 = TreeNode(2)
child2 = TreeNode(3)
child3 = TreeNode(4)

root.children.append(child1)
root.children.append(child2)
root.children.append(child3)

def print_tree_detailed(root):
    if(root==None): # Edge Case
        return
    
    print(f"{root.data}:",end = "")

    for eachChild in root.children:
        print(eachChild.data,end = ",")

    print()

    for eachChild in root.children:
        print_tree_detailed(eachChild)

print_tree_detailed(root)




root = TreeNode(1)

child1 = TreeNode(2)
child2 = TreeNode(3)
child3 = TreeNode(4)

root.children.append(child1)
child1.children.append(child2)
root.children.append(child3)

print_tree_detailed(root)
```
## Tree Traversals for Generic Trees

A traversal is a systematic way to visit every node in a tree exactly once. Different traversals produce different orders and are used for various purposes (e.g., copying, expression evaluation, searching). For a **generic tree** (where nodes can have any number of children), the most common traversals are:

- **Preorder**: Visit the node, then recursively visit each child.
- **Postorder**: Recursively visit each child, then visit the node.
- **Level‑order (Breadth‑first)**: Visit nodes level by level from top to bottom.

(Note: Inorder traversal is typically defined for binary trees and does not have a standard generalization for n‑ary trees.)

### The Detailed Print Function (Parent‑Child View)

The function provided prints each node followed by its immediate children, giving a snapshot of the tree’s structure:

```python
def print_tree_detailed(root):
    if root is None:
        return
    print(f"{root.data}:", end="")
    for child in root.children:
        print(child.data, end=",")
    print()
    for child in root.children:
        print_tree_detailed(child)
```

**Output for the first tree** (root 1 with children 2,3,4):

```
1:2,3,4,
2:
3:
4:
```

**Output for the second tree** (1 → [2,4]; 2 → [3]):

```
1:2,4,
2:3,
3:
4:
```

This is not a traversal in the traditional sense (it does not produce a single sequence of nodes), but it is useful for debugging.

---

## 1. Preorder Traversal

**Algorithm** (recursive):
```
preorder(node):
    if node is None: return
    visit(node)
    for each child in node.children:
        preorder(child)
```

**Order of visitation**: Root → all nodes in the first subtree → all nodes in the second subtree, etc.

**ASCII Tree Example** (same tree used in code):
```
        1
      /   \
     2     4
     |
     3
```

**Preorder output**: `1, 2, 3, 4`

**Visualization with visitation numbers**:
```
     (1)1
      /   \
   (2)2    (4)4
     |
   (3)3
```

**Python Implementation**:

```python
def preorder(root):
    if root is None:
        return
    print(root.data, end=' ')          # Visit node
    for child in root.children:
        preorder(child)
```

**Usage**:
```python
preorder(root)   # Output: 1 2 3 4
```

**Iterative version** (using a stack):

```python
def preorder_iterative(root):
    if root is None:
        return
    stack = [root]
    while stack:
        node = stack.pop()
        print(node.data, end=' ')
        # Push children in reverse order to maintain left‑to‑right order
        for child in reversed(node.children):
            stack.append(child)
```

---

## 2. Postorder Traversal

**Algorithm** (recursive):
```
postorder(node):
    if node is None: return
    for each child in node.children:
        postorder(child)
    visit(node)
```

**Order of visitation**: All nodes in the first subtree → second subtree → ... → root.

**Postorder output for the example tree**: `3, 2, 4, 1`

**Visualization**:
```
     (4)1
      /   \
   (2)2    (3)4
     |
   (1)3
```

**Python Implementation**:

```python
def postorder(root):
    if root is None:
        return
    for child in root.children:
        postorder(child)
    print(root.data, end=' ')
```

**Usage**:
```python
postorder(root)   # Output: 3 2 4 1
```

**Iterative version** (more complex, often uses two stacks or a visited flag):

```python
def postorder_iterative(root):
    if root is None:
        return
    stack = [(root, False)]
    while stack:
        node, visited = stack.pop()
        if visited:
            print(node.data, end=' ')
        else:
            stack.append((node, True))
            # Push children in reverse order to process leftmost first
            for child in reversed(node.children):
                stack.append((child, False))
```

---

## 3. Level‑order (Breadth‑first) Traversal

**Algorithm** (using a queue):
```
level_order(root):
    if root is None: return
    queue = empty queue
    queue.enqueue(root)
    while queue is not empty:
        node = queue.dequeue()
        visit(node)
        for each child in node.children:
            queue.enqueue(child)
```

**Order of visitation**: Nodes are visited level by level, from left to right.

**Level‑order output for the example tree**: `1, 2, 4, 3`

**Visualization** (levels separated by `|`):
```
Level 0: 1
Level 1: 2, 4
Level 2: 3
```

**Python Implementation**:

```python
from collections import deque

def level_order(root):
    if root is None:
        return
    queue = deque([root])
    while queue:
        node = queue.popleft()
        print(node.data, end=' ')
        for child in node.children:
            queue.append(child)
```

To print levels separately (with line breaks):

```python
def level_order_lines(root):
    if root is None:
        return
    queue = deque([root])
    while queue:
        level_size = len(queue)
        for _ in range(level_size):
            node = queue.popleft()
            print(node.data, end=' ')
            for child in node.children:
                queue.append(child)
        print()   # newline after each level
```

**Output**:
```
1 
2 4 
3 
```

---

## Comparison of Traversal Orders

| Traversal   | Order of Visitation (for example tree) |
|-------------|----------------------------------------|
| Preorder    | 1, 2, 3, 4                             |
| Postorder   | 3, 2, 4, 1                             |
| Level‑order | 1, 2, 4, 3                             |

**Use cases**:
- **Preorder**: Used to copy a tree, or to get a prefix expression (in expression trees).
- **Postorder**: Used to delete a tree (delete children before parent), or to get postfix expression.
- **Level‑order**: Used to find the shortest path, or to serialize a tree for breadth‑first search.

---

## Handling Large Trees – Recursion Limits

All recursive traversals are limited by the maximum depth of the tree. If the tree is very deep (e.g., a degenerate tree where each node has one child), recursion may hit Python’s recursion limit (default ~1000). For production code, iterative versions (using explicit stacks or queues) are safer.

---

## Visualizing Traversals with a Larger Generic Tree

Consider a slightly larger tree:

```
         A
       / | \
      B  C  D
     / \    |
    E   F   G
        |
        H
```

**Preorder**: A, B, E, F, H, C, D, G  
**Postorder**: E, H, F, B, C, G, D, A  
**Level‑order**: A, B, C, D, E, F, G, H

These sequences illustrate how each traversal orders the nodes differently.
