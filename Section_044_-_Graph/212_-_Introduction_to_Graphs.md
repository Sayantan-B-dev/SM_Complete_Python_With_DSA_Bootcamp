# Graph Data Structure: Engineering Notes

## 1. Definition and Concept Overview

A graph is a non‑linear data structure defined as an ordered pair \( G = (V, E) \), where:

- \( V \) is a set of **vertices** (also called nodes).
- \( E \) is a set of **edges**, each edge connecting a pair of vertices.

Graphs model pairwise relationships between objects. Unlike linear structures (arrays, linked lists) or hierarchical structures (trees), graphs allow arbitrary connectivity patterns, including cycles and multiple disconnected components.

**Key distinction from trees:**
- A tree is a special case of a graph with exactly \( |V| - 1 \) edges, no cycles, and a single connected component.
- Graphs permit cycles, multiple connected components, and any number of edges up to \( |V|(|V|-1)/2 \) (in an undirected simple graph).

**Real‑world and industrial applications:**
- Social networks: vertices represent users, edges represent friendships or interactions.
- City maps / road networks: vertices are intersections, edges are roads (often weighted by distance or travel time).
- Flight routes: vertices are airports, edges are direct flights (directed if routes are one‑way).
- Google Maps: graph‑based shortest‑path algorithms (Dijkstra, A*) run on road networks.
- Search engines: the web is modelled as a directed graph where pages are vertices and hyperlinks are edges.
- Social media platforms: friend recommendation, community detection, and content propagation rely on graph analytics.

---

## 2. Core Principles and Internal Mechanics

### 2.1 Graph Types
- **Undirected graph**: edges have no direction; an edge \( (u, v) \) is identical to \( (v, u) \).
- **Directed graph (digraph)**: each edge has a direction, represented as an ordered pair \( (u, v) \) (from \( u \) to \( v \)).
- **Weighted graph**: each edge carries a numerical value (e.g., distance, cost, capacity).
- **Unweighted graph**: edges have no associated weight; connectivity is binary.
- **Simple graph**: no self‑loops (edge from a vertex to itself) and no parallel edges (multiple edges between the same pair of vertices).

### 2.2 Representations
Two principal representations exist, each with distinct trade‑offs:

| Representation | Space Complexity | Edge Lookup | Iterate Neighbors | Typical Use |
|----------------|------------------|-------------|-------------------|-------------|
| Adjacency Matrix | \(O(\|V\|^2)\) | \(O(1)\) | \(O(\|V\|)\) | Dense graphs, frequent edge‑existence checks |
| Adjacency List | \(O(\|V\| + \|E\|)\) | \(O(\deg(v))\) | \(O(\deg(v))\) | Sparse graphs, graph traversal |

**Adjacency List (selected representation for implementation)**  
An array or hash map where each vertex maps to a list (or set) of its adjacent vertices. For weighted graphs, each entry stores a tuple `(neighbor, weight)`.

### 2.3 Fundamental Properties
- **Degree**: number of incident edges (for directed graphs: indegree and outdegree).
- **Path**: sequence of vertices where consecutive vertices are connected by an edge.
- **Cycle**: a path with at least three vertices where the start and end vertices are the same and no other vertex repeats.
- **Connected component**: maximal set of vertices such that there exists a path between every pair.
- **Dense vs Sparse**: dense graphs have \(|E| \approx |V|^2\); sparse graphs have \(|E| \ll |V|^2\).

---

## 3. Step‑by‑Step Logical Breakdown

### 3.1 Graph Initialisation
1. Choose representation (adjacency list via `defaultdict(list)` or `dict`).
2. Maintain separate structures for vertices if needed (most implementations infer vertices from edges).

### 3.2 Adding a Vertex
- Insert the vertex as a key in the adjacency dictionary with an empty list (or set) as its value.
- If the vertex already exists, no operation is performed (or an exception can be raised, depending on contract).

### 3.3 Adding an Edge
- For undirected graphs: add `v` to `adj[u]` and `u` to `adj[v]`.
- For directed graphs: add `v` to `adj[u]` only.
- Optionally check for duplicate edges; if using a set instead of a list, duplicates are prevented automatically.
- For weighted graphs, store `(neighbor, weight)` instead of the neighbor alone.

