## Table of Contents

1. **Title and Description**  
2. **Full Commented Code**  
3. **Algorithm Overview**  
   - 3.1. Problem Definition  
   - 3.2. Graph Interpretation  
   - 3.3. DFS Approach for Connected Components  
4. **Step‑by‑Step Workflow with ASCII Diagrams**  
   - 4.1. Example Input  
   - 4.2. Building the Graph from Adjacency Matrix  
   - 4.3. DFS Recursion Steps  
   - 4.4. Stack Frame Evolution  
5. **Recursion Tree Visualization**  
6. **Complexity Analysis**  
7. **Example Usage and Output**  
8. **How to Use the Function**  
9. **Conclusion**

---

## 1. Title and Description

**Title:** Find the Number of Provinces (Connected Components) in an Undirected Graph

**Description:**  
This function `find_circle_num(is_connected)` computes the number of **provinces** in a given graph, where a province is defined as a group of directly or indirectly connected cities. The input is an `n x n` adjacency matrix `is_connected` where `is_connected[i][j] = 1` if city `i` and city `j` are directly connected (undirected), and `0` otherwise. The graph is undirected and the matrix is symmetric (by definition, `is_connected[i][i] = 1` for all `i`).  

The solution performs a depth‑first search (DFS) starting from each unvisited city to mark all cities in its connected component, incrementing a counter for each component. This is a classic connected‑components algorithm on an undirected graph represented by an adjacency matrix.

---

## 2. Full Commented Code

```python
def find_circle_num(is_connected):
    """
    Function to find the number of provinces (connected components) in a graph.

    :param is_connected: List[List[int]] -> n x n adjacency matrix (0/1)
    :return: int -> The number of provinces
    """
    number_of_cities = len(is_connected)

    # Track visited cities to avoid reprocessing
    visited_cities = [False] * number_of_cities

    def depth_first_search(current_city):
        """
        Recursively visits all cities directly or indirectly connected to current_city.
        """
        # Explore all possible neighbors (0 to n-1)
        for neighbor_city in range(number_of_cities):
            # If there is an edge and the neighbor hasn't been visited
            if is_connected[current_city][neighbor_city] == 1 and not visited_cities[neighbor_city]:
                visited_cities[neighbor_city] = True
                depth_first_search(neighbor_city)

    province_count = 0

    # Iterate through all cities
    for city in range(number_of_cities):
        if not visited_cities[city]:
            # Start a new DFS for an unvisited city – this city starts a new province
            visited_cities[city] = True
            depth_first_search(city)
            province_count += 1

    return province_count


# Expected Output Example:
# Input:
# is_connected = [[1,1,0],[1,1,0],[0,0,1]]
# Output:
# 2
```

---

## 3. Algorithm Overview

### 3.1. Problem Definition
We have `n` cities labelled 0 to `n-1`. The adjacency matrix `is_connected` tells us which pairs of cities are directly connected by a road (undirected). The goal is to count the number of **connected components** (provinces), i.e., groups of cities where you can travel between any two cities via some sequence of roads.

