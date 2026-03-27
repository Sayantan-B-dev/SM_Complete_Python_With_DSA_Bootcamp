# The Has Path Problem

## 1. Problem Definition

Given an undirected graph, a source vertex `s` and a destination vertex `t`, determine whether there exists a path from `s` to `t`. This is a fundamental graph query that can be solved efficiently using either DFS or BFS. The key optimization is to stop the traversal as soon as `t` is discovered, avoiding unnecessary work.

---

## 2. Algorithm Overview

Both DFS and BFS can be used to explore the graph starting from `s`. If at any point the current vertex equals `t`, we immediately return `True`. If the exploration finishes without encountering `t`, we return `False`.

- **DFS**: Explores depth‑first, using recursion or an explicit stack. It may find a path quickly if `t` is deep, but can be inefficient if the graph is very deep without reaching `t`.
- **BFS**: Explores level‑by‑level, guaranteeing that we find the shortest path in terms of number of edges. It is often preferred when we only care about existence, as it explores all vertices at distance `k` before moving to `k+1`.

---

## 3. DFS‑Based Has Path

### 3.1 Recursive Implementation

The recursive DFS marks visited vertices to avoid cycles and stops when `t` is found.

```python
def has_path_dfs(self, source, destination):
    """
    Returns True if there is a path from source to destination.
    Uses recursive DFS with early termination.
    """
    visited = [False] * self.num_vertices

    def dfs(v):
        if v == destination:
            return True
        visited[v] = True
        for neighbor in range(self.num_vertices):
            if self.adj_matrix[v][neighbor] != 0 and not visited[neighbor]:
                if dfs(neighbor):
                    return True
        return False

    return dfs(source)
```

**Algorithm Steps**:
1. Initialize `visited` array of size `V` to `False`.
2. Define a recursive helper `dfs(v)`:
   - If `v == destination`, return `True`.
   - Mark `v` as visited.
   - For each neighbor `u` of `v` (where edge exists and `u` not visited):
     - Recursively call `dfs(u)`. If it returns `True`, propagate `True` upward.
   - If no neighbor leads to destination, return `False`.
3. Call `dfs(source)` and return the result.

**Termination**:
- The recursion returns `True` as soon as the destination is reached.
- If the entire component is explored without finding `t`, `False` is returned.

### 3.2 Iterative DFS (Stack‑Based)

An iterative version using an explicit stack can avoid recursion depth limits.

```python
def has_path_dfs_iterative(self, source, destination):
    visited = [False] * self.num_vertices
    stack = [source]

    while stack:
        v = stack.pop()
        if v == destination:
            return True
        if not visited[v]:
            visited[v] = True
            # Push neighbors; order not important for existence
            for neighbor in range(self.num_vertices):
                if self.adj_matrix[v][neighbor] != 0 and not visited[neighbor]:
                    stack.append(neighbor)
    return False
```

---

## 4. BFS‑Based Has Path

BFS is often the method of choice because it naturally explores in layers and can be more efficient in practice for existence queries.

```python
from collections import deque

def has_path_bfs(self, source, destination):
    """
    Returns True if there is a path from source to destination.
    Uses BFS with early termination.
    """
    if source == destination:
        return True

    visited = [False] * self.num_vertices
    q = deque([source])
    visited[source] = True

    while q:
        v = q.popleft()
        # Check neighbors
        for neighbor in range(self.num_vertices):
            if self.adj_matrix[v][neighbor] != 0:
                if neighbor == destination:
                    return True
                if not visited[neighbor]:
                    visited[neighbor] = True
                    q.append(neighbor)
    return False
```

**Algorithm Steps**:
1. If `source == destination`, return `True` immediately.
2. Initialize `visited` array and queue with `source`, mark `source` visited.
3. While queue not empty:
   - Dequeue vertex `v`.
   - For each neighbor `u` of `v`:
     - If `u == destination`, return `True`.
     - If `u` not visited, mark visited and enqueue `u`.
4. If queue empties without finding `destination`, return `False`.

**Why BFS May Be Preferable**:
- In graphs with a large branching factor, BFS can often find the destination faster because it explores all possibilities at the current distance before going deeper.
- BFS finds the shortest path (in unweighted graphs), but for existence only, early termination works similarly for both.

---

## 5. Complexity Analysis

- **Time Complexity**: \(O(V + E)\) for adjacency list representation, \(O(V^2)\) for adjacency matrix. In our implementation using an adjacency matrix, each vertex scans all \(V\) columns, leading to \(O(V^2)\).
- **Space Complexity**: \(O(V)\) for the `visited` array plus stack/queue overhead. The recursion depth in DFS can also be \(O(V)\) in worst case.

---

## 6. Handling Disconnected Graphs

Both algorithms inherently handle disconnected graphs because they only explore the component containing the source. If the destination lies in a different component, the traversal will exhaust the source's component without finding it and return `False`. No special driver loop is needed.

---

## 7. Complete Code Example (Using GraphAdjacencyMatrix)

```python
from collections import deque

class GraphAdjacencyMatrix:
    # ... (existing methods)

    def has_path_dfs(self, source, destination):
        visited = [False] * self.num_vertices

        def dfs(v):
            if v == destination:
                return True
            visited[v] = True
            for neighbor in range(self.num_vertices):
                if self.adj_matrix[v][neighbor] != 0 and not visited[neighbor]:
                    if dfs(neighbor):
                        return True
            return False

        return dfs(source)

    def has_path_bfs(self, source, destination):
        if source == destination:
            return True
        visited = [False] * self.num_vertices
        q = deque([source])
        visited[source] = True
        while q:
            v = q.popleft()
            for neighbor in range(self.num_vertices):
                if self.adj_matrix[v][neighbor] != 0:
                    if neighbor == destination:
                        return True
                    if not visited[neighbor]:
                        visited[neighbor] = True
                        q.append(neighbor)
        return False

# Example usage
if __name__ == "__main__":
    # Build a graph (same as earlier)
    graph = GraphAdjacencyMatrix(7)
    # ... (add vertices and edges)
    # Check path from 0 to 4
    print(graph.has_path_dfs(0, 4))   # True
    print(graph.has_path_bfs(0, 4))   # True
    print(graph.has_path_dfs(0, 5))   # False (different component)
```

---

## 8. Summary

- The **has path** problem is a basic graph query that can be solved by DFS or BFS with early termination.
- **DFS** is simple to implement recursively and may find a path quickly if the destination is deep.
- **BFS** explores level‑by‑level and is often preferred for its predictability and shortest‑path guarantee.
- Both algorithms run in \(O(V^2)\) when using an adjacency matrix, but can be improved to \(O(V+E)\) with an adjacency list.
- Early termination (stopping as soon as the destination is reached) prevents unnecessary traversal and improves performance.