### 3.4 Removing an Edge
- Locate the adjacency list/set of the source vertex and remove the target.
- For undirected graphs, repeat symmetrically.

### 3.5 Removing a Vertex
- Remove the vertex from the adjacency dictionary.
- Iterate over all remaining vertices and remove any occurrence of the deleted vertex from their adjacency lists/sets.
- Complexity \(O(|V| + |E|)\) for adjacency list.

### 3.6 Graph Traversal
Traversal algorithms are foundational for searching, connectivity checks, and topological ordering.

- **Breadth‑First Search (BFS)**: uses a queue; explores neighbours level‑by‑level. Yields shortest path (in unweighted graphs) and is optimal for exploring close‑by vertices first.
- **Depth‑First Search (DFS)**: uses a stack (or recursion); explores as far as possible before backtracking. Useful for cycle detection, topological sorting, and connectivity.

---

## 4. Implementation (Python)

The following implements an **undirected, unweighted graph** using an adjacency list stored as a dictionary mapping vertices to sets of neighbours. The set ensures \(O(1)\) edge existence checks and prevents duplicate edges.

```python
from collections import defaultdict

class Graph:
    """
    Undirected, unweighted graph using adjacency set representation.
    Supports adding/removing vertices and edges, BFS, and DFS traversal.
    """

    def __init__(self):
        # Adjacency map: vertex -> set of neighbor vertices
        self._adj = defaultdict(set)

    def add_vertex(self, v):
        """Add a vertex to the graph. No effect if vertex already exists."""
        if v not in self._adj:
            self._adj[v] = set()

    def add_edge(self, u, v):
        """
        Add an undirected edge between u and v.
        Both vertices are added automatically if not present.
        """
        if u == v:
            # Self‑loops are not supported in this simple graph implementation.
            # Raising an exception clarifies the limitation.
            raise ValueError("Self‑loops are not allowed")
        self.add_vertex(u)
        self.add_vertex(v)
        self._adj[u].add(v)
        self._adj[v].add(u)

    def remove_edge(self, u, v):
        """Remove the edge between u and v. Raises KeyError if edge does not exist."""
        # Safely remove from both adjacency sets
        try:
            self._adj[u].remove(v)
            self._adj[v].remove(u)
        except KeyError:
            # Re‑raise with a clearer message or silently ignore based on use case
            raise ValueError(f"Edge ({u}, {v}) not found")

    def remove_vertex(self, v):
        """
        Remove vertex v and all incident edges.
        """
        if v not in self._adj:
            return
        # Remove v from adjacency sets of all neighbors
        for neighbor in list(self._adj[v]):
            self._adj[neighbor].discard(v)
        # Delete the vertex entry
        del self._adj[v]

    def has_vertex(self, v):
        return v in self._adj

    def has_edge(self, u, v):
        """Check whether edge (u, v) exists."""
        return u in self._adj and v in self._adj[u]

    def neighbors(self, v):
        """Return a view of neighbors of vertex v."""
        if v not in self._adj:
            raise KeyError(f"Vertex {v} not found")
        return self._adj[v].copy()   # defensive copy

    def bfs(self, start):
        """
        Perform BFS from start vertex. Returns a list of vertices in BFS order.
        """
        if start not in self._adj:
            raise KeyError(f"Start vertex {start} not found")
        visited = set()
        order = []
        from collections import deque
        queue = deque([start])
        visited.add(start)

        while queue:
            vertex = queue.popleft()
            order.append(vertex)
            for neighbor in self._adj[vertex]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)
        return order

    def dfs(self, start):
        """
        Perform iterative DFS from start vertex. Returns a list of vertices in DFS preorder.
        """
        if start not in self._adj:
            raise KeyError(f"Start vertex {start} not found")
        visited = set()
        order = []
        stack = [start]

        while stack:
            vertex = stack.pop()
            if vertex not in visited:
                visited.add(vertex)
                order.append(vertex)
                # Push neighbors in reverse order to simulate recursive DFS order
                for neighbor in reversed(list(self._adj[vertex])):
                    if neighbor not in visited:
                        stack.append(neighbor)
        return order
```

---

