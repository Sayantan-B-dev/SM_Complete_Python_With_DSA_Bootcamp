## 1. Undirected Graph

### Definition
A graph where edges have no direction. An edge between vertices \(u\) and \(v\) is an unordered pair \(\{u, v\}\), meaning the connection is mutual.

### Key Aspects
- Edges are bidirectional.
- The adjacency matrix is symmetric.
- Commonly represented using adjacency lists or matrices.

### Diagram
```
A ----- B
|       |
|       |
C ----- D
```
Vertices: A, B, C, D  
Edges: AB, AC, BD, CD

### Use Cases
- Social networks (friendship is mutual).
- Road networks with two‑way streets.
- Molecular structures (bonds are mutual).

### Advantages
- Simpler to model symmetric relationships.
- Algorithms like BFS/DFS are straightforward.

### Disadvantages
- Cannot represent one‑way relationships.
- May waste space if relationships are inherently directed.

### Python Code Snippet (Adjacency List)
```python
class UndirectedGraph:
    def __init__(self):
        self.graph = {}

    def add_edge(self, u, v):
        # Add edge both ways
        self.graph.setdefault(u, []).append(v)
        self.graph.setdefault(v, []).append(u)

    def display(self):
        for node, neighbors in self.graph.items():
            print(f"{node}: {neighbors}")

# Example
g = UndirectedGraph()
g.add_edge('A', 'B')
g.add_edge('A', 'C')
g.add_edge('B', 'D')
g.display()
```

---

## 2. Directed Graph (Digraph)

### Definition
A graph where every edge has a direction. An edge is an ordered pair \((u, v)\), indicating a one‑way connection from \(u\) to \(v\).

### Key Aspects
- Edges are asymmetric.
- Two edges can exist in opposite directions between the same vertices.
- Can be *strongly connected* (path both ways) or *weakly connected* (underlying undirected is connected).

### Diagram
```
A → B
↑   ↓
C ← D
```
Edges: A→B, A→C, B→D, D→C

### Use Cases
- Web page links (hyperlinks are one‑way).
- Twitter “follows” relationships.
- Workflow dependencies (task A must finish before task B).

### Advantages
- Captures asymmetric relationships naturally.
- Enables modeling of flows, dependencies, and hierarchies.

### Disadvantages
- More complex traversal (need to consider direction).
- Requires more careful algorithm design (e.g., topological sort only for DAGs).

### Python Code Snippet (Adjacency List)
```python
class DirectedGraph:
    def __init__(self):
        self.graph = {}

    def add_edge(self, u, v):
        self.graph.setdefault(u, []).append(v)

    def display(self):
        for node, neighbors in self.graph.items():
            print(f"{node} → {neighbors}")

# Example
dg = DirectedGraph()
dg.add_edge('A', 'B')
dg.add_edge('A', 'C')
dg.add_edge('B', 'D')
dg.add_edge('D', 'C')
dg.display()
```

---

## 3. Weighted Graph

### Definition
A graph where each edge carries a numerical value (weight) representing cost, distance, capacity, or any other quantitative measure.

### Key Aspects
- Weights can be stored in adjacency lists (as tuples) or in a matrix.
- Used in shortest‑path, minimum spanning tree, and network flow algorithms.

### Diagram
```
A --2-- B
|       |
4       1
|       |
C --3-- D
```
Weights: AB=2, AD=4, BD=1, CD=3

### Use Cases
- GPS navigation (road distances or travel times).
- Network routing (latency or bandwidth).
- Optimization problems (e.g., traveling salesman).

### Advantages
- Adds quantitative information to connections.
- Enables optimization beyond connectivity.

### Disadvantages
- Increased memory overhead.
- Algorithm complexity often increases (e.g., Dijkstra vs. BFS).

### Python Code Snippet (Weighted Adjacency List)
```python
class WeightedGraph:
    def __init__(self):
        self.graph = {}

    def add_edge(self, u, v, weight):
        self.graph.setdefault(u, []).append((v, weight))
        # For undirected weighted, also add (u, weight) to v

    def display(self):
        for node, edges in self.graph.items():
            print(f"{node}: {edges}")

# Example
wg = WeightedGraph()
wg.add_edge('A', 'B', 2)
wg.add_edge('A', 'C', 4)
wg.add_edge('B', 'D', 1)
wg.add_edge('C', 'D', 3)
wg.display()
```

