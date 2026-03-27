```py
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
    
    def display(self):

        for index ,label in enumerate(self.vertices):
            if label !=None:
                print(f"Vertex Index : {index}, label :{label} ")
        
        for row in self.adj_matrix:
            print(row)

    
    def dfs(self):
        dfs_result = []
        visited = [False] * self.num_vertices
        for vertex in range(self.num_vertices):
            if(visited[vertex] == False):
                print("Calling for a component")
                self.dfs_recursive(vertex,dfs_result,visited)
    
    def dfs_recursive(self,vertex,dfs_result,visited):
        print(vertex)
        dfs_result.append(vertex)
        visited[vertex] = True

        for neighbor in range(self.num_vertices):
            if(vertex == neighbor):
                continue
            
            if(self.adj_matrix[vertex][neighbor] == 1):
                if(visited[neighbor]== False):
                    self.dfs_recursive(neighbor,dfs_result,visited)
        


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

graph.dfs()



```
# Depth‑First Search (DFS) – Comprehensive Documentation

## 1. Introduction

**Depth‑First Search (DFS)** is a fundamental graph traversal algorithm that explores as far as possible along each branch before backtracking. It is widely used for:

- Detecting cycles
- Topological sorting
- Solving puzzles (mazes, Sudoku)
- Finding connected components
- Pathfinding in unweighted graphs
- Generating spanning trees

DFS can be implemented either **recursively** (using the call stack) or **iteratively** (using an explicit stack). The provided code uses recursion.

---

## 2. Algorithm Overview

### 2.1 Recursive DFS (Standard)

1. **Start from a source vertex** (or loop through all vertices to cover disconnected components).
2. **Mark the current vertex as visited**.
3. **Process the vertex** (e.g., record it in a result list, print it, etc.).
4. **For each neighbor** of the current vertex:
   - If the neighbor has not been visited, recursively call DFS on that neighbor.
5. **Backtrack** when all neighbors have been explored.

### 2.2 Iterative DFS (Stack‑Based)

1. Initialize a stack with the start vertex.
2. While the stack is not empty:
   - Pop a vertex.
   - If it is unvisited, mark it visited and push all its unvisited neighbors onto the stack (order may vary).
3. This approach mimics recursion but avoids recursion depth limits.

---

## 3. Code Implementation with Line‑by‑Line Explanation

The following code implements recursive DFS for an **undirected, unweighted graph** stored in an adjacency matrix. The graph is created with 7 vertices (indices 0..6), edges as shown in the example.

```python
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

    def display(self):
        for index, label in enumerate(self.vertices):
            if label is not None:
                print(f"Vertex Index : {index}, label : {label} ")
        for row in self.adj_matrix:
            print(row)


    # DEPTH‑FIRST SEARCH (RECURSIVE)

    def dfs(self):
        """
        Public driver method for DFS.
        It handles disconnected components by iterating over all vertices.
        """
        # List to store the order in which vertices are visited.
        dfs_result = []

        # Visited array: tracks whether a vertex has been explored.
        visited = [False] * self.num_vertices

        # Loop over every vertex; if unvisited, start a new DFS component.
        for vertex in range(self.num_vertices):
            if visited[vertex] is False:
                print("Calling for a component")  # For debugging only
                self.dfs_recursive(vertex, dfs_result, visited)

        # (Optional) Return the result list if needed.
        # return dfs_result

    def dfs_recursive(self, vertex, dfs_result, visited):
        """
        Recursive helper function that performs DFS from a given vertex.

        :param vertex: Current vertex index.
        :param dfs_result: List to accumulate visited vertices in order.
        :param visited: List of booleans marking visited vertices.
        """
        # Print current vertex (for demonstration)
        print(vertex)

        # Record the vertex in the result list and mark it visited.
        dfs_result.append(vertex)
        visited[vertex] = True

        # Examine all possible neighbors (0..num_vertices-1).
        # Since the graph is stored as an adjacency matrix,
        # we must scan the entire row to find neighbors.
        for neighbor in range(self.num_vertices):
            # Skip the vertex itself (a vertex is not a neighbor of itself).
            if vertex == neighbor:
                continue

            # Check if there is an edge (adj_matrix[vertex][neighbor] == 1).
            if self.adj_matrix[vertex][neighbor] == 1:
                # If the neighbor is unvisited, recurse into it.
                if visited[neighbor] is False:
                    self.dfs_recursive(neighbor, dfs_result, visited)
```

