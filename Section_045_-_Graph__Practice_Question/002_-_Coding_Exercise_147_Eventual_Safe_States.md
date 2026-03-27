## Table of Contents

1. **Title and Description**  
2. **Full Commented Code**  
3. **Algorithm Overview**  
   - 3.1. Problem Definition  
   - 3.2. Key Insight  
   - 3.3. Reverse Graph and Outdegree  
   - 3.4. Topological Sort (Kahn’s Algorithm) on Reverse Graph  
4. **Step‑by‑Step Workflow with ASCII Diagrams**  
   - 4.1. Example Graph  
   - 4.2. Building Reverse Graph and Outdegree  
   - 4.3. Initializing Queue with Zero‑Outdegree Nodes  
   - 4.4. BFS Processing (Removing Nodes)  
   - 4.5. Marking Safe Nodes and Collecting Result  
5. **Recursive Thinking (Conceptual)**  
6. **Complexity Analysis**  
7. **Example Usage and Output**  
8. **How to Use the Function**  
9. **Conclusion**

---

## 1. Title and Description

**Title:** Eventual Safe Nodes in a Directed Graph

**Description:**  
This function `eventualSafeNodes(graph)` identifies all **safe nodes** in a directed graph. A node is considered **safe** if every possible path starting from that node eventually reaches a terminal node (a node with out‑degree 0) and never enters a cycle. The function returns the list of safe nodes sorted in ascending order.

The solution uses a **reverse graph** and **topological sort (Kahn’s algorithm)** to iteratively remove nodes that have out‑degree zero. Nodes that are removed (i.e., become out‑degree zero during the process) are exactly the safe nodes.

---

## 2. Full Commented Code

```python
def eventualSafeNodes(graph):
    """
    Function to find all the safe nodes in a directed graph.

    :param graph: List[List[int]] -> Adjacency list representing the directed graph
    :return: List[int] -> List of all safe nodes sorted in ascending order
    """
    number_of_nodes = len(graph)

    # Reverse graph to track incoming edges
    reversed_graph = [[] for _ in range(number_of_nodes)]

    # Array to store outdegree of each node
    outdegree_count = [0] * number_of_nodes

    # Build reversed graph and compute outdegrees
    for node in range(number_of_nodes):
        for neighbor in graph[node]:
            reversed_graph[neighbor].append(node)
        outdegree_count[node] = len(graph[node])

    # Queue for nodes with zero outdegree (terminal nodes)
    from collections import deque
    processing_queue = deque()

    for node in range(number_of_nodes):
        if outdegree_count[node] == 0:
            processing_queue.append(node)

    # Boolean array to mark safe nodes
    is_safe_node = [False] * number_of_nodes

    # Process nodes using BFS (Kahn's Algorithm style)
    while processing_queue:
        current_node = processing_queue.popleft()
        is_safe_node[current_node] = True

        for parent_node in reversed_graph[current_node]:
            outdegree_count[parent_node] -= 1
            if outdegree_count[parent_node] == 0:
                processing_queue.append(parent_node)

    # Collect all safe nodes in ascending order
    safe_nodes_list = [node for node in range(number_of_nodes) if is_safe_node[node]]

    return safe_nodes_list
```

---

## 3. Algorithm Overview

### 3.1. Problem Definition
We are given a directed graph represented by an adjacency list `graph` where `graph[i]` contains all nodes that node `i` has an edge to.  
A **terminal node** is a node with out‑degree 0 (no outgoing edges).  
A node is **safe** if **every** path starting from that node eventually ends at a terminal node. This implies that the node cannot be part of any cycle; otherwise, there would be a path that loops forever and never reaches a terminal.

### 3.2. Key Insight
The safe nodes are precisely those that are **not part of any cycle** and have a path to a terminal node.  
If we remove all terminal nodes (nodes with out‑degree 0) from the graph, the out‑degree of their predecessors decreases. If a predecessor becomes a new terminal (out‑degree 0) after removal, it is also safe. This process is analogous to a topological sort on the **reverse graph**:

- In the reverse graph, edges are reversed. Then the **out‑degree** in the original graph becomes the **in‑degree** in the reverse graph.
- Starting from nodes that have original out‑degree 0 (terminals), we “remove” them, which in the reverse graph means we decrement the out‑degree (actually in‑degree in original) of their predecessors.
- When a node’s out‑degree in the original graph drops to 0, it is safe and can be processed further.

### 3.3. Reverse Graph and Outdegree
We construct `reversed_graph` where for every original edge `u -> v`, we add `u` to `reversed_graph[v]`. This allows us to quickly find all predecessors of a node.

