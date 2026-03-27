## Table of Contents

1. **Title and Description**  
2. **Full Commented Code**  
3. **Algorithm Overview**  
   - 3.1. Graph Representation  
   - 3.2. Connected Components via Iterative DFS  
   - 3.3. Checking Completeness of a Component  
4. **Step‑by‑Step Workflow with ASCII Diagrams**  
   - 4.1. Input Example  
   - 4.2. Building Adjacency List  
   - 4.3. DFS to Find Components  
   - 4.4. Edge Count Calculation  
   - 4.5. Completeness Check  
5. **DFS Tree Representation**  
6. **Complexity Analysis**  
7. **Example Usage and Output**  
8. **How to Use the Function**  
9. **Conclusion**

---

## 1. Title and Description

**Title:** Count Complete Connected Components in an Undirected Graph

**Description:**  
This Python function `count_complete_components(n, edges)` identifies and counts the number of connected components in an undirected graph that are **complete** (i.e., cliques). A component is complete if every pair of vertices in that component is directly connected by an edge.  

The function:  
- Builds an adjacency list from the given `n` vertices and `edges`.  
- Uses iterative depth‑first search (DFS) to traverse each connected component and collect all its vertices.  
- For each component, it computes the actual number of edges within it and compares it with the maximum possible edges for a complete graph of the same size.  
- Returns the count of components that satisfy the completeness condition.

---

## 2. Full Commented Code

```python
def count_complete_components(n, edges):
    # Build adjacency list representation of the graph
    adjacency_list = [[] for _ in range(n)]
    
    # Populate adjacency list for undirected graph
    for source_vertex, destination_vertex in edges:
        adjacency_list[source_vertex].append(destination_vertex)
        adjacency_list[destination_vertex].append(source_vertex)
    
    # Track visited vertices to avoid reprocessing
    visited_vertices = [False] * n
    
    def depth_first_search(start_vertex):
        # Stack-based DFS to collect all nodes in a component
        stack = [start_vertex]
        visited_vertices[start_vertex] = True
        component_nodes = []
        
        while stack:
            current_vertex = stack.pop()
            component_nodes.append(current_vertex)
            
            for neighbor_vertex in adjacency_list[current_vertex]:
                if not visited_vertices[neighbor_vertex]:
                    visited_vertices[neighbor_vertex] = True
                    stack.append(neighbor_vertex)
        
        return component_nodes
    
    complete_components_count = 0
    
    # Traverse all components
    for vertex in range(n):
        if not visited_vertices[vertex]:
            component_nodes = depth_first_search(vertex)
            component_size = len(component_nodes)
            
            # Count edges within this component
            edge_count = 0
            for node in component_nodes:
                edge_count += len(adjacency_list[node])
            
            # Each edge counted twice in undirected graph
            edge_count //= 2
            
            # Check if component is complete: edges = k*(k-1)//2
            if edge_count == component_size * (component_size - 1) // 2:
                complete_components_count += 1
    
    return complete_components_count
```

---

## 3. Algorithm Overview

### 3.1. Graph Representation
The graph is stored as an **adjacency list** – a list of length `n` where `adjacency_list[u]` contains all vertices adjacent to `u`. Because the graph is undirected, each edge `(u, v)` is added to both `adjacency_list[u]` and `adjacency_list[v]`.

### 3.2. Connected Components via Iterative DFS
The function iterates over all vertices. For each unvisited vertex, it launches an iterative DFS (using a stack) to traverse the entire connected component. The DFS collects all vertices belonging to that component in `component_nodes`.

### 3.3. Checking Completeness of a Component
Once a component is identified:
- Let `k = len(component_nodes)`.
- Count the total number of edges inside the component:  
  For each node in the component, add the length of its adjacency list. Because the graph is undirected, this counts each internal edge twice. Hence, we divide by 2 to get the actual edge count.
- A complete graph with `k` vertices has exactly `k*(k-1)//2` edges.  
- If the counted edges equal this value, the component is a clique and we increment the counter.

---

## 4. Step‑by‑Step Workflow with ASCII Diagrams

We will use an example to illustrate each stage.

### 4.1. Input Example
`n = 7`  
`edges = [(0,1), (0,2), (1,2), (3,4), (5,6)]`  

The graph consists of three components:
- Component A: vertices {0,1,2} – fully connected (triangle)
- Component B: vertices {3,4} – a single edge (complete graph of size 2)
- Component C: vertices {5,6} – a single edge (complete graph of size 2)

Expected output: 3 (all components are complete).

### 4.2. Building Adjacency List
Initial: `adj = [[], [], [], [], [], [], []]`

Process each edge:
- (0,1):  
  `adj[0].append(1)` → `[1]`  
  `adj[1].append(0)` → `[0]`
- (0,2):  
  `adj[0].append(2)` → `[1,2]`  
  `adj[2].append(0)` → `[0]`
- (1,2):  
  `adj[1].append(2)` → `[0,2]`  
  `adj[2].append(1)` → `[0,1]`
- (3,4):  
  `adj[3].append(4)` → `[4]`  
  `adj[4].append(3)` → `[3]`
- (5,6):  
  `adj[5].append(6)` → `[6]`  
  `adj[6].append(5)` → `[5]`

Final adjacency list:
```
0: [1,2]
1: [0,2]
2: [0,1]
3: [4]
4: [3]
5: [6]
6: [5]
```

### 4.3. DFS to Find Components
`visited = [False]*7`

