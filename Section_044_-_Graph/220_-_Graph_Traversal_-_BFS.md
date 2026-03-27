```py
from collections import deque

class GraphAdjacencyMatrix:
    def __init__(self,num_vertices):
        self.num_vertices = num_vertices
        self.vertices = [None] * num_vertices
        self.adj_matrix = [ [0] * num_vertices for row in range(num_vertices)]
    
    def add_vertex(self,index,label):
        if( index >=0 and index <self.num_vertices):
            self.vertices[index] = label
        else:
            print("Index OOB")

    def add_edge(self,source,destination,weight=1):
        if 0 <= source < self.num_vertices and 0<= destination <self.num_vertices:
            self.adj_matrix[source][destination] = weight
            self.adj_matrix[destination][source] = weight # Undirected Graph

    def bfs(self,start_vertex = 0):
        visited = [False] * self.num_vertices
        queue = deque([start_vertex])

        bfs_result = []
        visited[start_vertex] = True

        while len(queue) !=0:
            vertex = queue.popleft() 
            print(vertex)
            for neighbour in range(self.num_vertices):

                if(self.adj_matrix[vertex][neighbour]!=0):
                    if(visited[neighbour]== False):
                        queue.append(neighbour)
                        visited[neighbour] = True
    




graph = GraphAdjacencyMatrix(7)
graph.add_vertex(0,'A')
graph.add_vertex(1,'B')
graph.add_vertex(2,'C')
graph.add_vertex(3,'D')
graph.add_vertex(4,'E')
graph.add_vertex(5,'F')
graph.add_vertex(6,'F')

graph.add_edge(0,1)
graph.add_edge(1,2)
graph.add_edge(2,3)
graph.add_edge(3,4)
graph.add_edge(2,4)
# graph.add_edge(0,5)
graph.add_edge(5,6)



graph.bfs()


```
# Breadth-First Search (BFS) – Comprehensive Documentation

## 1. Introduction

**Breadth-First Search (BFS)** is a graph traversal algorithm that explores vertices in order of increasing distance from the source. It visits all neighbors of a vertex before moving to their neighbors, effectively expanding a wavefront level by level. BFS is widely used for:

- Finding the shortest path in unweighted graphs
- Checking bipartiteness
- Finding connected components
- Solving puzzles with minimal steps
- Web crawling and social network analysis

---

## 2. Algorithm Overview

The algorithm uses a **queue** (FIFO) to manage the frontier. A `visited` array prevents revisiting vertices.

### 2.1 Pseudocode

```
BFS(graph, start):
    visited = array of size |V|, all false
    queue = empty queue
    visited[start] = true
    queue.enqueue(start)

    while queue is not empty:
        vertex = queue.dequeue()
        process(vertex)   // e.g., print, add to result

        for each neighbor of vertex:
            if not visited[neighbor]:
                visited[neighbor] = true
                queue.enqueue(neighbor)
```

### 2.2 Key Principles

- **Queue** ensures that vertices are processed in the order they are discovered (FIFO).
- **Visited marking** occurs **when a vertex is enqueued**, not when it is dequeued. This prevents multiple enqueues of the same vertex.
- **Distance** from the start can be tracked by storing distance alongside the vertex or using a separate array.
- The algorithm naturally handles **connected components** if we iterate over all unvisited vertices and restart BFS for each.

---

## 3. Code Implementation with Line‑by‑Line Explanation

The following code implements BFS for an undirected graph stored in an adjacency matrix. It processes all vertices reachable from a given start.

