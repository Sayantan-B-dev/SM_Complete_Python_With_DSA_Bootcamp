## Table of Contents

1. **Title and Description**  
2. **Full Commented Code**  
3. **Algorithm Overview**  
   - 3.1. Graph Representation  
   - 3.2. Connected Components via Iterative DFS  
   - 3.3. Counting Unreachable Pairs  
4. **Step-by-Step Workflow with ASCII Diagrams**  
   - 4.1. Input Graph Example  
   - 4.2. Building Adjacency List  
   - 4.3. DFS to Find Component Sizes  
   - 4.4. Accumulating Unreachable Pairs  
5. **Recursion Tree (Conceptual)**  
6. **Complexity Analysis**  
7. **Example Usage and Output**  
8. **How to Use the Function**  
9. **Conclusion**

---

## 1. Title and Description

**Title:** Count Unreachable Pairs in an Undirected Graph  

**Description:**  
This Python function `count_unreachable_pairs(n, edges)` computes the number of unordered pairs of vertices `(u, v)` such that there is **no path** between `u` and `v` in an undirected graph with `n` vertices (labeled `0` to `n-1`) and a given list of edges.  

The function achieves this by:  
- Building an adjacency list representation of the graph.  
- Performing iterative depth‑first search (DFS) to find all connected components.  
- Calculating the size of each component.  
- Using a summation technique to count all pairs of vertices that lie in different components – these are precisely the unreachable pairs.

The algorithm avoids double counting and runs in **O(n + m)** time, where `m = len(edges)`.

---

## 2. Full Commented Code

```python
def count_unreachable_pairs(n, edges):
    """
    Count the number of unordered pairs (u, v) such that u and v are not connected
    in an undirected graph with n vertices and given edges.

    Parameters:
        n (int): Number of vertices (0 .. n-1)
        edges (list of tuples): Each tuple (u, v) represents an undirected edge.

    Returns:
        int: Number of unreachable vertex pairs.
    """
    # Build adjacency list representation of the graph
    adjacency_list = [[] for _ in range(n)]

    # Populate adjacency list with undirected edges
    for source_vertex, destination_vertex in edges:
        adjacency_list[source_vertex].append(destination_vertex)
        adjacency_list[destination_vertex].append(source_vertex)

    # Visited array to track explored vertices
    visited_vertices = [False] * n

    def depth_first_search(start_vertex):
        """
        Iterative DFS that returns the size of the connected component
        containing start_vertex.
        """
        stack = [start_vertex]          # Use a stack for DFS
        visited_vertices[start_vertex] = True
        component_size = 0

        while stack:
            current_vertex = stack.pop()
            component_size += 1

            for neighbor_vertex in adjacency_list[current_vertex]:
                if not visited_vertices[neighbor_vertex]:
                    visited_vertices[neighbor_vertex] = True
                    stack.append(neighbor_vertex)

        return component_size

    # Store sizes of all connected components
    connected_component_sizes = []

    for vertex in range(n):
        if not visited_vertices[vertex]:
            component_size = depth_first_search(vertex)
            connected_component_sizes.append(component_size)

    # Calculate number of unreachable pairs
    unreachable_pairs_count = 0
    remaining_vertices = n

    for size in connected_component_sizes:
        remaining_vertices -= size           # vertices not yet processed
        unreachable_pairs_count += size * remaining_vertices

    return unreachable_pairs_count
```

---

## 3. Algorithm Overview

### 3.1. Graph Representation
The graph is stored as an **adjacency list** – a list of length `n` where `adjacency_list[u]` contains all vertices directly connected to `u` by an edge. Because the graph is undirected, each edge `(u, v)` is added to both `adjacency_list[u]` and `adjacency_list[v]`.

### 3.2. Connected Components via Iterative DFS
The function traverses all vertices. For each unvisited vertex, it launches a DFS (implemented iteratively with a stack) to explore its entire connected component. During the traversal, every visited vertex is marked, and the total number of vertices in that component is counted. The sizes are stored in the list `connected_component_sizes`.