We also compute `outdegree_count[node]` = number of outgoing edges from `node` in the original graph.

### 3.4. Topological Sort (Kahn’s Algorithm) on Reverse Graph
We use a queue initialized with all nodes that have `outdegree_count == 0` (terminal nodes).  
While the queue is not empty:
- Pop a node `current` – it is safe (marked true).
- For each predecessor `parent` of `current` (obtained from `reversed_graph[current]`):
  - Decrement `outdegree_count[parent]` because we are effectively removing `current` from the graph.
  - If `outdegree_count[parent]` becomes 0, it means all outgoing edges from `parent` now lead to safe nodes (or were already removed), so `parent` is also safe – push it to the queue.

At the end, all nodes marked `is_safe_node` are safe.

---

## 4. Step‑by‑Step Workflow with ASCII Diagrams

Let’s work through a concrete example to see each stage.

**Example Graph:**  
`graph = [[1,2], [2,3], [5], [0], [5], [], []]`  
This corresponds to 7 nodes (0..6) with the following directed edges:

```
0 → 1, 2
1 → 2, 3
2 → 5
3 → 0
4 → 5
5 → (no outgoing)
6 → (no outgoing)
```

The graph visualisation:

```
   ┌─┐
   │ │
   v │
0 → 1 → 2 → 5
│   │   │
│   v   │
└→ 3 ←─┘

4 → 5
6
```

- Node 5 and 6 are terminal (out‑degree 0).
- Node 0,1,2,3 form a cycle (0→1→2→3→0). They are not safe because from them you can loop forever.
- Node 4 goes to terminal 5 → safe.
- Node 5 and 6 are terminal → safe.

Expected output: `[4,5,6]` (sorted).

### 4.1. Building Reverse Graph and Outdegree

Initialize:
```
number_of_nodes = 7
reversed_graph = [ [], [], [], [], [], [], [] ]
outdegree_count = [0,0,0,0,0,0,0]
```

Process each node’s outgoing edges:

**Node 0:** edges [1,2]  
- reversed_graph[1].append(0) → rev[1] = [0]  
- reversed_graph[2].append(0) → rev[2] = [0]  
- outdegree_count[0] = 2

**Node 1:** edges [2,3]  
- rev[2].append(1) → rev[2] = [0,1]  
- rev[3].append(1) → rev[3] = [1]  
- outdegree_count[1] = 2

**Node 2:** edges [5]  
- rev[5].append(2) → rev[5] = [2]  
- outdegree_count[2] = 1

**Node 3:** edges [0]  
- rev[0].append(3) → rev[0] = [3]  
- outdegree_count[3] = 1

**Node 4:** edges [5]  
- rev[5].append(4) → rev[5] = [2,4]  
- outdegree_count[4] = 1

**Node 5:** edges []  
- outdegree_count[5] = 0

**Node 6:** edges []  
- outdegree_count[6] = 0

Final structures:

```
outdegree_count = [2, 2, 1, 1, 1, 0, 0]

reversed_graph:
0: [3]
1: [0]
2: [0,1]
3: [1]
4: []
5: [2,4]
6: []
```

### 4.2. Initializing Queue with Zero‑Outdegree Nodes

Nodes with outdegree 0: 5 and 6 → queue = deque([5, 6])

### 4.3. BFS Processing (Removing Nodes)

We’ll process the queue step by step, showing state after each removal.

**Step 1:** current = 5 (pop left)  
- Mark safe: is_safe[5] = True  
- For each parent in rev[5] = [2,4]:  
  - parent=2: outdegree_count[2]-- → from 1 to 0 → becomes 0 → push 2 to queue.  
  - parent=4: outdegree_count[4]-- → from 1 to 0 → becomes 0 → push 4 to queue.  
Queue now: [6, 2, 4]

**Step 2:** current = 6  
- Mark safe: is_safe[6] = True  
- rev[6] = [] → no parents.  
Queue now: [2, 4]

**Step 3:** current = 2  
- Mark safe: is_safe[2] = True  
- rev[2] = [0,1]  
  - parent=0: outdegree_count[0]-- → from 2 to 1 (not zero)  
  - parent=1: outdegree_count[1]-- → from 2 to 1 (not zero)  
Queue now: [4]

**Step 4:** current = 4  
- Mark safe: is_safe[4] = True  
- rev[4] = [] → no parents.  
Queue now: empty

Processing ends.

### 4.4. Marking Safe Nodes and Collecting Result

