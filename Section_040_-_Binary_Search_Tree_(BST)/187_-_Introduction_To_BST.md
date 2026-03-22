![https://yongdanielliang.github.io/animation/web/BST.html](https://yongdanielliang.github.io/animation/web/BST.html)

![https://www.cs.usfca.edu/~galles/visualization/BST.html](https://www.cs.usfca.edu/~galles/visualization/BST.html)


# Binary Search Tree (BST)

## 1. Definition and Concept Overview

A Binary Search Tree (BST) is a binary tree data structure that maintains a strict ordering invariant:

- For any node, all nodes in its left subtree store values strictly less than the node’s value.
- All nodes in its right subtree store values strictly greater than the node’s value.
- The left and right subtrees themselves are binary search trees.

This property enables binary‑search–style lookups: starting from the root, each comparison eliminates approximately half of the remaining nodes. The structure is directly inspired by the binary search algorithm applied to a dynamic, linked representation.

## 2. Core Principles and Internal Mechanics

**Comparison‑based navigation**  
Each node stores a key (value) and pointers to left and right children. Search, insertion, and deletion follow a deterministic path based on comparisons with the current node’s key:

- If the target is smaller, move to the left child.
- If larger, move to the right child.
- Equality terminates the operation (for search) or determines the handling policy (for insertion/deletion).

**Recursive structure**  
The BST property is defined recursively. This allows algorithms to be expressed elegantly with recursion, though iterative implementations are equally valid and avoid stack overflow in deep trees.

**Ordered traversal**  
An inorder traversal of a BST visits nodes in non‑decreasing order. This property is fundamental to operations like range queries and sorting.

**Balance sensitivity**  
The efficiency of BST operations depends on the tree’s height. In a perfectly balanced tree with *n* nodes, height ≈ log₂ *n*. If nodes are inserted in sorted order, the tree degenerates to a linked list with height *n*, negating the logarithmic advantage.

## 3. Step‑by‑Step Logical Breakdown

### Search
1. Start at the root.
2. If the current node is `None`, the key does not exist.
3. If the target equals the current node’s key, return the node.
4. If the target is less, repeat from the left child; otherwise, repeat from the right child.

### Insertion
1. If the tree is empty, create a new node as the root.
2. Compare the new key with the current node:
   - If equal, decide a policy: reject duplicates, increment a counter, or store in a side structure. Standard BSTs typically disallow duplicates.
   - If smaller and left child exists, recurse left; if left child is absent, insert there.
   - If larger and right child exists, recurse right; if right child is absent, insert there.

### Deletion
Handling node removal requires three cases:

- **Leaf node**: Remove it directly (set the parent’s corresponding pointer to `None`).
- **Node with one child**: Replace the node with its child.
- **Node with two children**: Find the inorder successor (smallest node in the right subtree) or inorder predecessor (largest node in the left subtree). Copy its value into the current node, then recursively delete the successor/predecessor (which will have at most one child).

## 4. Implementation (Python)

```python
class BSTNode:
    """Node in a Binary Search Tree."""
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None


class BinarySearchTree:
    def __init__(self):
        self.root = None

    def search(self, key):
        """Return the node with the given key, or None if not found."""
        current = self.root
        while current:
            if key == current.key:
                return current
            elif key < current.key:
                current = current.left
            else:
                current = current.right
        return None

    def insert(self, key):
        """Insert a key into the BST. Duplicate keys are ignored."""
        if self.root is None:
            self.root = BSTNode(key)
            return

        current = self.root
        while True:
            if key == current.key:
                # Duplicate: silently ignored (policy choice)
                return
            elif key < current.key:
                if current.left is None:
                    current.left = BSTNode(key)
                    return
                current = current.left
            else:  # key > current.key
                if current.right is None:
                    current.right = BSTNode(key)
                    return
                current = current.right

    def delete(self, key):
        """Remove the node with the given key from the BST."""
        def _delete(node, key):
            if node is None:
                return None

            if key < node.key:
                node.left = _delete(node.left, key)
            elif key > node.key:
                node.right = _delete(node.right, key)
            else:
                # Node to delete found
                # Case 1: leaf or single child
                if node.left is None:
                    return node.right
                if node.right is None:
                    return node.left

                # Case 2: two children - find inorder successor
                successor = node.right
                while successor.left:
                    successor = successor.left
                node.key = successor.key
                # Delete the successor from the right subtree
                node.right = _delete(node.right, successor.key)

            return node

        self.root = _delete(self.root, key)

    def inorder(self):
        """Return list of keys in sorted order (inorder traversal)."""
        result = []
        def _inorder(node):
            if node is None:
                return
            _inorder(node.left)
            result.append(node.key)
            _inorder(node.right)
        _inorder(self.root)
        return result
```

## 5. Time and Space Complexity Analysis

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Search    | O(log n)     | O(n)       |
| Insert    | O(log n)     | O(n)       |
| Delete    | O(log n)     | O(n)       |
| Space     | O(n)         | O(n)       |

- **Average case** assumes a randomly built BST where the height is O(log n). Balanced variants guarantee this bound.
- **Worst case** occurs when keys are inserted in sorted order, producing a degenerate tree (height = n).
- **Space complexity** accounts for storing *n* nodes. Recursive implementations consume O(h) additional stack space, where h is the tree height.

## 6. Edge Cases and Failure Scenarios

- **Duplicate keys**  
  Without a defined policy, duplicates violate the strict ordering. Solutions include: rejecting insertion, storing a count per node, or using a secondary collection (e.g., list) for equal keys.

- **Empty tree**  
  Operations on an empty tree: search returns `None`; insertion creates a root; deletion does nothing.

- **Deletion of root**  
  Handled implicitly by the recursive `_delete` function, which returns the new root after deletion.

- **Key not present**  
  Search returns `None`; deletion leaves the tree unchanged.

- **Degenerate tree**  
  All operations degrade to O(n). This is the primary performance pitfall and motivates self‑balancing structures (AVL, Red‑Black).

- **Large recursion depth**  
  Recursive implementations may hit Python’s recursion limit on highly unbalanced trees. Iterative versions avoid this.

## 7. Practical Use Cases

- **Phonebook / Directory**  
  Keys (names) support logarithmic lookup, insertion, and deletion. Inorder traversal provides alphabetical listing.

- **File system indexing**  
  Metadata lookup by path or inode number often uses tree structures with BST properties.

- **Auto‑complete / prefix search**  
  While a trie is more common, a BST storing strings can support prefix searches with modifications (e.g., storing all suffixes in subtrees).

- **Database indexing**  
  Many database indexes use B‑trees, a generalization of BSTs optimized for disk storage and balanced growth.

- **Machine Learning**  
  Decision trees (e.g., random forests) partition feature space based on threshold comparisons, structurally similar to BSTs.

- **Range queries**  
  With inorder traversal, BSTs efficiently retrieve keys within a given interval: traverse selectively, ignoring branches that cannot contain values in the range.

## 8. Limitations and Trade‑offs

- **No built‑in balance**  
  Standard BSTs do not maintain height balance, leading to worst‑case linear time. For guaranteed O(log n) performance, use AVL or Red‑Black trees, which enforce balance with rotation operations.

- **Memory overhead**  
  Each node stores two pointers (left, right), increasing memory consumption compared to array‑based structures.

- **Cache locality**  
  Nodes are allocated dynamically and scattered in memory, causing poor cache performance relative to contiguous structures (e.g., sorted arrays).

- **Comparison overhead**  
  Each operation requires key comparisons; for large or complex keys (e.g., strings), this cost becomes significant.

- **Not suitable for exact‑match only**  
  For pure existence checks without ordering needs, hash tables offer O(1) average‑case operations, though they lose ordered traversal capabilities.

- **Concurrency**  
  Standard BST implementations are not thread‑safe. Concurrent modifications require external synchronization or lock‑free variants.

- **Balanced BST introduction**  
  Self‑balancing variants (AVL, Red‑Black, Splay) address the worst‑case height issue but introduce additional complexity and constant‑factor overhead in rotations. They are the foundation for many production systems (e.g., `std::map` in C++).