### 3.3. Counting Unreachable Pairs
Let the component sizes be `s1, s2, ..., sk`.  
The total number of unordered vertex pairs is `total = n * (n-1) / 2`.  
Pairs that are reachable (i.e., belong to the same component) sum to `reachable = sum_{i=1}^k s_i * (s_i - 1) / 2`.  
Thus unreachable = total - reachable.

The code uses an equivalent **on‑the‑fly** method:  
- Initialize `remaining = n` and `unreachable = 0`.  
- For each component of size `size`:
  - Subtract `size` from `remaining` (so `remaining` now holds the number of vertices not yet considered).
  - Add `size * remaining` to `unreachable`.  
This works because every pair of vertices from two different components is counted exactly once – when the first of the two components is processed.

---

## 4. Step‑by‑Step Workflow with ASCII Diagrams

Let’s work through a concrete example.

**Example:**  
`n = 6`  
`edges = [(0,1), (0,2), (3,4)]`  

The graph looks like this:

```
0 -- 1         3 -- 4
|              |
2              5 (isolated)
```

Vertices: {0,1,2} form one component; {3,4} form a second component; {5} is an isolated vertex (third component).

### 4.1. Input Graph Example

```
Component A: 0--1
             |
             2
Component B: 3--4
Component C: 5
```

### 4.2. Building Adjacency List

Initial: `adj = [[], [], [], [], [], []]`

Process each edge:

- Edge (0,1):  
  `adj[0].append(1) → [1]`  
  `adj[1].append(0) → [0]`

- Edge (0,2):  
  `adj[0].append(2) → [1,2]`  
  `adj[2].append(0) → [0]`

- Edge (3,4):  
  `adj[3].append(4) → [4]`  
  `adj[4].append(3) → [3]`

Final adjacency list:
```
0: [1,2]
1: [0]
2: [0]
3: [4]
4: [3]
5: []
```

### 4.3. DFS to Find Component Sizes

`visited = [False, False, False, False, False, False]`

**Iteration over vertices 0..5**

- Vertex 0: not visited → DFS(0)

  ```
  DFS(0):
    stack = [0], visited[0]=True, size=0
    pop 0 → size=1
    neighbours of 0: 1 (not visited) → visited[1]=True, push 1; 2 (not visited) → visited[2]=True, push 2
    stack = [1,2]
    pop 2 → size=2
    neighbours of 2: 0 (already visited)
    stack = [1]
    pop 1 → size=3
    neighbours of 1: 0 (visited)
    stack = [] → return size=3
  ```
  Component size = 3 → `sizes = [3]`

  Now visited: [True, True, True, False, False, False]

- Vertex 1: visited → skip  
- Vertex 2: visited → skip  
- Vertex 3: not visited → DFS(3)

  ```
  DFS(3):
    stack = [3], visited[3]=True, size=0
    pop 3 → size=1
    neighbours of 3: 4 (not visited) → visited[4]=True, push 4
    stack = [4]
    pop 4 → size=2
    neighbours of 4: 3 (visited)
    stack = [] → return size=2
  ```
  Component size = 2 → `sizes = [3,2]`

  visited: [True, True, True, True, True, False]

- Vertex 4: visited → skip  
- Vertex 5: not visited → DFS(5)

  ```
  DFS(5):
    stack = [5], visited[5]=True, size=0
    pop 5 → size=1
    neighbours of 5: none
    stack = [] → return size=1
  ```
  Component size = 1 → `sizes = [3,2,1]`

### 4.4. Accumulating Unreachable Pairs

`remaining = 6, unreachable = 0`

- Process size = 3:  
  `remaining = 6 - 3 = 3`  
  `unreachable = 0 + 3*3 = 9`

- Process size = 2:  
  `remaining = 3 - 2 = 1`  
  `unreachable = 9 + 2*1 = 11`

- Process size = 1:  
  `remaining = 1 - 1 = 0`  
  `unreachable = 11 + 1*0 = 11`

Result = 11