```python
from collections import deque

class GraphAdjacencyMatrix:
    def __init__(self, num_vertices):
        self.num_vertices = num_vertices
        self.vertices = [None] * num_vertices
        self.adj_matrix = [[0] * num_vertices for _ in range(num_vertices)]

    def add_vertex(self, index, label):
        if 0 <= index < self.num_vertices:
            self.vertices[index] = label
        else:
            print("Index OOB")

    def add_edge(self, source, destination, weight=1):
        if 0 <= source < self.num_vertices and 0 <= destination < self.num_vertices:
            self.adj_matrix[source][destination] = weight
            self.adj_matrix[destination][source] = weight   # Undirected Graph
        else:
            print("One or both vertices not found.")


    # BREADTH‑FIRST SEARCH

    def bfs(self, start_vertex=0):
        """
        Performs BFS starting from the given vertex.
        Assumes the graph may be disconnected; only processes the component
        containing start_vertex. To cover all components, a driver loop is needed.
        """
        # visited array: False initially. Marks vertices that have been added to the queue.
        visited = [False] * self.num_vertices

        # Initialize queue with the start vertex using collections.deque for O(1) popleft.
        queue = deque([start_vertex])

        # List to store the order of visitation (optional).
        bfs_result = []

        # Mark the start vertex as visited immediately to avoid re‑enqueueing.
        visited[start_vertex] = True

        # Main loop: process while the queue is not empty.
        while len(queue) != 0:
            # Remove the vertex from the front of the queue.
            vertex = queue.popleft()

            # Process the current vertex (here we just print it).
            print(vertex)

            # (Optional) Append to result list.
            bfs_result.append(vertex)

            # Examine all possible neighbors (0 .. num_vertices-1).
            # Since the graph is stored in an adjacency matrix, we scan the entire row.
            for neighbor in range(self.num_vertices):
                # Check if there is an edge (non‑zero weight) between vertex and neighbor.
                if self.adj_matrix[vertex][neighbor] != 0:
                    # If the neighbor has not yet been visited, mark it and enqueue it.
                    if not visited[neighbor]:
                        visited[neighbor] = True   # Mark now, before enqueue.
                        queue.append(neighbor)
        # (Optional) Return bfs_result if needed.
```

---

## 4. Explanation of Key Components

### 4.1 Visited Array
- **Purpose**: Prevents cycles and infinite loops by ensuring each vertex is processed at most once.
- **Marking Timing**: Mark a vertex **as soon as it is enqueued**, not when it is dequeued. This guarantees that the same vertex is not added multiple times to the queue.
- **Initialization**: All `False`. The start vertex is marked `True` before the while loop.

### 4.2 Queue
- **Data Structure**: `deque` (double‑ended queue) from Python’s `collections` module provides `O(1)` appends and pops from both ends.
- **Role**: Maintains the frontier; vertices are processed in the order they were discovered (FIFO).
- **Enqueue Operation**: When an unvisited neighbor is found, it is appended to the right end of the queue.
- **Dequeue Operation**: The leftmost element is popped for processing.

### 4.3 Neighbor Scanning in Adjacency Matrix
- Since the graph is stored as an adjacency matrix, finding neighbors requires iterating over all `V` possible columns for each vertex. This results in `O(V²)` time for BFS.
- In an adjacency list representation, neighbor iteration is `O(degree(v))`, leading to `O(V + E)` overall.

### 4.4 Handling Disconnected Graphs
- The provided `bfs` method only explores the component containing the `start_vertex`. To visit all vertices in a disconnected graph, a driver loop would be needed:
  ```python
  def bfs_all(self):
      visited = [False] * self.num_vertices
      for v in range(self.num_vertices):
          if not visited[v]:
              self.bfs_component(v, visited)
  ```
- The driver loop ensures that every vertex is eventually visited, regardless of component membership.

---

## 5. Step‑by‑Step Walkthrough with Dummy Data

The example graph (as in the code) has 7 vertices (0‑6) with edges:

```
0 — 1 — 2 — 3 — 4
          \     /
           ———  (2–4 edge)
5 — 6
```

**Graph Diagram (ASCII)**:

```
0 --- 1 --- 2 --- 3 --- 4
              \       /
               \_____/

5 --- 6
```

**BFS starts at vertex 0**. The following table traces the queue, visited array, and output.