## 5. Time and Space Complexity Analysis

All complexities are given for the adjacency‑set implementation above.

| Operation          | Time Complexity (Average) | Explanation |
|--------------------|---------------------------|-------------|
| `add_vertex`       | \(O(1)\)                  | Dictionary insertion |
| `add_edge`         | \(O(1)\)                  | Set insertion (amortized) |
| `remove_edge`      | \(O(1)\)                  | Set removal |
| `remove_vertex`    | \(O(\deg(v))\)            | Iterate over neighbours to delete incident edges |
| `has_vertex`       | \(O(1)\)                  | Dictionary lookup |
| `has_edge`         | \(O(1)\)                  | Set membership test |
| `neighbors`        | \(O(\deg(v))\)            | Copying the neighbor set (if copy is omitted, returns reference in \(O(1)\)) |
| BFS / DFS          | \(O(\|V\| + \|E\|)\)      | Each vertex and edge processed once |
| **Space Complexity** | \(O(\|V\| + \|E\|)\)     | Storage for adjacency sets plus vertex dictionary |

---

## 6. Edge Cases and Failure Scenarios

- **Self‑loops**: Implementation explicitly rejects self‑loops because many graph algorithms assume simple graphs. If self‑loops are required, the data structure can be adapted by allowing a vertex to appear in its own adjacency set.
- **Duplicate edges**: Using a set for adjacency eliminates duplicates automatically.
- **Removing a non‑existent vertex or edge**: `remove_vertex` silently returns if vertex absent; `remove_edge` raises `ValueError`. The choice depends on the intended contract (fail‑fast vs tolerant).
- **Isolated vertices**: Graph supports vertices with degree zero; they exist as keys in `_adj` with an empty set.
- **Disconnected graphs**: BFS/DFS return only the component reachable from the start vertex. To traverse all components, the caller must iterate over all vertices.
- **Large degree**: When removing a vertex, iterating over its neighbours is proportional to its degree; this is acceptable unless a vertex has degree \(O(|V|)\) (dense graph), in which case adjacency matrix may be more appropriate.
- **Mutability during iteration**: The implementation copies the neighbor set in `neighbors()` to prevent modification during iteration. In `remove_vertex`, `list(self._adj[v])` creates a copy to avoid runtime errors.

---

## 7. Practical Use Cases

- **Shortest path in road networks**: Weighted graph with Dijkstra’s algorithm.
- **Social network analysis**: BFS for friend recommendations, community detection.
- **Web crawling**: Directed graph of web pages; BFS used to traverse hyperlinks.
- **Dependency resolution (e.g., build systems)**: Directed acyclic graph (DAG) with topological sorting.
- **Network flow (e.g., routing, bandwidth)**: Weighted directed graphs with Ford‑Fulkerson.
- **Machine learning**: Graph neural networks (GNNs) operate on graph structures for node classification, link prediction.

---

## 8. Limitations and Trade‑offs

- **Adjacency list vs matrix**:
  - The adjacency list is space‑efficient for sparse graphs but edge existence checks are \(O(1)\) only when using a set (still constant time, but with higher constant factor than a matrix).
  - For dense graphs, adjacency matrix provides faster edge checks and better locality, at the cost of \(O(|V|^2)\) memory.
- **Thread safety**: The implementation is not thread‑safe; concurrent modifications require external synchronisation.
- **Weighted graphs**: The current implementation is unweighted. Weighted graphs can be supported by storing tuples in the adjacency set, but set membership then requires checking against the tuple’s first element – this adds complexity. An alternative is using a dictionary of dictionaries.
- **Dynamic graph updates**: Frequent removals of vertices or edges are supported, but each removal of a vertex scans all remaining vertices. For graphs with high churn, a more sophisticated data structure (e.g., adjacency list with linked lists, or a graph database) may be required.
- **No support for parallel edges**: By using a set, multiple edges between the same vertices are collapsed. If parallel edges are needed, adjacency must be stored as a list or a multiset.
- **Memory overhead per vertex**: The dictionary and set structures impose overhead that may become significant for very large graphs (millions of vertices). In such cases, specialised graph libraries (e.g., NetworkX, igraph) or disk‑based representations are preferred.