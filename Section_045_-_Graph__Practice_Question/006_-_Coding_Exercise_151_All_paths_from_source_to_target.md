## Table of Contents

1. **Title and Description**  
2. **Full Commented Code**  
3. **Algorithm Overview**  
   - 3.1. Problem Definition  
   - 3.2. DFS with Backtracking  
   - 3.3. Key Properties of a DAG  
4. **Step‑by‑Step Workflow with ASCII Diagrams**  
   - 4.1. Example Graph  
   - 4.2. DFS Recursion Tree  
   - 4.3. Path Construction and Backtracking  
   - 4.4. Stack Frame Evolution  
5. **Recursion Tree Visualization**  
6. **Complexity Analysis**  
7. **Example Usage and Output**  
8. **How to Use the Function**  
9. **Conclusion**

---

## 1. Title and Description

**Title:** Find All Paths from Source to Target in a Directed Acyclic Graph (DAG)

**Description:**  
This function `all_paths_source_target(graph)` returns **all possible paths** from node `0` to node `n‑1` in a **directed acyclic graph (DAG)** represented by an adjacency list. The graph is guaranteed to have no cycles, so the number of paths is finite.  

The solution uses a **depth‑first search (DFS)** with **backtracking** to explore all routes from the source to the target. Each time the target node is reached, a copy of the current path is saved. The algorithm ensures that every simple path (no repeated nodes, because DAG guarantees no cycles) is discovered exactly once.

---

## 2. Full Commented Code

```python
def all_paths_source_target(graph):
    """
    Function to find all possible paths from node 0 to node n - 1 in a DAG.

    :param graph: List[List[int]] -> Adjacency list representing the DAG
    :return: List[List[int]] -> List of all possible paths from node 0 to node n - 1
    """
    target_node = len(graph) - 1
    all_paths_result = []

    def depth_first_search(current_node, current_path):
        # Add current node to path
        current_path.append(current_node)

        # If target reached, store a copy of the path
        if current_node == target_node:
            all_paths_result.append(list(current_path))
        else:
            # Explore all neighbors
            for neighbor_node in graph[current_node]:
                depth_first_search(neighbor_node, current_path)

        # Backtrack to explore other paths
        current_path.pop()

    # Start DFS from source node 0
    depth_first_search(0, [])

    return all_paths_result


# Expected Output Example:
# Input:
# graph = [[1,2],[3],[3],[]]
# Output:
# [[0,1,3],[0,2,3]]
```

---

## 3. Algorithm Overview

### 3.1. Problem Definition
We are given a **directed acyclic graph** (DAG) with `n` nodes (labeled 0 to n‑1). The graph is represented by an adjacency list `graph` where `graph[i]` lists all nodes that can be reached from `i` via a directed edge. The goal is to find every possible **path** from node `0` (source) to node `n‑1` (target). A path is a sequence of nodes where consecutive nodes are connected by an edge, and nodes are not repeated (implied by the DAG property).

### 3.2. DFS with Backtracking
The algorithm performs a depth‑first traversal from node `0`. It maintains a **current path** (`current_path`) that grows as we move deeper. When a neighbor is visited, the function recurses. Once a path reaches the target node, a copy of the current path is appended to the result list. After exploring all neighbours of a node, the function **backtracks** by removing the last node from the path, allowing alternative routes to be explored.

### 3.3. Key Properties of a DAG
Because the graph is a DAG, there are no cycles. Hence, the DFS will not get stuck in infinite loops, and each path is simple (no node appears twice). The total number of paths may be exponential in the worst case (e.g., in a DAG that is a layered graph), but the algorithm enumerates all of them.

---

## 4. Step‑by‑Step Workflow with ASCII Diagrams

We will use the provided example:

```
graph = [[1,2],   # node 0 -> 1, 2
         [3],     # node 1 -> 3
         [3],     # node 2 -> 3
         []]      # node 3 has no outgoing edges
```

The graph looks like this (directed edges):

```
0 → 1 → 3
 ↘
  2 → 3
```

### 4.1. Example Graph

```
      ┌─→ 1 ─→ 3
      │
0 ───┤
      │
      └─→ 2 ─→ 3
```

Node 3 is the target (n‑1 = 3). There are two distinct paths from 0 to 3: `[0,1,3]` and `[0,2,3]`.

### 4.2. DFS Recursion Tree
The DFS calls can be visualised as a tree of recursive invocations:

```
dfs(0, [])
│
├─ neighbor 1 → dfs(1, [0])
│   │
│   └─ neighbor 3 → dfs(3, [0,1])
│       │
│       └─ target reached → save [0,1,3]
│
└─ neighbor 2 → dfs(2, [0])
    │
    └─ neighbor 3 → dfs(3, [0,2])
        │
        └─ target reached → save [0,2,3]
```

### 4.3. Path Construction and Backtracking
Let’s simulate the process step by step with the call stack:

**Initial call:** `dfs(0, [])`  
- `current_path` = `[0]` (after append)  
- Not target → loop over neighbors: `1` and `2`.

**First recursion:** `dfs(1, [0])`  
- Append `1` → path = `[0,1]`  
- Not target (1 ≠ 3) → neighbors: `3`  
  - `dfs(3, [0,1])`  
    - Append `3` → path = `[0,1,3]`  
    - Target reached → save a copy `[0,1,3]` to `all_paths_result`  
    - No neighbors → backtrack: pop `3` → path = `[0,1]`  
  - End of loop → backtrack: pop `1` → path = `[0]`