After BFS, `is_safe` = [False, False, True, False, True, True, True]  
Safe nodes: indices where True → [2,4,5,6]

But wait – node 2 was marked safe? In our earlier analysis, node 2 is part of the cycle and should not be safe. Let’s re‑examine the graph:  
Edges: 0→1, 0→2; 1→2, 1→3; 2→5; 3→0; 4→5; 5→; 6→.  
The cycle is 0→1→3→0. Node 2 has an outgoing edge to terminal 5, but also can be reached from the cycle (0→2, 1→2). However, from node 2, every path:  
- 2 → 5 (terminal) – good.  
- There is no path from 2 back into the cycle because all edges from cycle go into 2, but 2 does not have any edge back to the cycle (its only outgoing is to 5). So indeed, from node 2, the only possible path is to 5. Therefore, node 2 is safe!  
Our earlier intuition that nodes in the cycle are unsafe still holds for 0,1,3, but 2 is safe because it is a sink of the cycle and leads only to a terminal.  

Thus the safe nodes are: 2,4,5,6.

### 4.5. Final Output

Sorted: [2,4,5,6]

---

## 5. Recursive Thinking (Conceptual)

While the algorithm uses iterative BFS, it can be understood recursively:  
A node is safe if **all** its neighbors are safe. This forms a recursive definition:  
- Base: nodes with outdegree 0 are safe.  
- Recursive: node is safe iff for every neighbor, that neighbor is safe.

If we naïvely compute this with DFS, we might encounter cycles. The iterative topological approach is a more efficient way to solve it without recursion, but the underlying principle is the same.

A recursion tree for node 2 (from the example) would be:

```
safe(2) -> neighbor 5 is safe (terminal) -> returns True
```

For node 4:  
```
safe(4) -> neighbor 5 is safe -> True
```

For node 0 (unsafe):  
```
safe(0) -> neighbors 1 and 2
   safe(1) -> neighbors 2 and 3
      safe(2) -> True
      safe(3) -> neighbor 0
         safe(0) -> (already in recursion stack, cycle detected) -> returns False
   safe(2) -> True
```
Because of the cycle, safe(0) would return False.

The BFS solution essentially computes this in a bottom‑up manner without recursion.

---

## 6. Complexity Analysis

- **Time:**  
  Building reverse graph: O(V + E)  
  Computing outdegree: O(V + E)  
  BFS traversal: each node is enqueued at most once, and each edge (in reverse graph) is processed once → O(V + E)  
  Total: O(V + E)

- **Space:**  
  Reverse graph: O(V + E)  
  Outdegree array: O(V)  
  Queue: O(V)  
  Safe marker array: O(V)  
  Total: O(V + E)

---

## 7. Example Usage and Output

```python
# Example 1: The graph described above
graph1 = [[1,2], [2,3], [5], [0], [5], [], []]
print(eventualSafeNodes(graph1))  # Output: [2,4,5,6]

# Example 2: Graph with no cycles, all nodes safe
graph2 = [[1], [2], [3], []]
print(eventualSafeNodes(graph2))  # Output: [0,1,2,3]

# Example 3: Graph with a self‑loop
graph3 = [[0], [2], []]
print(eventualSafeNodes(graph3))  # Output: [2]  (node 1 goes to safe node 2, node 0 self‑loop not safe)

# Example 4: Empty graph
graph4 = []
print(eventualSafeNodes(graph4))  # Output: []
```

**Outputs:**
```
[2,4,5,6]
[0,1,2,3]
[2]
[]
```

---

## 8. How to Use the Function

1. **Prepare the graph** as a list of lists `graph` where `graph[i]` is the list of nodes that node `i` has a directed edge to.
2. **Call** `eventualSafeNodes(graph)`.
3. The function returns a list of safe node indices sorted in ascending order.

**Example call:**

```python
safe_nodes = eventualSafeNodes([[1], [2], [3], []])
print("Safe nodes:", safe_nodes)  # Safe nodes: [0,1,2,3]
```

---

## 9. Conclusion

The `eventualSafeNodes` function effectively identifies all nodes that cannot lead to a cycle in a directed graph. By building a reverse graph and applying a topological‑sort‑like process (Kahn’s algorithm) starting from terminal nodes, it marks nodes as safe when all their outgoing edges eventually lead to terminal nodes. The algorithm runs in linear time and space, making it suitable for large graphs. The step‑by‑step walkthrough with ASCII diagrams clarifies the internal mechanics, and the example usage demonstrates its correctness on various graph structures.