---

## 4. Unweighted Graph

### Definition
A graph where edges have no associated weights; they only indicate presence or absence of a connection.

### Key Aspects
- All edges are considered equal.
- Simplest graph representation.
- Algorithms typically rely on BFS/DFS for shortest paths (unweighted).

### Diagram
Same as the undirected or directed examples without numbers.

### Use Cases
- Social networks (just friendship, not strength).
- Simple connectivity checks.
- Path existence problems.

### Advantages
- Lower memory usage.
- Simpler, faster algorithms (BFS/DFS for shortest path).

### Disadvantages
- Cannot capture quantitative differences between connections.
- May oversimplify real‑world relationships.

### Python Code Snippet (Same as Undirected/Directed but without weights)
```python
# See UndirectedGraph or DirectedGraph examples above.
# They are inherently unweighted.
```

---

## 5. Cyclic Graph

### Definition
A graph that contains at least one cycle – a closed path where the start and end vertices are the same, and no vertex is repeated except the start/end.

### Key Aspects
- In undirected graphs: any closed loop forms a cycle.
- In directed graphs: a directed cycle requires edges to follow direction.
- Detecting cycles is essential for many algorithms (e.g., deadlock detection, scheduling).

### Diagram
```
A — B
|   |
D — C
```
Cycle: A–B–C–D–A

### Use Cases
- Detecting deadlocks in operating systems.
- Finding loops in program flow graphs.
- Analyzing feedback loops in systems.

### Advantages
- Represents feedback or recurrence in systems.
- Enables algorithms like cycle detection for optimization.

### Disadvantages
- Can complicate traversal (infinite loops if not handled).
- May indicate undesirable dependencies (e.g., circular dependencies in build systems).

### Python Code Snippet (Cycle Detection in Undirected Graph)
```python
def has_cycle_undirected(graph, n):
    visited = [False] * n
    parent = [-1] * n

    def dfs(v, p):
        visited[v] = True
        for neighbor in graph[v]:
            if not visited[neighbor]:
                if dfs(neighbor, v):
                    return True
            elif neighbor != p:
                return True
        return False

    for v in range(n):
        if not visited[v]:
            if dfs(v, -1):
                return True
    return False

# Example: graph with 4 vertices (0-1-2-3-0)
g = {0: [1,3], 1: [0,2], 2: [1,3], 3: [2,0]}
print(has_cycle_undirected(g, 4))  # True
```

---

## 6. Acyclic Graph

### Definition
A graph with no cycles. In directed graphs, a *Directed Acyclic Graph (DAG)* is fundamental.

### Key Aspects
- Undirected acyclic graphs are *forests* (a tree if connected).
- DAGs support topological ordering.
- Widely used to represent dependencies.

### Diagram (DAG)
```
A → B → D
↓
C
```
Edges: A→B, A→C, B→D (no cycles)

### Use Cases
- Task scheduling (prerequisites).
- Version control systems (commit DAG).
- Data processing pipelines (Apache Airflow).

### Advantages
- Guarantees topological order.
- Simplifies dynamic programming (longest path in DAG is solvable in linear time).

### Disadvantages
- Cannot represent feedback loops.
- May require careful design to avoid cycles.

### Python Code Snippet (Topological Sort of a DAG)
```python
from collections import deque

def topological_sort(graph, n):
    indeg = [0] * n
    for u in range(n):
        for v in graph[u]:
            indeg[v] += 1
    q = deque([i for i in range(n) if indeg[i] == 0])
    order = []
    while q:
        u = q.popleft()
        order.append(u)
        for v in graph[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)
    return order if len(order) == n else None  # None if cycle

# DAG: 0→1, 0→2, 1→3
dag = {0: [1,2], 1: [3], 2: [], 3: []}
print(topological_sort(dag, 4))  # [0, 1, 2, 3] or similar
```

---

## 7. Complete Graph

