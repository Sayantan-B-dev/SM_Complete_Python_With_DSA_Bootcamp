## 1. Definition and Core Concept

An **adjacency list** represents a graph by storing, for each vertex, a list of its neighboring vertices (and optionally edge weights). It is a collection of lists or arrays, one per vertex.

- For **directed graphs**, the list for vertex \(u\) contains vertices \(v\) such that \((u,v)\) is an edge.
- For **undirected graphs**, each edge appears twice: once in the list of each endpoint.
- Edge weights can be stored alongside the neighbor (e.g., as a tuple \((v, w)\)).

The adjacency list is one of the most commonly used graph representations because it balances space efficiency and fast neighbor iteration.

---

## 2. Key Aspects

| Aspect | Description |
|--------|-------------|
| **Data Structure** | An array/list of size \(\|V\|\) where each entry is a linked list, dynamic array, or set. |
| **Space Complexity** | \(O(|V| + |E|)\) – ideal for sparse graphs. |
| **Edge Storage** | Each undirected edge stored twice (once per endpoint); each directed edge stored once. |
| **Weight Support** | Trivially extended by storing pairs (neighbor, weight). |
| **Directed vs Undirected** | Directed: only forward edges stored. Undirected: both directions stored. |
| **Query Types** | Fast iteration over neighbors; adjacency check can be \(O(\deg(v))\) if list is unsorted, or \(O(\log \deg(v))\) if using balanced trees / sets. |

---

## 3. Diagrammatic Representation

### Example: Undirected Graph

```
Vertices: A, B, C, D
Edges: A-B, A-C, B-D

Adjacency List:
A -> [B, C]
B -> [A, D]
C -> [A]
D -> [B]
```

### Example: Directed Weighted Graph

```
Vertices: X, Y, Z
Edges: X→Y (weight 5), X→Z (weight 3), Y→Z (weight 2)

Adjacency List:
X -> [(Y,5), (Z,3)]
Y -> [(Z,2)]
Z -> []
```

---

## 4. Use Cases

- **Sparse graphs** (most real-world graphs: social networks, web links, road maps).
- **Graph algorithms** that need to explore neighbors (BFS, DFS, Dijkstra, topological sort).
- **Dynamic graphs** where vertices/edges are frequently added or removed.
- **Large graphs** where an adjacency matrix would consume too much memory.

---

## 5. Advantages

1. **Memory efficient** for sparse graphs – only stores existing edges.
2. **Fast neighbor iteration** – \(O(\deg(v))\) to traverse all adjacent vertices.
3. **Supports both directed and undirected** with simple modifications.
4. **Easily extensible** to weighted, multi‑graph, or with extra data per edge.
5. **Good for incremental updates** – adding a vertex or edge is fast (amortized \(O(1)\)).

---

## 6. Disadvantages

1. **Adjacency check** can be slow if the list is unsorted – may need to scan all neighbors (\(O(\deg(v))\)).
2. **Less cache‑friendly** than adjacency matrix for dense graphs (due to pointer chasing).
3. **Edge removal** may require searching through the neighbor list, though it can be optimized with sets.
4. **Weighted edges** slightly increase storage (store tuples instead of simple neighbors).
5. **Not ideal for very dense graphs** – overhead of storing each edge twice can exceed matrix memory when \(|E|\) approaches \(|V|^2\).

---

## 7. Algorithms and Operations (with Complexities)

Assume the graph is stored with hash‑based adjacency (Python dict mapping vertex → list of neighbors).  
\(n = |V|\), \(m = |E|\).

| Operation | Time Complexity (with list) | Time Complexity (with set) |
|-----------|----------------------------|---------------------------|
| Add Vertex | \(O(1)\) amortized | \(O(1)\) |
| Add Edge | \(O(1)\) amortized | \(O(1)\) |
| Remove Edge | \(O(\deg(v))\) (search + delete) | \(O(1)\) (if set) |
| Check Adjacency | \(O(\deg(v))\) (scan list) | \(O(1)\) average |
| Iterate Over Neighbors | \(O(\deg(v))\) | \(O(\deg(v))\) |
| Space | \(O(n + m)\) | \(O(n + m)\) (sets add overhead) |

