**Trees (Data Structure)**

A tree is a primary, non-linear data structure used to represent hierarchical relationships. Unlike arrays, linked lists, stacks, and queues (which are linear), a tree organizes data in levels.

**Definition**
A tree consists of nodes connected by edges.
- **Node:** An entity that contains data and links to other nodes.
- **Edge:** The link connecting two nodes.
- **Root:** The topmost node of the tree (the only node with no parent).
- **Parent & Child:** A node directly above another is its parent; the node directly below is its child.
- **Leaf:** A node with no children.

**Tree vs. Other Structures**
- **Linear Structures (Arrays, Linked Lists):** Data is arranged in a sequence (one after another). Traversal is straightforward but can be inefficient for searching/inserting in the middle.
- **Derived Structures (Stacks, Queues):** Abstract data types built on top of linear structures, following specific rules (LIFO/FIFO).
- **Hierarchical Structure (Trees):** Data is arranged in levels (like a file system). Allows for faster search, insertion, and deletion in many cases (e.g., Binary Search Trees).

**Generic Tree**
A generic tree (or general tree) is a tree where a node can have any number of child nodes. There is no limit on the branching factor. Examples include the file system directory structure.