### Definition
A graph where every pair of distinct vertices is directly connected by a unique edge. Denoted \(K_n\) for \(n\) vertices.

### Key Aspects
- Number of edges: \(\frac{n(n-1)}{2}\) for undirected; \(n(n-1)\) for directed (if loops not allowed).
- Maximum density.
- Often used in theoretical proofs and worst‑case analysis.

### Diagram (K₄)
```
A — B
| \ |
|  \|
C — D
```
All 6 edges present.

### Use Cases
- Benchmarking graph algorithms.
- Clustering where all points are mutually similar.
- Network design where full connectivity is assumed.

### Advantages
- Simplifies analysis (every vertex is equivalent).
- Guarantees connectivity and high redundancy.

### Disadvantages
- Impractical for large \(n\) (quadratic edges).
- High memory consumption.

### Python Code Snippet (Check if a graph is complete)
```python
def is_complete(graph, n):
    # graph is adjacency list, n is number of vertices
    for u in range(n):
        if len(graph[u]) != n - 1:
            return False
        # Also ensure no duplicate neighbors (simplified)
    return True

# Example K₃
complete = {0: [1,2], 1: [0,2], 2: [0,1]}
print(is_complete(complete, 3))  # True
```

---

## 8. Dense Graph

### Definition
A graph where the number of edges is close to the maximum possible. There is no strict threshold, but often \(|E| = \Theta(|V|^2)\).

### Key Aspects
- Algorithms that iterate over all edges become expensive.
- Often represented with adjacency matrices for efficiency.
- Contrasts with sparse graphs.

### Use Cases
- Social networks with high clustering.
- Fully connected neural networks.
- Graph databases when relationships are abundant.

### Advantages
- Rich connectivity enables robust analysis.
- Some algorithms (e.g., Floyd–Warshall) are suited for dense graphs.

### Disadvantages
- High memory usage.
- Many algorithms run in \(O(|V|^2)\) or worse, which can be prohibitive.

### Python Code Snippet (Check density threshold)
```python
def is_dense(graph, n, threshold=0.5):
    max_edges = n * (n - 1)  # for directed, or n*(n-1)//2 for undirected
    actual_edges = sum(len(neighbors) for neighbors in graph.values())
    density = actual_edges / max_edges
    return density >= threshold

# Example: dense graph with 4 vertices, 10 directed edges (max 12)
dense_graph = {0: [1,2,3], 1: [0,2,3], 2: [0,1,3], 3: [0,1,2]}
print(is_dense(dense_graph, 4))  # True
```

---

## 9. Sparse Graph

### Definition
A graph with relatively few edges, typically \(|E| = O(|V|)\) or \(O(|V|\log|V|)\).

### Key Aspects
- Adjacency lists are more memory‑efficient than matrices.
- Most real‑world networks (social, web, biological) are sparse.
- Algorithms often aim for near‑linear time in \(|V|+|E|\).

### Use Cases
- Web graph (pages as vertices, hyperlinks as edges).
- Road networks (each intersection has few outgoing roads).
- Sparse matrix representations.

### Advantages
- Lower memory footprint.
- Faster graph traversals (BFS/DFS are \(O(|V|+|E|)\)).

### Disadvantages
- May not capture all relationships if data is actually dense.
- Some algorithms need adaptations to handle sparsity efficiently.

### Python Code Snippet (Check sparsity)
```python
def is_sparse(graph, n, threshold=0.1):
    max_edges = n * (n - 1)  # for directed
    actual_edges = sum(len(neighbors) for neighbors in graph.values())
    density = actual_edges / max_edges
    return density <= threshold

# Example: sparse graph (a path)
sparse = {0: [1], 1: [0,2], 2: [1,3], 3: [2]}
print(is_sparse(sparse, 4))  # True
```

---

## 10. Labelled Graph

### Definition
A graph where vertices, edges, or both carry labels (identifiers, names, colors, or other data). In most algorithmic contexts, “labeled” simply means vertices have unique identifiers.

### Key Aspects
- Labels distinguish vertices beyond their position.
- In graph isomorphism, unlabeled graphs are studied; real data is almost always labeled.
- Edge labels can represent types (e.g., “friend”, “colleague”).

