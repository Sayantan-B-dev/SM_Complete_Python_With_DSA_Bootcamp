## 1. Node / Vertex

### Definition
A fundamental unit of a graph. Nodes represent entities such as people, cities, web pages, or states.

### Key Aspects
- Often labeled (e.g., A, B, 1, 2) to distinguish them.
- In diagrams, nodes are drawn as circles or dots.
- The set of vertices is usually denoted \(V\) (or \(V(G)\) for graph \(G\)).

### Diagram
```
(A)   (B)   (C)
```
Three vertices: A, B, C.

### Python Snippet
In code, vertices are typically stored as keys in an adjacency list or indices in a matrix.

```python
# Representing vertices with IDs 0,1,2
graph = {
    0: [],  # vertex 0
    1: [],  # vertex 1
    2: []   # vertex 2
}
```

---

## 2. Edge

### Definition
A connection between two vertices. Edges represent relationships, links, or interactions.

### Key Aspects
- Can be **undirected** (no direction) or **directed** (one-way).
- Can carry additional data (weight, label).
- The set of edges is denoted \(E\).

### Diagram (Undirected Edge)
```
A — B
```
Edge between A and B.

### Diagram (Directed Edge)
```
A → B
```
Edge from A to B.

### Python Snippet
```python
# Undirected edge: add both directions
graph.setdefault('A', []).append('B')
graph.setdefault('B', []).append('A')

# Directed edge: add only one direction
graph.setdefault('A', []).append('B')
```

---

## 3. Directed, Undirected, Bidirected

### Directed Edge
An ordered pair \((u, v)\) representing a one‑way relationship from \(u\) to \(v\).

### Undirected Edge
An unordered pair \(\{u, v\}\) representing a mutual connection.

### Bidirected Edge
Sometimes used in specialized contexts (e.g., in graph theory, a “bidirected edge” can mean two directed edges in opposite directions between the same vertices). In practice, a graph with such a pair is simply a directed graph where both directions exist.

### Diagram
```
Undirected:    A — B
Directed:      A → B
Bidirected:    A ⇄ B   (two opposite directed edges)
```

### Python Snippet
```python
# Undirected
graph_undirected['A'].append('B')
graph_undirected['B'].append('A')

# Directed
graph_directed['A'].append('B')   # only A→B

# Bidirected (two directed edges)
graph_bidirected['A'].append('B')
graph_bidirected['B'].append('A')
```

---

## 4. Adjacent Vertex (Neighbor)

### Definition
Two vertices are **adjacent** if there is an edge directly connecting them. Each is a neighbor of the other.

### Key Aspects
- In undirected graphs, adjacency is symmetric.
- In directed graphs, adjacency depends on direction: \(v\) is adjacent from \(u\) if \((u, v)\) exists, and \(v\) is adjacent to \(u\) if \((v, u)\) exists.

### Diagram
```
A — B   (A and B are adjacent)
C → D   (C and D are adjacent from C to D; D is not adjacent from D to C unless the opposite edge exists)
```

### Python Snippet
```python
def are_adjacent(graph, u, v):
    return v in graph.get(u, [])

# Example
graph = {'A': ['B'], 'B': ['A']}
print(are_adjacent(graph, 'A', 'B'))  # True
print(are_adjacent(graph, 'A', 'C'))  # False
```

---

## 5. Degree (Indegree vs Outdegree)

### Degree (Undirected Graph)
The number of edges incident to a vertex. For an undirected graph, \(\deg(v) = |\{u \mid \{v,u\} \in E\}|\).

### Indegree (Directed Graph)
The number of incoming edges to a vertex: \(\text{indeg}(v) = |\{u \mid (u, v) \in E\}|\).

### Outdegree (Directed Graph)
The number of outgoing edges from a vertex: \(\text{outdeg}(v) = |\{u \mid (v, u) \in E\}|\).