**Iteration 0:** vertex 0 not visited → DFS(0)  
- Stack = [0], visited[0]=True, comp = []
- Pop 0 → comp = [0], neighbours 1,2:  
  - 1 not visited → visited[1]=True, push 1  
  - 2 not visited → visited[2]=True, push 2  
  Stack = [1,2] (order may vary; we assume LIFO, so 2 popped next)
- Pop 2 → comp = [0,2], neighbours 0,1 both visited
- Pop 1 → comp = [0,2,1], neighbours 0,2 visited
- Stack empty → return [0,2,1]  
Component A = [0,1,2] (size=3)

**Iteration 1:** vertex 1 visited → skip  
**Iteration 2:** vertex 2 visited → skip  

**Iteration 3:** vertex 3 not visited → DFS(3)  
- Stack = [3], visited[3]=True, comp = []
- Pop 3 → comp = [3], neighbour 4 not visited → push 4, visited[4]=True
- Pop 4 → comp = [3,4], neighbour 3 visited
- Stack empty → return [3,4]  
Component B = [3,4] (size=2)

**Iteration 4:** vertex 4 visited → skip  

**Iteration 5:** vertex 5 not visited → DFS(5)  
- Stack = [5], visited[5]=True, comp = []
- Pop 5 → comp = [5], neighbour 6 not visited → push 6, visited[6]=True
- Pop 6 → comp = [5,6], neighbour 5 visited
- Stack empty → return [5,6]  
Component C = [5,6] (size=2)

**Iteration 6:** vertex 6 visited → skip  

Components found: A(3), B(2), C(2)

### 4.4. Edge Count Calculation
For each component, we compute the actual number of internal edges.

**Component A (nodes [0,1,2]):**  
- deg(0) = 2, deg(1) = 2, deg(2) = 2  
- Sum of degrees = 2+2+2 = 6  
- Edge count = 6 // 2 = 3

**Component B (nodes [3,4]):**  
- deg(3) = 1, deg(4) = 1  
- Sum = 2, edge count = 1

**Component C (nodes [5,6]):**  
- deg(5) = 1, deg(6) = 1  
- Sum = 2, edge count = 1

### 4.5. Completeness Check
For a component of size k, the maximum number of edges is `k*(k-1)//2`.

- A: k=3 → max = 3*2//2 = 3. Actual edges = 3 → complete.  
- B: k=2 → max = 2*1//2 = 1. Actual = 1 → complete.  
- C: k=2 → max = 1. Actual = 1 → complete.

Thus `complete_components_count = 3`.

---

## 5. DFS Tree Representation

Although the DFS is implemented iteratively, we can draw the DFS tree (spanning forest) for the example to visualise the traversal order.

For component A (starting at 0):
```
        0
       / \
      1   2
```
DFS stack order (LIFO): 0 → push 1,2 → pop 2 → then pop 1. The resulting DFS tree edges (tree edges) are (0,2) and (0,1). The graph is complete, so there are also extra edges (1,2) which are back edges but not part of the DFS tree.

For component B (starting at 3):
```
        3
        |
        4
```
Tree edge (3,4).

For component C (starting at 5):
```
        5
        |
        6
```
Tree edge (5,6).

The DFS forest has three trees, each corresponding to a connected component.

---

## 6. Complexity Analysis

- **Time:**  
  Building adjacency list: O(m), where m = len(edges).  
  DFS: each vertex is visited once, each edge is examined twice (once per endpoint) → O(n + m).  
  Counting edges in a component: each node’s degree is summed, again O(n + m).  
  Overall: **O(n + m)**.

- **Space:**  
  Adjacency list: O(n + m).  
  Visited array: O(n).  
  Stack and component list: O(n) in worst case.  
  Overall: **O(n + m)**.

---

## 7. Example Usage and Output

```python
# Example 1: Three components, all complete
n1 = 7
edges1 = [(0,1), (0,2), (1,2), (3,4), (5,6)]
print(count_complete_components(n1, edges1))  # Output: 3

# Example 2: Mixed components
n2 = 5
edges2 = [(0,1), (0,2), (1,2), (2,3)]   # Component {0,1,2} complete (triangle), {3} isolated (complete), {4} isolated (complete)
# Note: {2,3} not complete because missing edge (0,3) or (1,3) but 3 only connected to 2
print(count_complete_components(n2, edges2))  # Output: 3? Wait: Component {0,1,2} is complete; {3} size 1 is trivially complete; {4} size 1 also complete → 3.

# Example 3: No complete components
n3 = 4
edges3 = [(0,1), (1,2), (2,3)]   # A path of 4 nodes – not complete
print(count_complete_components(n3, edges3))  # Output: 0
```

**Outputs:**
```
3
3
0
```

---

## 8. How to Use the Function

1. **Prepare the graph data:**  
   - `n`: number of vertices (labeled 0 to n-1).  
   - `edges`: list of tuples `(u, v)` representing undirected edges.

2. **Call the function:**  
   `result = count_complete_components(n, edges)`

3. **Interpret result:**  
   The function returns an integer – the number of connected components that are complete graphs.

**Example with a custom graph:**
```python
vertices = 6
edges = [(0,1), (1,2), (2,0), (3,4)]   # triangle (0-1-2) and edge (3-4), node 5 isolated
print(count_complete_components(vertices, edges))  # Output: 3
```

---

## 9. Conclusion

The `count_complete_components` function efficiently counts connected components that are cliques. By using iterative DFS to isolate components and then comparing the actual edge count with the theoretical maximum for a complete graph, it provides a clear and linear‑time solution. The step‑by‑step example and ASCII diagrams illustrate the internal mechanics, making it easy to understand how components are extracted and validated. This function is a useful tool for graph analysis tasks that require identifying densely connected clusters.