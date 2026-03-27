# Counting Connected Components in a Graph

## 1. Definition

A **connected component** of an undirected graph is a maximal subgraph in which any two vertices are connected by a path, and no vertex in the subgraph is connected to any vertex outside it. A graph that is itself connected has exactly one component; a disconnected graph has two or more components.

Counting the number of connected components is a fundamental operation that can be performed using either DFS or BFS. The algorithm is straightforward: perform a traversal from every unvisited vertex; each time a new traversal is initiated, a new component is discovered.

---

## 2. Algorithm

### 2.1 Using DFS (Recursive)

1. Initialize a `visited` array of size `|V|` to `False`.
2. Initialize a counter `components = 0`.
3. For each vertex `v` in the range `0..|V|-1`:
   - If `visited[v]` is `False`:
     - Increment `components` by 1.
     - Call `dfs_recursive(v)` to mark all vertices reachable from `v` as visited.
4. Return `components`.

### 2.2 Using BFS (Iterative)

The same logic applies, replacing the recursive DFS with a BFS (using a queue).

---

## 3. Implementation (Adjacency Matrix Version)

The following code extends the previously defined `GraphAdjacencyMatrix` class to include a method `count_components()`. Both DFS‑based and BFS‑based implementations are shown.

```python
from collections import deque

class GraphAdjacencyMatrix:
    # ... (existing __init__, add_vertex, add_edge, display, etc.)

    # DFS-BASED COMPONENT COUNT
    def count_components_dfs(self):
        """
        Returns the number of connected components using DFS.
        """
        visited = [False] * self.num_vertices
        components = 0

        def dfs(v):
            """DFS that marks all vertices in the component."""
            visited[v] = True
            for neighbor in range(self.num_vertices):
                if self.adj_matrix[v][neighbor] != 0 and not visited[neighbor]:
                    dfs(neighbor)

        for v in range(self.num_vertices):
            if not visited[v]:
                components += 1          # new component found
                dfs(v)                  # mark its entire component

        return components

    # BFS-BASED COMPONENT COUNT
    def count_components_bfs(self):
        """
        Returns the number of connected components using BFS.
        """
        visited = [False] * self.num_vertices
        components = 0
        q = deque()

        for v in range(self.num_vertices):
            if not visited[v]:
                components += 1
                # BFS for this component
                q.append(v)
                visited[v] = True
                while q:
                    u = q.popleft()
                    for w in range(self.num_vertices):
                        if self.adj_matrix[u][w] != 0 and not visited[w]:
                            visited[w] = True
                            q.append(w)

        return components
```

---

## 4. Step‑by‑Step Walkthrough (Example Graph)

Consider the disconnected graph used in earlier examples with vertices 0‑6 and edges:
- Component A: 0‑1‑2‑3‑4  (with edges 0‑1, 1‑2, 2‑3, 2‑4, 3‑4)
- Component B: 5‑6          (edge 5‑6)

### 4.1 DFS‑Based Counting

| Step | Vertex `v` | `visited[v]` | Action | Components |
|------|------------|--------------|--------|------------|
| 0    | –          | –            | Initialize visited all False | 0 |
| 1    | 0          | False        | components++ → 1; call dfs(0). DFS marks 0,1,2,3,4. | 1 |
| 2    | 1          | True         | Skip | 1 |
| 3    | 2          | True         | Skip | 1 |
| 4    | 3          | True         | Skip | 1 |
| 5    | 4          | True         | Skip | 1 |
| 6    | 5          | False        | components++ → 2; call dfs(5). DFS marks 5,6. | 2 |
| 7    | 6          | True         | Skip | 2 |
| 8    | –          | –            | Return 2 | 2 |

### 4.2 BFS‑Based Counting

The BFS approach follows an analogous pattern, marking all vertices of each component during the BFS traversal.

---

## 5. Complexity Analysis

| Implementation | Time Complexity | Space Complexity |
|----------------|-----------------|------------------|
| **DFS (Adjacency Matrix)** | \(O(V^2)\) – each vertex scans all \(V\) neighbors | \(O(V)\) for visited and recursion stack (worst‑case) |
| **BFS (Adjacency Matrix)** | \(O(V^2)\) – same scanning overhead | \(O(V)\) for visited and queue |
| **DFS (Adjacency List)** | \(O(V + E)\) | \(O(V)\) (plus recursion stack) |
| **BFS (Adjacency List)** | \(O(V + E)\) | \(O(V)\) |

- The time complexity is dominated by the graph representation. Adjacency matrix forces \(O(V^2)\) even for sparse graphs; adjacency list yields linear time.
- Space complexity is \(O(V)\) for auxiliary structures (visited array, recursion stack, or queue).

---

## 6. Key Points

- **Counting components** is essentially running a traversal from every unvisited vertex.
- Each time a new traversal starts, a new component is detected.
- The underlying traversal (DFS or BFS) marks all vertices belonging to that component.
- The `visited` array is shared across all components; it is never reset between components.
- For directed graphs, the concept changes to **strongly connected components**, which requires more advanced algorithms (e.g., Kosaraju’s or Tarjan’s).

---

## 7. Complete Example Usage

```python
if __name__ == "__main__":
    graph = GraphAdjacencyMatrix(7)
    graph.add_vertex(0, 'A'); graph.add_vertex(1, 'B'); graph.add_vertex(2, 'C')
    graph.add_vertex(3, 'D'); graph.add_vertex(4, 'E'); graph.add_vertex(5, 'F')
    graph.add_vertex(6, 'G')

    graph.add_edge(0,1); graph.add_edge(1,2); graph.add_edge(2,3)
    graph.add_edge(3,4); graph.add_edge(2,4); graph.add_edge(5,6)

    print("Components (DFS):", graph.count_components_dfs())  # Output: 2
    print("Components (BFS):", graph.count_components_bfs())  # Output: 2
```

---

## 8. Summary

Counting connected components is a simple yet essential graph operation. The algorithm leverages the fact that a traversal (DFS or BFS) from a vertex will cover its entire component. By iterating over all vertices and initiating a traversal only when a vertex is unvisited, we can count the components in linear time relative to the graph representation.