### 3.1 Explanation of Key Methods

#### `dfs()` – Driver Method
- **Purpose**: Initiate DFS for the entire graph, ensuring that all vertices (even those in separate components) are visited.
- **Algorithm**:
  1. Create an empty list `dfs_result` to store the traversal order.
  2. Create a `visited` array of length `num_vertices`, all `False`.
  3. Iterate through all vertex indices. For each vertex not yet visited, call `dfs_recursive` with that vertex as the starting point.
  4. This pattern is necessary because the graph might be disconnected; each call handles one connected component.

#### `dfs_recursive(vertex, dfs_result, visited)` – Recursive Helper
- **Purpose**: Perform DFS from a given vertex, using recursion to explore deep branches.
- **Algorithm**:
  1. **Mark and record**: Print the vertex (for demonstration), append it to `dfs_result`, and set `visited[vertex] = True`.
  2. **Explore neighbors**: Loop through all vertices (0..num_vertices-1) to find neighbors.  
     - Skip the vertex itself.
     - If `adj_matrix[vertex][neighbor] == 1`, an edge exists.
  3. **Recurse**: For each unvisited neighbor, call `dfs_recursive` on that neighbor. The recursion ensures depth‑first behavior – once a neighbor is explored, we go deeper before returning to the current vertex.

---

## 4. Visualization of DFS on the Example Graph

The graph created in the example has 7 vertices (0‑6) with the following edges:

```
0 — 1 — 2 — 3 — 4
          \     /
           ———  (2–4 edge)
5 — 6
```

**Graph structure (ASCII):**

```
0 --- 1 --- 2 --- 3 --- 4
              \       /
               \_____/
   
5 --- 6
```

**DFS traversal starting from vertex 0** (the first component) will go as deep as possible along each branch. One possible order (depending on neighbor scanning order – here we scan from 0 to 6) is:

- Start at 0 → visited 0.
- Neighbors of 0: only 1 (since 0–1 edge). Go to 1.
- At 1: neighbors are 0 (visited) and 2. Go to 2.
- At 2: neighbors are 1, 3, 4. Choose 3 first (since scanning from 0 upwards).
  - At 3: neighbors are 2 and 4. 2 visited, 4 unvisited → go to 4.
  - At 4: neighbors are 2 and 3 (both visited). Backtrack to 3.
  - Backtrack to 2; next neighbor is 4 (already visited). Backtrack to 1.
  - Backtrack to 0 → finished component.
- Next component: vertex 5 (unvisited).
  - At 5: neighbor 6 → go to 6.
  - At 6: neighbor 5 (visited) → backtrack.
- Done.

**Result order (dfs_result):** [0, 1, 2, 3, 4, 5, 6]  
(But note: the print statements will show `0,1,2,3,4,5,6` in that order.)

**ASCII illustration of recursion stack:**