### Diagram
```
Undirected:
A — B
|   |
C — D
deg(A)=2, deg(B)=2, deg(C)=2, deg(D)=2

Directed:
A → B
↑   ↓
C ← D
indeg(A)=1, outdeg(A)=1
indeg(B)=1, outdeg(B)=1
indeg(C)=2, outdeg(C)=0
indeg(D)=0, outdeg(D)=2
```

### Python Snippet
```python
# Undirected degree
def degree_undirected(graph, v):
    return len(graph.get(v, []))

# Directed indegree/outdegree
def outdegree(graph, v):
    return len(graph.get(v, []))

def indegree(graph, v):
    count = 0
    for u, neighbors in graph.items():
        if v in neighbors:
            count += 1
    return count

# Example directed graph
dg = {'A': ['B'], 'B': ['D'], 'C': [], 'D': ['C']}
print(outdegree(dg, 'A'))  # 1
print(indegree(dg, 'A'))   # 0
print(indegree(dg, 'C'))   # 1 (from D)
```

---

## 6. Path

### Definition
A **path** of length \(n\) from vertex \(u_1\) to vertex \(u_{n+1}\) is a sequence of vertices \(u_1, u_2, \ldots, u_{n+1}\) such that each consecutive pair \((u_i, u_{i+1})\) is an edge in the graph. No vertex is repeated (simple path) unless explicitly allowed.

### Open Path vs Closed Path
- **Open Path**: The start and end vertices are different (\(u_1 \neq u_{n+1}\)).
- **Closed Path**: The start and end vertices are the same (\(u_1 = u_{n+1}\)). A closed path with no repeated vertices (except the start/end) is a **cycle**.

### Diagram
```
Open path A→B→D:
A — B — C
    |
    D
(Path: A, B, D)

Closed path (cycle) A→B→C→A:
A — B
|   |
C — D   (the closed path A-B-D-C-A is a cycle if no repeated vertices)
```

### Python Snippet (Check if a simple path exists using DFS)
```python
def has_path(graph, start, end, visited=None):
    if visited is None:
        visited = set()
    if start == end:
        return True
    visited.add(start)
    for neighbor in graph.get(start, []):
        if neighbor not in visited:
            if has_path(graph, neighbor, end, visited):
                return True
    return False

# Example
g = {'A': ['B', 'C'], 'B': ['D'], 'C': ['D'], 'D': []}
print(has_path(g, 'A', 'D'))  # True
print(has_path(g, 'B', 'C'))  # False
```

---

## 7. Cycle

### Definition
A **cycle** is a closed path (start = end) where no vertex is repeated except the start/end, and the path has length at least 3 (for undirected) or at least 1 (for directed, though often at least 2). In simple terms, it is a loop that returns to the starting vertex without reusing vertices.

### Key Aspects
- **Undirected cycle**: Requires at least 3 distinct vertices (e.g., triangle).
- **Directed cycle**: Requires at least one directed path that returns to the start (can be a self‑loop if allowed, but typically edges are between distinct vertices).
- A graph with no cycles is **acyclic**.

### Diagram
```
Undirected cycle (triangle):
A — B
|   |
C —   (but actually A-B-C-A forms a triangle)

Directed cycle:
A → B → C → A
```

### Python Snippet (Cycle Detection in Undirected Graph)
```python
def has_cycle_undirected(graph, n):
    visited = [False] * n
    parent = [-1] * n

    def dfs(v, p):
        visited[v] = True
        for nei in graph[v]:
            if not visited[nei]:
                if dfs(nei, v):
                    return True
            elif nei != p:
                return True
        return False

    for v in range(n):
        if not visited[v]:
            if dfs(v, -1):
                return True
    return False

# Graph with a cycle (0-1-2-0)
cycle_graph = {0: [1,2], 1: [0,2], 2: [1,0]}
print(has_cycle_undirected(cycle_graph, 3))  # True
```