**Verification with formula:**  
Total pairs = 6*5/2 = 15  
Reachable pairs = 3*2/2 + 2*1/2 + 1*0/2 = 3 + 1 + 0 = 4  
Unreachable = 15 - 4 = 11 ✓

---

## 5. Recursion Tree (Conceptual)

Although the code uses an **iterative** DFS (stack‑based), a recursive DFS would look very similar. Below is a recursion tree for the same graph starting from vertex 0 in component A.  

Recursive function `dfs(v)` marks `v` visited and recursively calls itself for each unvisited neighbour. The call tree:

```
dfs(0)
├── dfs(1)          [neighbour of 0]
│   └── (neighbours: 0 → already visited)
└── dfs(2)          [neighbour of 0]
    └── (neighbours: 0 → already visited)
```

For component B (starting at 3):

```
dfs(3)
└── dfs(4)          [neighbour of 3]
    └── (neighbours: 3 → already visited)
```

For component C (starting at 5):

```
dfs(5)
└── (no neighbours)
```

The recursion depth equals the diameter of the component, and each vertex is visited exactly once.

---

## 6. Complexity Analysis

- **Time:**  
  Building adjacency list: O(m) (m = number of edges)  
  DFS traversal: each vertex and each edge is processed once → O(n + m)  
  Accumulating pairs: O(k) where k is number of components (≤ n)  
  Overall: O(n + m)

- **Space:**  
  Adjacency list: O(n + m)  
  Visited array: O(n)  
  Stack: O(n) in worst case (chain graph)  
  Component sizes list: O(n)  
  Overall: O(n + m)

---

## 7. Example Usage and Output

Here are several test cases to illustrate the function.

```python
# Example 1: Graph with three components (as above)
n1 = 6
edges1 = [(0,1), (0,2), (3,4)]
print(count_unreachable_pairs(n1, edges1))  # Output: 11

# Example 2: Complete graph (all vertices connected)
n2 = 4
edges2 = [(0,1), (0,2), (0,3), (1,2), (1,3), (2,3)]
print(count_unreachable_pairs(n2, edges2))  # Output: 0

# Example 3: Empty graph (no edges)
n3 = 5
edges3 = []
print(count_unreachable_pairs(n3, edges3))  # Output: 10  (5*4/2 = 10)

# Example 4: Graph with two components of sizes 2 and 3
n4 = 5
edges4 = [(0,1), (2,3), (2,4)]
print(count_unreachable_pairs(n4, edges4))  # Output: 2*3 = 6
```

**Outputs:**
```
11
0
10
6
```

---

## 8. How to Use the Function

1. **Import or copy** the function into your Python environment.
2. **Define `n`** – the number of vertices (0‑based indices).
3. **Define `edges`** as a list of tuples, each tuple `(u, v)` representing an undirected edge.
4. **Call** `count_unreachable_pairs(n, edges)` and store/print the result.

**Example call:**

```python
result = count_unreachable_pairs(7, [(0,1), (2,3), (4,5), (5,6)])
print("Unreachable pairs:", result)
```

This counts unreachable pairs in a graph with vertices 0…6 and edges that form three components: {0,1}, {2,3}, {4,5,6}.  
Output: `Unreachable pairs: 15`  
(Total pairs = 21; reachable pairs = 1+1+3 = 5; unreachable = 16? Wait recalc: component sizes 2,2,3 → reachable = 1+1+3 = 5, total = 21, unreachable = 16. Let's check: 2*2=4 (pairs between first two components) + 2*3=6 (between first and third) + 2*3=6 (between second and third) = 16. So output 16, not 15. My earlier mental math was wrong; correct is 16.)

---

## 9. Conclusion

The function `count_unreachable_pairs` efficiently computes the number of vertex pairs that are not connected in an undirected graph. By finding connected components via iterative DFS and then summing cross‑component pairs, it avoids explicit pair enumeration and runs in linear time relative to the graph size. The step‑by‑step walkthrough with ASCII diagrams clarifies each phase of the algorithm, and the recursion tree concept illustrates how a recursive depth‑first search would traverse the graph. This implementation is both clear and suitable for graphs with up to tens of thousands of vertices and edges.