### Diagram
```
A(John) --- B(Mary)
  (friend)     (sister)
     |           |
  C(Peter) --- D(Anna)
```
Labels on vertices (names) and edges (relationship types).

### Use Cases
- Knowledge graphs (entities with typed relationships).
- Social network analysis (users have profiles, edges have interaction types).
- Semantic networks.

### Advantages
- Captures rich semantics.
- Enables queries like “find all friends of John”.

### Disadvantages
- Increases complexity of storage and querying.
- May require specialized indexing.

### Python Code Snippet (Simple labelled graph)
```python
class LabelledGraph:
    def __init__(self):
        self.vertices = {}   # vertex_id -> label
        self.edges = {}      # (u, v) -> edge_label

    def add_vertex(self, vid, label):
        self.vertices[vid] = label

    def add_edge(self, u, v, edge_label):
        self.edges[(u, v)] = edge_label

    def display(self):
        print("Vertices:")
        for vid, label in self.vertices.items():
            print(f"  {vid}: {label}")
        print("Edges:")
        for (u, v), label in self.edges.items():
            print(f"  {u} -({label})-> {v}")

lg = LabelledGraph()
lg.add_vertex('A', 'John')
lg.add_vertex('B', 'Mary')
lg.add_edge('A', 'B', 'friend')
lg.display()
```

---

## 11. Connected Graph

### Definition
- **Undirected**: There is a path between every pair of vertices. If not connected, it consists of *connected components*.
- **Directed**: *Weakly connected* if the underlying undirected graph is connected; *strongly connected* if for every pair \(u, v\) there is a directed path from \(u\) to \(v\) and from \(v\) to \(u\).

### Key Aspects
- Connectivity is fundamental for many graph properties.
- Algorithms like DFS can find components.

### Diagram (Connected Undirected)
```
A — B
|   |
C — D
```
All vertices reachable from each other.

### Diagram (Strongly Connected Directed)
```
A → B
↑   ↓
D ← C
```
Every vertex can reach every other following direction.

### Use Cases
- Network reliability (if graph is connected, failure of one node may not isolate others).
- Social network analysis (connected components represent communities).
- Routing protocols.

### Advantages
- Ensures global reachability.
- Simplifies many algorithms (e.g., BFS/DFS from one source covers all).

### Disadvantages
- May not reflect real disconnected structures.
- Adding connectivity constraints can be restrictive.

### Python Code Snippet (Check connectivity for undirected)
```python
def is_connected(graph, n):
    visited = [False] * n

    def dfs(v):
        visited[v] = True
        for neighbor in graph[v]:
            if not visited[neighbor]:
                dfs(neighbor)

    dfs(0)  # start from vertex 0
    return all(visited)

# Connected example
connected = {0: [1], 1: [0,2], 2: [1]}
print(is_connected(connected, 3))  # True

# Disconnected
disconnected = {0: [1], 1: [0], 2: []}
print(is_connected(disconnected, 3))  # False
```

---

## Summary Table

| Property          | Key Feature                              | Python Snippet Focus                     |
|-------------------|------------------------------------------|------------------------------------------|
| Undirected        | Bidirectional edges                       | Adding edges both ways                   |
| Directed          | One‑way edges                             | Adjacency list with direction            |
| Weighted          | Edges have numerical values               | Store (neighbor, weight)                 |
| Unweighted        | No weights                                | Simple adjacency list                    |
| Cyclic            | Contains at least one cycle               | Cycle detection with DFS                 |
| Acyclic           | No cycles (DAG)                           | Topological sort                         |
| Complete          | Every vertex pair connected               | Check neighbor count                     |
| Dense             | Edges near \(n^2\)                        | Compute density                          |
| Sparse            | Edges \(O(n)\) or \(O(n \log n)\)         | Compute density                          |
| Labelled          | Vertices/edges have labels                | Separate dictionaries for labels         |
| Connected         | Single component (undirected)             | DFS to mark visited                      |

Each property can be combined with others. For example, a **weighted directed acyclic graph (DAG)** is common in scheduling, while a **sparse labelled undirected graph** often represents a social network.

