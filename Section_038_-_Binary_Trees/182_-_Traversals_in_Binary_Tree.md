## Binary Tree Traversals: Preorder, Inorder, Postorder

Traversals define the order in which nodes are visited. The three classic depth‑first traversals differ in where the parent (root) is processed relative to its children.

---

### 1. The Example Tree (root1)

We use the tree provided by `predefined_binary_tree_inputs()`:

```
        1
       / \
      2   3
     / \   \
    4   5   6
```

**Properties:**
- Height = 3 (edges: 1‑2‑4)
- Not perfectly balanced (node 3 has only a right child)

---

### 2. Preorder Traversal (Root → Left → Right)

**Algorithm:**
1. Visit the current node (print its data).
2. Recursively traverse the left subtree.
3. Recursively traverse the right subtree.

**Code:**
```python
def preorder_traversal(root):
    if root is None:
        return
    print(root.data, end=" ")
    preorder_traversal(root.left)
    preorder_traversal(root.right)
```

**Step‑by‑step (root1):**
- Node 1: print `1`
  - Left subtree (node 2):  
    - Node 2: print `2`  
      - Left subtree (node 4): print `4`  
        - left of 4: None → return  
        - right of 4: None → return  
      - Right subtree (node 5): print `5`  
        - left/right None → return  
  - Right subtree (node 3):  
    - Node 3: print `3`  
      - left of 3: None → return  
      - right subtree (node 6): print `6`  
        - children None → return

**Output:**  
`1 2 4 5 3 6`

**Visual trace:**
```
Preorder: 1 → 2 → 4 → 5 → 3 → 6
```

---

### 3. Inorder Traversal (Left → Root → Right)

**Algorithm:**
1. Recursively traverse the left subtree.
2. Visit the current node.
3. Recursively traverse the right subtree.

**Code:**
```python
def inorder_traversal(root):
    if root is None:
        return
    inorder_traversal(root.left)
    print(root.data, end=" ")
    inorder_traversal(root.right)
```

**Step‑by‑step (root1):**
- Left subtree of 1 (node 2):  
  - Left subtree of 2 (node 4):  
    - left of 4: None → print `4` → right of 4: None  
  - Print `2`  
  - Right subtree of 2 (node 5):  
    - left of 5: None → print `5` → right of 5: None  
- Print `1`  
- Right subtree of 1 (node 3):  
  - left of 3: None  
  - Print `3`  
  - Right subtree of 3 (node 6):  
    - left of 6: None → print `6` → right of 6: None

**Output:**  
`4 2 5 1 3 6`

**Visual trace:**
```
Inorder: 4 → 2 → 5 → 1 → 3 → 6
```

---

### 4. Postorder Traversal (Left → Right → Root)

**Algorithm:**
1. Recursively traverse the left subtree.
2. Recursively traverse the right subtree.
3. Visit the current node.

**Code:**
```python
def postorder_traversal(root):
    if root is None:
        return
    postorder_traversal(root.left)
    postorder_traversal(root.right)
    print(root.data, end=" ")
```

**Step‑by‑step (root1):**
- Left subtree of 1 (node 2):  
  - Left subtree of 2 (node 4):  
    - left/right None → print `4`  
  - Right subtree of 2 (node 5):  
    - left/right None → print `5`  
  - Print `2`  
- Right subtree of 1 (node 3):  
  - left of 3: None  
  - Right subtree of 3 (node 6):  
    - left/right None → print `6`  
  - Print `3`  
- Print `1`

**Output:**  
`4 5 2 6 3 1`

**Visual trace:**
```
Postorder: 4 → 5 → 2 → 6 → 3 → 1
```

---

### 5. Comparison Table

| Traversal | Order | Output for root1 |
|-----------|-------|------------------|
| **Preorder** | Root → Left → Right | `1 2 4 5 3 6` |
| **Inorder** | Left → Root → Right | `4 2 5 1 3 6` |
| **Postorder** | Left → Right → Root | `4 5 2 6 3 1` |

---

### 6. Recursion Call Tree (for Preorder)

```
preorder(1)
├─ print 1
├─ preorder(2)
│  ├─ print 2
│  ├─ preorder(4)
│  │  ├─ print 4
│  │  ├─ preorder(None)
│  │  └─ preorder(None)
│  └─ preorder(5)
│     ├─ print 5
│     ├─ preorder(None)
│     └─ preorder(None)
└─ preorder(3)
   ├─ print 3
   ├─ preorder(None)
   └─ preorder(6)
      ├─ print 6
      ├─ preorder(None)
      └─ preorder(None)
```

Each recursive call follows the same pattern: print the node, then go left, then right.

---

### 7. Key Observations

- **Preorder** is useful for copying a tree or producing a prefix expression (in expression trees).
- **Inorder** on a binary search tree (BST) yields nodes in sorted order.
- **Postorder** is used for deleting a tree (children before parent) and for postfix expression evaluation.

All traversals are **O(n)** time and **O(h)** space (recursion stack).