```
dfs_recursive(0)        # start
    visited[0]=True, dfs_result=[0]
    neighbor loop: found 1 (unvisited)
    dfs_recursive(1)
        visited[1]=True, dfs_result=[0,1]
        neighbor loop: found 0 (visited), found 2 (unvisited)
        dfs_recursive(2)
            visited[2]=True, dfs_result=[0,1,2]
            neighbor loop: found 1 (visited), found 3 (unvisited), found 4 (unvisited)
            (scan order: 3 first)
            dfs_recursive(3)
                visited[3]=True, dfs_result=[0,1,2,3]
                neighbor loop: found 2 (visited), found 4 (unvisited)
                dfs_recursive(4)
                    visited[4]=True, dfs_result=[0,1,2,3,4]
                    neighbor loop: found 2 (visited), found 3 (visited)
                    return
                return
            (back to 2) next neighbor: 4 already visited, continue
            return
        return
    return
# Component 1 finished, next vertex 5 unvisited
dfs_recursive(5)
    visited[5]=True, dfs_result=[0,1,2,3,4,5]
    neighbor loop: found 6 (unvisited)
    dfs_recursive(6)
        visited[6]=True, dfs_result=[0,1,2,3,4,5,6]
        neighbor loop: found 5 (visited)
        return
    return
```

---

## 5. Detailed Walkthrough with Dummy Data

Let's trace the execution step by step using the example graph. We'll annotate each relevant step with the current state of `visited` and `dfs_result`.

### Initial State
- `visited = [False, False, False, False, False, False, False]`
- `dfs_result = []`

### Component 1 (starting at vertex 0)

1. **dfs()** loop: `vertex = 0` (unvisited). Print "Calling for a component".  
   Call `dfs_recursive(0, dfs_result, visited)`.

2. **Inside dfs_recursive(0)**:
   - Print `0`.
   - `dfs_result.append(0)` → `[0]`
   - `visited[0] = True` → `[T, F, F, F, F, F, F]`
   - **Neighbor loop** (neighbor from 0 to 6):
     - neighbor 0: skip (vertex==neighbor)
     - neighbor 1: `adj_matrix[0][1] == 1` and `visited[1] == False` → call `dfs_recursive(1)`.
     - neighbors 2‑6: no edges.

3. **dfs_recursive(1)**:
   - Print `1`.
   - `dfs_result` → `[0, 1]`
   - `visited[1] = True` → `[T, T, F, F, F, F, F]`
   - Neighbors:
     - neighbor 0: visited → skip.
     - neighbor 2: `adj_matrix[1][2] == 1` and `visited[2] == False` → call `dfs_recursive(2)`.
     - Others: none.

4. **dfs_recursive(2)**:
   - Print `2`.
   - `dfs_result` → `[0, 1, 2]`
   - `visited[2] = True` → `[T, T, T, F, F, F, F]`
   - Neighbors:
     - neighbor 1: visited.
     - neighbor 3: edge exists and `visited[3] == False` → call `dfs_recursive(3)`.
     - neighbor 4: edge exists but will be considered after returning from 3 (order matters).
     - Others: none.

5. **dfs_recursive(3)**:
   - Print `3`.
   - `dfs_result` → `[0, 1, 2, 3]`
   - `visited[3] = True` → `[T, T, T, T, F, F, F]`
   - Neighbors:
     - neighbor 2: visited.
     - neighbor 4: edge exists and `visited[4] == False` → call `dfs_recursive(4)`.
     - Others: none.

6. **dfs_recursive(4)**:
   - Print `4`.
   - `dfs_result` → `[0, 1, 2, 3, 4]`
   - `visited[4] = True` → `[T, T, T, T, T, F, F]`
   - Neighbors:
     - neighbor 2: visited.
     - neighbor 3: visited.
     - No unvisited neighbors. Return to caller (3).

7. Back to **dfs_recursive(3)**:
   - No more unvisited neighbors. Return to caller (2).

8. Back to **dfs_recursive(2)**:
   - Continue neighbor loop: next neighbor = 4, but `visited[4]` is now `True`, so skip.
   - No more neighbors. Return to caller (1).

9. Back to **dfs_recursive(1)**:
   - Continue neighbor loop: next neighbor = 3 (already visited), neighbor = 4 (already visited) – all visited.
   - Return to caller (0).

10. Back to **dfs_recursive(0)**:
    - Continue neighbor loop: neighbors 2..6: none have edges with 0. Return to dfs().

### Component 2 (starting at vertex 5)