### 3.2. Graph Interpretation
The matrix defines an undirected graph:
- Vertices: cities.
- Edge between `i` and `j` if `is_connected[i][j] = 1` (and `i ≠ j`; diagonal entries are always 1 but are ignored for traversal because they represent a self‑loop, which doesn't affect connectivity).

Because the graph is undirected, the matrix is symmetric: `is_connected[i][j] == is_connected[j][i]` for all `i, j`.

### 3.3. DFS Approach for Connected Components
We perform a graph traversal to find all vertices reachable from each unvisited vertex:
- Maintain a boolean array `visited_cities` to track visited cities.
- Iterate over all cities. If a city is unvisited, it marks the start of a new province.
- Perform DFS (recursive) from that city, marking all cities reachable from it as visited.
- Increment the province counter for each new component.

The recursion explores all neighbours of the current city; because the graph is undirected and the matrix is dense (but not necessarily fully connected), we use a simple loop over all vertices to check adjacency.

---

## 4. Step‑by‑Step Workflow with ASCII Diagrams

### 4.1. Example Input
`is_connected = [[1,1,0],[1,1,0],[0,0,1]]`

This represents 3 cities (0,1,2). The matrix:

```
    0  1  2
0   1  1  0
1   1  1  0
2   0  0  1
```

Interpretation:
- City 0 is connected to 0 (self) and 1.
- City 1 is connected to 0 and 1 (self).
- City 2 is connected only to itself.

So the undirected edges: 0–1. City 2 is isolated.

The graph looks like:

```
0 — 1      2
```

### 4.2. Building the Graph from Adjacency Matrix
We can visualise the adjacency matrix as a grid:

```
  0 1 2
0 1 1 0
1 1 1 0
2 0 0 1
```

In DFS, for each city we loop over all neighbours (columns) and check if there is a `1` (except when the neighbour is the same city, but that doesn't matter because it's already visited by the time we process the loop? Actually we skip only if visited, so self‑loop (i,i) would be visited already? We'll see the flow.)

### 4.3. DFS Recursion Steps
We simulate the DFS process.

**Initial state:**  
`number_of_cities = 3`  
`visited = [False, False, False]`  
`province_count = 0`

**Iteration for city 0:**  
`visited[0]` is False → start new province.  
- Set `visited[0] = True`  
- Call `dfs(0)`  
  - In `dfs(0)`, loop over neighbor_city in 0..2:
    - neighbor=0: `is_connected[0][0] == 1` but `visited[0]` is True → skip.
    - neighbor=1: `is_connected[0][1] == 1` and `visited[1]` is False → set `visited[1]=True`, call `dfs(1)`.
      - In `dfs(1)`, loop over neighbor 0..2:
        - neighbor=0: `is_connected[1][0] == 1` and `visited[0]` is True → skip.
        - neighbor=1: self, visited → skip.
        - neighbor=2: `is_connected[1][2] == 0` → skip.
      - Return from `dfs(1)`.
    - neighbor=2: `is_connected[0][2] == 0` → skip.
  - Return from `dfs(0)`.
- After `dfs(0)` returns, increment `province_count` to 1.

**Now visited = [True, True, False]**

**Iteration for city 1:** visited[1] is True → skip.

**Iteration for city 2:** visited[2] is False → start new province.
- Set `visited[2] = True`
- Call `dfs(2)`
  - Loop neighbor 0..2:
    - neighbor=0: `is_connected[2][0] == 0` → skip.
    - neighbor=1: `is_connected[2][1] == 0` → skip.
    - neighbor=2: self, visited → skip.
  - Return.
- Increment `province_count` to 2.

**Iteration ends.**  
Return `province_count = 2`.

### 4.4. Stack Frame Evolution
We can illustrate the call stack during the first DFS:

```
dfs(0) called
  -> sets visited[0]=True (already set before call)
  -> neighbor=1 → visited[1]=True, call dfs(1)
    dfs(1) called
      -> neighbor=0 visited; neighbor=2 no edge; return
    back to dfs(0)
  -> neighbor=2 no edge; return
```

The recursion depth is 2.

For the second DFS (city 2), depth is 1.

---

## 5. Recursion Tree Visualization

The recursion tree for the entire algorithm (showing only DFS calls for unvisited cities):

```
Component 1 (starting at city 0):
    dfs(0)
    └── dfs(1)

Component 2 (starting at city 2):
    dfs(2)
```

No further calls because city 2 has no outgoing edges (except self).

---

## 6. Complexity Analysis

- **Time:**  
  The adjacency matrix is `n x n`. For each city, we iterate over all `n` neighbours. Each DFS call visits each city at most once, so total neighbour checks = O(n²).  
  Thus time complexity = **O(n²)**.

- **Space:**  
  `visited` array: O(n).  
  Recursion stack: worst‑case depth = n (e.g., a chain).  
  Total: **O(n)**.

---

## 7. Example Usage and Output

```python
# Example 1: The given matrix
is_connected1 = [[1,1,0],[1,1,0],[0,0,1]]
print(find_circle_num(is_connected1))  # Output: 2

# Example 2: All cities connected (single province)
is_connected2 = [[1,1,1],[1,1,1],[1,1,1]]
print(find_circle_num(is_connected2))  # Output: 1

# Example 3: No connections (each city isolated)
is_connected3 = [[1,0,0],[0,1,0],[0,0,1]]
print(find_circle_num(is_connected3))  # Output: 3

# Example 4: Two provinces: {0,2} and {1}
is_connected4 = [[1,0,1],[0,1,0],[1,0,1]]
print(find_circle_num(is_connected4))  # Output: 2

# Example 5: n = 1
is_connected5 = [[1]]
print(find_circle_num(is_connected5))  # Output: 1
```

**Outputs:**
```
2
1
3
2
1
```

---

## 8. How to Use the Function

1. **Prepare the adjacency matrix** as a list of lists of integers (0 or 1) representing an undirected graph. The matrix must be square and symmetric. The diagonal entries are usually 1 (self‑connection) but they don't affect the count because we skip already visited cities.
2. **Call** `find_circle_num(is_connected)`.
3. The function returns an integer – the number of connected components (provinces).

**Example call:**
```python
matrix = [[1,1,0],[1,1,0],[0,0,1]]
provinces = find_circle_num(matrix)
print("Number of provinces:", provinces)  # Number of provinces: 2
```

---

## 9. Conclusion

The `find_circle_num` function effectively counts connected components in an undirected graph represented by an adjacency matrix using DFS. The algorithm visits each city once and recursively explores all neighbours, marking visited cities. The step‑by‑step walkthrough with ASCII diagrams clarifies the traversal order and recursion tree, while the complexity analysis shows that the solution runs in O(n²) time and O(n) space. This is a classic approach to the "Number of Provinces" problem and is widely applicable to graph connectivity tasks.