For **directed** graphs, only the source’s list is updated on add; for **undirected**, both endpoints’ lists are updated.

---

## 8. Implementation (Python)

Below is a clean implementation with comments explaining each method.

```python
class GraphAdjacencyList:
    """Undirected graph using adjacency list stored as dict: vertex -> list of (neighbor, weight)."""

    def __init__(self):
        # Store vertices in a list (optional, could derive from keys of adj_list)
        self.V = []
        # Adjacency dictionary: key = vertex, value = list of (neighbor, weight)
        self.adj_list = {}

    def add_vertex(self, vertex):
        """Add a vertex to the graph if not already present."""
        if vertex not in self.V:
            self.V.append(vertex)
            self.adj_list[vertex] = []   # initialize empty neighbor list
        else:
            print(f"Vertex {vertex} already exists")

    def add_edge(self, source, destination, weight=1):
        """Add an undirected edge between source and destination with optional weight."""
        if source in self.V and destination in self.V:
            # Add edge in both directions (undirected)
            self.adj_list[source].append((destination, weight))
            self.adj_list[destination].append((source, weight))
        else:
            print("One or both vertices not found.")

    def display(self):
        """Print the adjacency list."""
        for vertex, neighbors in self.adj_list.items():
            print(f"{vertex} : {neighbors}")

    # Additional useful methods
    def neighbors(self, vertex):
        """Return list of neighbors of vertex (without weights)."""
        return [neighbor for neighbor, _ in self.adj_list.get(vertex, [])]

    def has_edge(self, u, v):
        """Check if edge (u, v) exists (undirected)."""
        return any(neighbor == v for neighbor, _ in self.adj_list.get(u, []))

    def degree(self, vertex):
        """Return the degree of vertex (number of incident edges)."""
        return len(self.adj_list.get(vertex, []))

# Example usage
if __name__ == "__main__":
    g = GraphAdjacencyList()
    for v in ['A', 'B', 'C', 'D', 'E']:
        g.add_vertex(v)

    edges = [('A','B'), ('A','D'), ('B','D'), ('B','C')]
    for u, v in edges:
        g.add_edge(u, v)

    g.display()
    print("Degree of B:", g.degree('B'))
    print("Neighbors of A:", g.neighbors('A'))
    print("Edge A-D exists?", g.has_edge('A','D'))
```

---

## 9. Comparison with Other Representations

| Feature | Adjacency List | Adjacency Matrix | Edge List |
|--------|----------------|------------------|-----------|
| Space | \(O(n + m)\) | \(O(n^2)\) | \(O(n + m)\) |
| Check adjacency | \(O(\deg(v))\) (list) / \(O(1)\) (set) | \(O(1)\) | \(O(m)\) |
| Iterate neighbors | \(O(\deg(v))\) | \(O(n)\) | \(O(m)\) |
| Add vertex | \(O(1)\) | \(O(n^2)\) (resize matrix) | \(O(1)\) |
| Add edge | \(O(1)\) | \(O(1)\) | \(O(1)\) |
| Remove edge | \(O(\deg(v))\) (list) / \(O(1)\) (set) | \(O(1)\) | \(O(m)\) |
| Best for | Sparse graphs, neighbor‑centric algorithms | Dense graphs, quick adjacency checks | Edge‑centric algorithms (e.g., Kruskal) |

---

## 10. Summary

The adjacency list is the go‑to representation for most graph problems because it scales well to large, sparse graphs and enables efficient traversal. It is implemented using a dictionary (or array) of lists, and can be easily adapted for directed, weighted, and even multi‑graphs. While it sacrifices constant‑time adjacency checks (unless using sets), its memory efficiency and fast neighbor iteration make it the preferred choice in practice.