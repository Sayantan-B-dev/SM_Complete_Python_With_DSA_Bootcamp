# Tree Terminologies

This document explains the fundamental terms used to describe the structure and properties of trees in computer science and mathematics. Each term is defined, illustrated with examples, and where helpful, shown in ASCII diagrams.

## Table of Contents
1. [Node](#node)
2. [Edge](#edge)
3. [Root](#root)
4. [Leaf](#leaf)
5. [Parent](#parent)
6. [Child](#child)
7. [Ancestors](#ancestors)
8. [Descendants](#descendants)
9. [Subtree](#subtree)
10. [Level](#level)
11. [Forest](#forest)
12. [Path](#path)
13. [Cousins](#cousins)
14. [Siblings](#siblings)
15. [Degree](#degree)
16. [Depth](#depth)
17. [Height](#height)
18. [Internal Node](#internal-node)
19. [Branching Factor](#branching-factor)

---

## Reference Tree
The following generic tree will be used to illustrate many of the terms below.

```
        A
      / | \
     B  C  D
    / \    / \
   E   F  G   H
      / \
     I   J
```

- Node `A` is the root.
- Nodes `B`, `C`, `D` are children of `A`.
- Nodes `E`, `F` are children of `B`.
- Nodes `G`, `H` are children of `D`.
- Node `F` has children `I` and `J`.

---

## 1. Node
A node is the fundamental unit of a tree. It contains data and may have links to other nodes (its children). In diagrams, nodes are typically represented by circles containing a label.

**Example:** In the reference tree, each letter (A, B, C, D, E, F, G, H, I, J) represents a node.

---

## 2. Edge
An edge is a connection between two nodes. It represents a parent-child relationship. A tree with N nodes always has exactly N-1 edges (if it is connected and acyclic).

**Example:** The line connecting `A` and `B` is an edge; similarly, the line between `F` and `I` is an edge.

---

## 3. Root
The root is the topmost node in a tree. It has no parent. Every non‑empty tree has exactly one root. All other nodes are reachable from the root by following edges.

**Example:** In the reference tree, node `A` is the root.

---

## 4. Leaf
A leaf (or external node) is a node that has no children. Leaves are the endpoints of the tree.

**Example:** In the reference tree, leaves are: `C`, `E`, `G`, `H`, `I`, `J`. (Node `C` has no children; `E` has none; etc.)

---

## 5. Parent
A parent is a node that has one or more child nodes. An edge connects a parent to its child. Every node except the root has exactly one parent.

**Example:** 
- Parent of `B`, `C`, `D` is `A`.
- Parent of `E` and `F` is `B`.
- Parent of `I` and `J` is `F`.

---

## 6. Child
A child is a node that has a parent. A node can have zero, one, or many children.

**Example:**
- Children of `A`: `B`, `C`, `D`.
- Children of `D`: `G`, `H`.
- Children of `F`: `I`, `J`.

---

## 7. Ancestors
The ancestors of a node are all the nodes on the path from the root to that node’s parent (including the root, excluding the node itself). In other words, they are the node’s parent, grandparent, great‑grandparent, etc.

**Example:** Ancestors of `I`:
- Parent: `F`
- Grandparent: `B`
- Great‑grandparent: `A`
Thus ancestors = {`F`, `B`, `A`}.

---

## 8. Descendants
The descendants of a node are all the nodes that are in its subtree (its children, grandchildren, etc.). The node itself is not considered its own descendant.

**Example:** Descendants of `B` are: `E`, `F`, `I`, `J`. (Children `E` and `F`, plus `F`’s children `I` and `J`.)

---

## 9. Subtree
A subtree is any node in the tree and all its descendants. It is itself a tree. The root of a subtree is that node. Every node defines a subtree (sometimes called the subtree rooted at that node).

**Example:** The subtree rooted at `B` consists of nodes `B`, `E`, `F`, `I`, `J` and the edges among them.

ASCII representation of subtree rooted at `B`:
```
      B
     / \
    E   F
       / \
      I   J
```

---

## 10. Level
The level of a node is the number of edges on the path from the root to that node. The root is at level 0 (sometimes level 1 in some textbooks; here we use level 0).

**Example:**
- Level 0: `A`
- Level 1: `B`, `C`, `D`
- Level 2: `E`, `F`, `G`, `H`
- Level 3: `I`, `J`

---

## 11. Forest
A forest is a disjoint set of trees. If you remove the root of a tree, the remaining connected components form a forest. Also, any collection of trees is a forest.

**Example:** Removing root `A` from the reference tree leaves three trees: one rooted at `B`, one at `C`, and one at `D`. Those three trees together form a forest.

ASCII forest after removing `A`:
```
    B         C         D
   / \                 / \
  E   F               G   H
     / \
    I   J
```

---

## 12. Path
A path is a sequence of nodes where each consecutive pair is connected by an edge. In a tree, there is exactly one unique path between any two nodes (because a tree is acyclic).

**Example:** The path from `I` to `H` is: `I` → `F` → `B` → `A` → `D` → `H`. Length (number of edges) = 5.

---

## 13. Cousins
Cousins are nodes that are at the same level but have different parents. They are not siblings.

**Example:** In the reference tree, nodes `E` (child of `B`) and `G` (child of `D`) are both at level 2, and their parents are different (`B` vs `D`). Therefore, `E` and `G` are cousins. Also `F` and `G` are cousins (level 2, parents `B` and `D`). `I` and `G` are not cousins because `I` is at level 3, `G` at level 2.

---

## 14. Siblings
Siblings are nodes that share the same parent.

**Example:**
- Siblings: `B`, `C`, `D` (parent `A`)
- Siblings: `E` and `F` (parent `B`)
- Siblings: `G` and `H` (parent `D`)
- Siblings: `I` and `J` (parent `F`)

---

## 15. Degree
### Degree of a Node
The degree of a node is the number of its children. (In some contexts, for trees, the degree counts children only; for graphs, it counts incident edges.)

**Example:**
- Degree of `A` = 3 (children `B`, `C`, `D`)
- Degree of `B` = 2 (children `E`, `F`)
- Degree of `C` = 0 (leaf)
- Degree of `F` = 2 (children `I`, `J`)
- Degree of `I` = 0

### Degree of a Tree
The degree of a tree is the maximum degree among all its nodes.

**Example:** In the reference tree, the maximum degree is 3 (node `A`), so the tree degree is 3.

---

## 16. Depth
The depth of a node is the number of edges from the root to that node. This is identical to the node’s level (if level starts at 0).

**Example:** Depth of `I` = 3.

---

## 17. Height
The height of a node is the number of edges on the longest downward path from that node to a leaf. The height of a tree is the height of its root.

**Example:**
- Height of leaf `C` = 0.
- Height of `F` = 1 (to `I` or `J`).
- Height of `B` = 2 (path `B`→`F`→`I`).
- Height of tree (root `A`) = 3 (path `A`→`B`→`F`→`I`).

---

## 18. Internal Node
An internal node (or non‑leaf) is any node that has at least one child. It is not a leaf.

**Example:** `A`, `B`, `D`, `F` are internal nodes. `C`, `E`, `G`, `H`, `I`, `J` are leaves.

---

## 19. Branching Factor
The branching factor of a node is the number of its children. Often the term “branching factor” refers to the average number of children per node, or the maximum (as in B‑trees). For a generic tree, it’s another term for degree.

**Example:** The branching factor of node `A` is 3.

---

## File System Tree Example (ASCII)
To see these terms in a familiar context, consider a directory structure:

```
            / (root)
         /   |    \
      home  usr  etc
      /  \         |
   alice bob     passwd
   /     |
 docs   pics
```

- Root node: `/`
- Children of `/`: `home`, `usr`, `etc`
- Siblings: `home`, `usr`, `etc`
- Parent of `alice` and `bob` is `home`
- Leaf nodes: `docs`, `pics`, `passwd` (if they contain no further subdirectories)
- Path from `/` to `pics`: `/` → `home` → `bob` → `pics`
- Ancestors of `pics`: `bob`, `home`, `/`
- Depth of `pics` = 3
- Subtree rooted at `home`: includes `alice`, `bob`, `docs`, `pics`

---
