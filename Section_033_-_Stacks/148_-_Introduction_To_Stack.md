### Why Study Multiple Data Structures?

No single data structure solves all problems efficiently. The choice dictates an application's performance, memory usage, and complexity. Understanding multiple structures allows you to optimize for specific operations.

- **No Universal Solution:** Arrays offer fast index access but slow inserts/deletes. Linked Lists offer fast inserts/deletes but slow search. Hash Tables offer fast lookups but no inherent order.
- **Algorithm Efficiency:** The data structure directly impacts an algorithm's time complexity (e.g., searching in an unsorted array is O(n), while searching in a balanced Binary Search Tree is O(log n)).
- **Problem Representation:** Certain problems map naturally to specific structures. A call stack maps to a Stack; a printer queue maps to a Queue; a network of friends maps to a Graph.
- **Resource Constraints:** Structures have different memory footprints. Arrays have minimal overhead; trees and graphs require extra memory for pointers/references.

**Trade-off Summary:**

| Structure | Access | Search | Insert | Delete | Memory |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Array** | O(1) | O(n) | O(n) | O(n) | Low |
| **Linked List** | O(n) | O(n) | O(1) | O(1) | Medium |
| **Hash Table** | N/A | O(1)* | O(1)* | O(1)* | High |
| **BST** | O(log n) | O(log n) | O(log n) | O(log n) | Medium |
| **Stack/Queue** | O(n) | O(n) | O(1)** | O(1)** | Low/Med |
| **Graph** | O(1) | O(V+E) | O(1) | O(V+E) | High |

*\*Average case. Worst-case can be O(n).*  
*\**Insert/Delete only at specific ends.*

---

### What Data Structure to Choose and When

The choice depends on the dominant operations and data characteristics.

- **Need to store and access elements sequentially by index?**
    - **Use:** Array or List.
    - **Why:** Provides O(1) random access if the index is known. Good for static data or data accessed in loops.

- **Need frequent insertions or deletions from the beginning or middle?**
    - **Use:** Linked List.
    - **Why:** Avoids shifting elements like an array. Insertion/deletion is O(1) once you are at the correct node.

- **Need to manage data in a Last-In, First-Out (LIFO) manner?** (e.g., function calls, undo)
    - **Use:** Stack.
    - **Why:** Optimized for push/pop operations at one end.

- **Need to manage data in a First-In, First-Out (FIFO) manner?** (e.g., processing tasks in order)
    - **Use:** Queue.
    - **Why:** Optimized for enqueue/dequeue operations.

- **Need fast lookups, insertions, and deletions based on a key, and order doesn't matter?**
    - **Use:** Hash Table (Dictionary / HashMap / Object).
    - **Why:** Provides near O(1) average time complexity for these operations. Ideal for caching, indexing, and databases.

- **Need to maintain sorted data and support range queries or ordered traversal?** (e.g., "find all items between X and Y")
    - **Use:** Balanced Binary Search Tree (e.g., Red-Black Tree) or B-Tree.
    - **Why:** Keeps data sorted automatically. Search, min, max, predecessor, successor are O(log n). B-Trees are optimized for disk storage (databases, file systems).

- **Need to model relationships between entities?** (e.g., social networks, maps, web links)
    - **Use:** Graph.
    - **Why:** Fundamental structure for representing connections. Use Adjacency List for sparse graphs, Adjacency Matrix for dense graphs.

- **Need to find the shortest path or cheapest connection?**
    - **Use:** Graph algorithms (Dijkstra, A*, BFS) with appropriate data structures (Priority Queue for Dijkstra).

- **Need frequent access to the minimum or maximum element?**
    - **Use:** Priority Queue (often implemented with a Heap).
    - **Why:** Provides O(1) access to the min/max and O(log n) insertion/deletion.

---

### Introduction to the Stack Data Structure

A stack is a linear data structure that follows the **Last-In, First-Out (LIFO)** principle. The most recently added element is the first one to be removed. It can be visualized as a stack of plates: you add a plate to the top, and you take a plate from the top.

#### Core Operations
- **Push:** Adds an element to the top of the stack.
- **Pop:** Removes and returns the top element from the stack.
- **Peek (or Top):** Returns the top element without removing it.
- **isEmpty:** Checks if the stack is empty.
- **Size:** Returns the number of elements in the stack.

**Time Complexity:** Push, Pop, Peek, and isEmpty are O(1) operations.

#### Properties
- **LIFO Order:** The defining characteristic.
- **Restricted Access:** Elements can only be added or removed from one end (the top). You cannot access elements in the middle directly without popping elements above them.
- **Dynamic or Static Size:** Can be implemented using arrays (fixed size if static) or linked lists (dynamic size).

#### Real-World and Technical Use Cases

1.  **Function Call Management (Call Stack):** In programming languages, a stack manages active function calls. When a function is called, its return address and local variables are pushed onto the call stack. When the function returns, its frame is popped, and control returns to the previous function.
2.  **Undo/Redo Operations:** In editors (text, graphic), every action (typing, deleting) is pushed onto an "undo stack". Pressing Ctrl+Z pops the last action to revert it. A separate "redo stack" can store popped actions.
3.  **Expression Evaluation and Syntax Parsing:**
    - **Converting infix expressions (e.g., `A + B`) to postfix (e.g., `A B +`):** The Shunting Yard algorithm uses a stack for operators.
    - **Evaluating postfix expressions:** Operands are pushed onto a stack; when an operator is read, operands are popped, the operation is performed, and the result is pushed back.
    - **Syntax Checking:** Used to check for balanced parentheses, brackets, and braces in code. An opening symbol is pushed; a closing symbol triggers a pop and checks for a match.
4.  **Backtracking Algorithms:**
    - **Depth-First Search (DFS) in graphs/trees:** Can be implemented iteratively using a stack to remember the path to backtrack.
    - **Maze Solving:** The path taken is pushed onto a stack. When a dead end is reached, you pop back to the last junction.
5.  **Browser History:** The "Back" button functionality. The URLs you visit are pushed onto a stack. Clicking "Back" pops the current URL to reveal the previous one.

**Simple Implementation Concept (Pseudocode):**

```
class Stack:
    list = []

    function push(item):
        list.append(item)

    function pop():
        if not isEmpty():
            return list.pop() // Removes and returns last element
        else:
            return error

    function peek():
        if not isEmpty():
            return list[last_index]
        else:
            return error

    function isEmpty():
        return list is empty
```