**Second recursion:** `dfs(2, [0])` (after returning from the first branch)  
- Append `2` → path = `[0,2]`  
- Not target → neighbors: `3`  
  - `dfs(3, [0,2])`  
    - Append `3` → path = `[0,2,3]`  
    - Target reached → save `[0,2,3]`  
    - Backtrack: pop `3` → path = `[0,2]`  
  - End of loop → backtrack: pop `2` → path = `[0]`

**Final:** Backtrack from root: pop `0` → path = `[]`.

Result: `[[0,1,3], [0,2,3]]`.

### 4.4. Stack Frame Evolution
We can visualise the stack as the recursion proceeds (depth increases):

```
Frame: dfs(0, [])        path = []
  → Append 0 → [0]
  → Recurse to 1

Frame: dfs(1, [0])       path = [0]
  → Append 1 → [0,1]
  → Recurse to 3

Frame: dfs(3, [0,1])     path = [0,1]
  → Append 3 → [0,1,3]
  → Save path → [0,1,3]
  → Backtrack (pop 3) → [0,1]
  → Return

Frame: back to dfs(1, [0])   path = [0,1]
  → Backtrack (pop 1) → [0]
  → Return

Frame: back to dfs(0, [])    path = [0]
  → Next neighbor 2

Frame: dfs(2, [0])       path = [0]
  → Append 2 → [0,2]
  → Recurse to 3

Frame: dfs(3, [0,2])     path = [0,2]
  → Append 3 → [0,2,3]
  → Save path → [0,2,3]
  → Backtrack (pop 3) → [0,2]
  → Return

Frame: back to dfs(2, [0])   path = [0,2]
  → Backtrack (pop 2) → [0]
  → Return

Frame: back to dfs(0, [])    path = [0]
  → Backtrack (pop 0) → []
  → Return
```

---

## 5. Recursion Tree Visualization

We can draw the full recursion tree with the current path at each node:

```
dfs(0, [])
├─ dfs(1, [0])
│   └─ dfs(3, [0,1])  → saves [0,1,3]
└─ dfs(2, [0])
    └─ dfs(3, [0,2])  → saves [0,2,3]
```

Each leaf of this tree corresponds to a complete path from 0 to 3. The tree has no overlapping subproblems because the graph is a DAG, but the recursion explores every possible route.

---

## 6. Complexity Analysis

- **Time:**  
  In the worst case (e.g., a DAG that is a complete DAG or a layered graph), the number of paths can be exponential in `n`. For example, a graph where each node `i` (except the last) connects to all later nodes can have `2^(n‑2)` paths. The DFS visits each path once, and constructing each path takes O(n) time (copying). Therefore, the time complexity is **O(n * 2^(n‑2))** in the worst case. For sparse DAGs, it is much smaller.

- **Space:**  
  The recursion depth is at most `n` (the longest path). The current path list holds at most `n` nodes. The result list stores all paths, which in the worst case can be exponential in size. Hence, the space complexity is **O(n * number_of_paths)** for the output plus O(n) recursion stack.

---

## 7. Example Usage and Output

```python
# Example 1: The given DAG
graph1 = [[1,2],[3],[3],[]]
print(all_paths_source_target(graph1))  # Output: [[0,1,3],[0,2,3]]

# Example 2: Linear DAG (single path)
graph2 = [[1],[2],[3],[]]
print(all_paths_source_target(graph2))  # Output: [[0,1,2,3]]

# Example 3: DAG with multiple paths and a node with degree 0 before target
graph3 = [[1,2],[3],[3,4],[4],[]]
print(all_paths_source_target(graph3))  
# Output: [[0,1,3,4], [0,2,3,4], [0,2,4]]

# Example 4: Single node (n=1) – no edges, source=target
graph4 = [[]]
print(all_paths_source_target(graph4))  # Output: [[0]]
```

**Outputs:**
```
[[0,1,3],[0,2,3]]
[[0,1,2,3]]
[[0,1,3,4],[0,2,3,4],[0,2,4]]
[[0]]
```

---

## 8. How to Use the Function

1. **Prepare the graph** as an adjacency list: a list of lists where `graph[i]` contains all nodes reachable from `i` via a directed edge.  
   - The graph must be a **DAG** (no cycles).  
   - Node indices must be from `0` to `n‑1`.  
2. **Call** `all_paths_source_target(graph)`.  
3. The function returns a list of lists, each inner list representing a path from node `0` to node `n‑1`.

**Example with a custom DAG:**

```python
dag = [[1,2],[3],[3,4],[4],[],[]]   # n=6, target=5 (but note: no path from 0 to 5)
print(all_paths_source_target(dag))  # Output: [] (since target 5 is unreachable)
```

**Important:** The function assumes the graph is a DAG; if there are cycles, it may still work but could produce infinite paths (though the recursion would eventually hit a depth limit or recurse indefinitely). The problem statement guarantees a DAG.

---

## 9. Conclusion

The `all_paths_source_target` function enumerates all simple paths from the source to the target in a DAG using depth‑first search with backtracking. The algorithm is straightforward and leverages the property of acyclic graphs to avoid infinite loops. The step‑by‑step illustration with ASCII diagrams and recursion trees clarifies the traversal order and path construction. While the worst‑case complexity can be exponential, this is inherent to the problem of listing all paths. The function is efficient for moderate‑sized graphs and is a classic example of DFS‑based path enumeration.