| Step | Queue (front → back) | Dequeued | Visited (0‑6) | Output | Action |
|------|-----------------------|----------|----------------|--------|--------|
| 0 | `[0]` | – | `[T, F, F, F, F, F, F]` | – | Initialize: enqueue start, mark visited. |
| 1 | `[0]` | `0` | `[T, F, F, F, F, F, F]` | `0` | Dequeue 0; process. Examine neighbors: 1 (edge, unvisited) → mark and enqueue. |
| 2 | `[1]` | – | `[T, T, F, F, F, F, F]` | – | After step 1. |
| 3 | `[1]` | `1` | `[T, T, F, F, F, F, F]` | `1` | Dequeue 1; process. Neighbors: 0 (visited), 2 (edge, unvisited) → enqueue 2. |
| 4 | `[2]` | – | `[T, T, T, F, F, F, F]` | – | After step 3. |
| 5 | `[2]` | `2` | `[T, T, T, F, F, F, F]` | `2` | Dequeue 2; process. Neighbors: 1 (visited), 3 (edge, unvisited) → enqueue 3; 4 (edge, unvisited) → enqueue 4. |
| 6 | `[3, 4]` | – | `[T, T, T, T, T, F, F]` | – | After step 5. |
| 7 | `[3, 4]` | `3` | `[T, T, T, T, T, F, F]` | `3` | Dequeue 3; process. Neighbors: 2 (visited), 4 (already visited – skip). |
| 8 | `[4]` | – | (unchanged) | – | After step 7. |
| 9 | `[4]` | `4` | `[T, T, T, T, T, F, F]` | `4` | Dequeue 4; process. Neighbors: 2 (visited), 3 (visited). |
| 10 | `[]` | – | – | – | Queue empty; BFS ends. Vertices 5 and 6 remain unvisited because they are in a separate component not reachable from 0. |

**Output order**: `0, 1, 2, 3, 4`

### 5.1 Why Vertex 5 and 6 Are Not Visited
- The BFS only explores the component containing the start vertex (0). Vertex 5 and 6 form a separate component. To visit them, a driver loop would be required (or a BFS call starting at 5).

---

## 6. Visualizing the BFS Tree

BFS constructs a shortest‑path tree from the start vertex.

**BFS tree for the example (root = 0):**

```
        0
        |
        1
        |
        2
       / \
      3   4
```

**Levels (distance from 0):**
- Level 0: {0}
- Level 1: {1}
- Level 2: {2}
- Level 3: {3, 4}

---

## 7. Complexity Analysis

| Operation | Complexity |
|-----------|------------|
| **Time**  | \(O(V^2)\) because the adjacency matrix forces scanning all \(V\) columns for each dequeued vertex. If the graph were stored in an adjacency list, BFS would be \(O(V + E)\). |
| **Space** | \(O(V)\) for the queue and visited array. The adjacency matrix itself occupies \(O(V^2)\) space, independent of BFS. |

---

## 8. BFS vs DFS: Comparison

| Aspect | BFS | DFS (Recursive) |
|--------|-----|-----------------|
| **Data Structure** | Queue (FIFO) | Stack (implicit via recursion) |
| **Order of Exploration** | Level‑order (by distance) | Depth‑first |
| **Shortest Path** | Guaranteed in unweighted graphs | Not guaranteed (may find arbitrary path) |
| **Memory** | Queue can grow large in wide graphs | Stack depth can be large in deep graphs |
| **Use Cases** | Shortest path, web crawling, bipartiteness | Topological sort, cycle detection, solving puzzles |

---

## 9. Importance of Marking Visited **When Enqueuing**

A critical detail in BFS (and in any graph traversal) is **when** to mark a vertex as visited.

- **Correct approach**: Mark visited **immediately when the vertex is enqueued**.
- **Incorrect approach**: Mark visited only when the vertex is dequeued.

If marking is delayed until dequeue, the same vertex may be enqueued multiple times by different neighbors before it is processed, leading to:

- Redundant queue entries
- Potential infinite loops in graphs with cycles
- Exponential growth of queue size in worst‑case

**Example**: In a star graph with center `c` and many leaves, if we mark visited only at dequeue, each leaf may enqueue `c` repeatedly, causing massive duplication.

By marking when enqueuing, we guarantee each vertex enters the queue at most once.

---

## 10. Summary

Breadth‑First Search is a fundamental graph algorithm that systematically explores a graph level by level. The implementation using an adjacency matrix is straightforward but less efficient for sparse graphs than an adjacency list. Key takeaways:

- Use a **queue** to maintain the frontier.
- Mark vertices **visited at enqueue time** to avoid duplicates.
- BFS gives the shortest path in unweighted graphs.
- The algorithm can be extended to handle disconnected graphs with a driver loop.

The provided `bfs` method serves as a clear illustration of these principles, and can be easily adapted for tasks like distance computation or path reconstruction.