# Graph Traversal on Disconnected Graphs

## 1. The Problem

Graphs may be **disconnected**—they consist of multiple connected components where no path exists between vertices of different components. Standard traversal algorithms (DFS, BFS) starting from a single source will only visit the component containing that source. To process the entire graph, we must invoke the traversal for each component separately.

This documentation explains how to extend the previously implemented DFS and BFS to handle disconnected graphs.

---

## 2. General Approach

The core idea is:

1. Maintain a `visited` array (or set) to track which vertices have been discovered.
2. Iterate over all vertices in the graph.
3. For each vertex that is not yet visited, start a new traversal (DFS or BFS) from that vertex.
4. Each such call will explore one connected component.

This approach works for both DFS and BFS, and the underlying recursive or iterative algorithm remains unchanged—it is simply wrapped by a driver function.

---

## 3. Disconnected DFS (Recursive)

### 3.1 Implementation

The original `dfs()` method used a loop over vertices to call `dfs_recursive` for each unvisited vertex. This is already a correct driver for disconnected graphs.

```python
def dfs(self):
    """
    Traverses all vertices, handling disconnected components.
    """
    dfs_result = []
    visited = [False] * self.num_vertices

    # Loop over every vertex; if unvisited, start a new DFS component.
    for vertex in range(self.num_vertices):
        if not visited[vertex]:
            print("Calling for a component")   # Debug
            self.dfs_recursive(vertex, dfs_result, visited)

    # (Optionally return dfs_result)
```

### 3.2 How It Works

- The `visited` array is shared across components.
- When a component is fully explored, the loop continues to the next vertex.
- If that vertex is already visited (because it belonged to the previous component), it is skipped.
- If it is unvisited, a new component is discovered and explored.

### 3.3 Example Output for Disconnected Graph

Using the graph from earlier (components: {0,1,2,3,4} and {5,6}), the output would be:

```
Calling for a component
0
1
2
3
4
Calling for a component
5
6
```

---

## 4. Disconnected BFS

### 4.1 Strategy

BFS can be adapted similarly:

- Implement a helper method `bfs_component(start, visited)` that performs BFS on the component containing `start`, using the shared `visited` array.
- In a driver method `bfs_all()`, iterate over all vertices; for each unvisited vertex, call `bfs_component`.

### 4.2 Code Implementation

```python
def bfs_component(self, start_vertex, visited):
    """
    BFS for a single connected component.
    """
    queue = deque([start_vertex])
    visited[start_vertex] = True

    while queue:
        vertex = queue.popleft()
        print(vertex)                     # Process vertex
        # Optionally add to a result list

        # Find neighbors (adjacency matrix scan)
        for neighbor in range(self.num_vertices):
            if self.adj_matrix[vertex][neighbor] != 0:
                if not visited[neighbor]:
                    visited[neighbor] = True
                    queue.append(neighbor)

def bfs_all(self):
    """
    Performs BFS on all components of the graph.
    """
    visited = [False] * self.num_vertices

    for vertex in range(self.num_vertices):
        if not visited[vertex]:
            print("Calling BFS for a component")   # Debug
            self.bfs_component(vertex, visited)
```

### 4.3 Example Output for Disconnected Graph

Using the same graph (start vertex 0 is the first unvisited), the output would be:

```
Calling BFS for a component
0
1
2
3
4
Calling BFS for a component
5
6
```

### 4.4 Important Note on Marking Visited

- In BFS, it is crucial to mark vertices **as visited when they are enqueued**, not when they are dequeued. This prevents multiple enqueues.
- The helper `bfs_component` follows this rule correctly.

---

## 5. Complexity

| Operation | Complexity (adjacency matrix) |
|-----------|-------------------------------|
| **DFS (disconnected)** | \(O(V^2)\) – because each vertex scans all \(V\) columns for neighbors. |
| **BFS (disconnected)** | \(O(V^2)\) – same reason. |
| **Space** | \(O(V)\) for visited array and recursion stack (DFS) / queue (BFS). |

---

## 6. Summary

- Disconnected graphs require a **driver loop** that initiates a traversal from every unvisited vertex.
- The underlying DFS or BFS algorithm remains unchanged; it simply works on one component.
- The shared `visited` array ensures vertices are processed exactly once across all components.
- This pattern is applicable to any graph traversal and is essential for algorithms that need to examine the entire graph.