11. **dfs()** loop continues: `vertex = 1` (visited), `2` (visited), `3` (visited), `4` (visited), `5` (unvisited).  
    Print "Calling for a component".  
    Call `dfs_recursive(5, dfs_result, visited)`.

12. **dfs_recursive(5)**:
    - Print `5`.
    - `dfs_result` → `[0, 1, 2, 3, 4, 5]`
    - `visited[5] = True` → `[T, T, T, T, T, T, F]`
    - Neighbors:
      - neighbor 6: edge exists and `visited[6] == False` → call `dfs_recursive(6)`.

13. **dfs_recursive(6)**:
    - Print `6`.
    - `dfs_result` → `[0, 1, 2, 3, 4, 5, 6]`
    - `visited[6] = True` → `[T, T, T, T, T, T, T]`
    - Neighbors:
      - neighbor 5: visited.
      - No other neighbors. Return.

14. Back to **dfs_recursive(5)**: no more unvisited neighbors. Return to dfs().

15. **dfs()** loop continues: `vertex = 6` (visited). Loop ends.

**Final dfs_result** = `[0, 1, 2, 3, 4, 5, 6]`

---

## 6. Important Concepts

### Visited Array
- **Purpose**: Prevents infinite loops and revisiting vertices.
- **Initialization**: All `False` initially.
- **Marking**: A vertex is marked as soon as it is **first discovered** (before exploring its neighbors). This ensures we never re‑enter it.

### Recursion vs. Iteration
- **Recursive** DFS uses the system call stack to remember the path. It is concise but may cause stack overflow on very deep graphs.
- **Iterative** DFS uses an explicit stack (e.g., `list` in Python) to mimic recursion, giving more control over stack size.

### Handling Disconnected Graphs
- The driver loop in `dfs()` ensures that even if the graph has multiple connected components, every vertex is eventually visited.

### Neighbor Scanning in Adjacency Matrix
- Since the graph is stored as an adjacency matrix, finding neighbors requires scanning the entire row (`O(V)` time per vertex). This is inefficient for sparse graphs but acceptable for dense ones.

---

## 7. Complexity Analysis

- **Time Complexity**: `O(V²)` because for each vertex we scan all `V` possible neighbors to find edges. If the graph were stored in an adjacency list, the time would be `O(V + E)`.
- **Space Complexity**: `O(V)` for the `visited` array and recursion stack (worst case recursion depth `V`).

---

## 8. Recursive vs. Iterative DFS

| Aspect | Recursive DFS | Iterative DFS (Stack) |
|--------|---------------|------------------------|
| **Implementation** | Simple, natural for tree structures | Explicit stack, more verbose |
| **Memory** | Uses call stack; limited by recursion depth | Uses heap‑allocated stack; can handle deeper graphs |
| **Order** | Typically pre‑order (process before children) | Can be pre‑order or post‑order depending on push order |
| **Common Uses** | Graph theory exercises, small to medium graphs | Production code, very deep graphs |

**Iterative Implementation Example** (for reference):

```python
def dfs_iterative(self, start):
    visited = [False] * self.num_vertices
    stack = [start]
    while stack:
        vertex = stack.pop()
        if not visited[vertex]:
            visited[vertex] = True
            print(vertex)   # process
            # Push neighbors in reverse order to simulate recursion order
            for neighbor in range(self.num_vertices - 1, -1, -1):
                if self.adj_matrix[vertex][neighbor] == 1 and not visited[neighbor]:
                    stack.append(neighbor)
```

---

## 9. Summary

DFS is a versatile graph traversal technique that explores depth‑first. The recursive implementation presented here is clear and easy to understand, though it relies on the system stack. The key components are:

- A **visited array** to track explored vertices.
- A **driver loop** to handle disconnected components.
- **Recursive exploration** of unvisited neighbors.

The code can be adapted to different graph representations (e.g., adjacency list) and modified for specific tasks like finding paths, detecting cycles, or generating topological orders.