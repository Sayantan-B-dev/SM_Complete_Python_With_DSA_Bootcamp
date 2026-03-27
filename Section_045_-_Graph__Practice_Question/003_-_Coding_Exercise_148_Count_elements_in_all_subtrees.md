## Table of Contents

1. **Title and Description**  
2. **Full Commented Code**  
3. **Algorithm Overview**  
   - 3.1. Tree Representation  
   - 3.2. Subtree Size Definition  
   - 3.3. DFS Approach  
4. **Step‑by‑Step Workflow with ASCII Diagrams**  
   - 4.1. Input Example  
   - 4.2. Building Adjacency List  
   - 4.3. DFS Recursion Tree  
   - 4.4. Stack Frame Evolution  
   - 4.5. Backtracking and Subtree Size Accumulation  
5. **Recursion Tree Visualization**  
6. **Complexity Analysis**  
7. **Example Usage and Output**  
8. **How to Use the Function**  
9. **Conclusion**

---

## 1. Title and Description

**Title:** Count Subtree Nodes in a Tree  

**Description:**  
This Python function `countSubtreeNodes(n, edges)` computes, for each node in an **undirected tree** (with `n` nodes labeled 0..n-1), the number of nodes in its subtree when the tree is rooted at node 0.  
- The input `edges` is a list of `(parent, child)` pairs (the tree is undirected, so the direction is only conceptual for input).  
- The function builds an adjacency list and performs a **depth‑first search (DFS)** from the root (node 0).  
- For each node, it recursively calculates the size of its subtree (including the node itself) and stores it in an array `subtree_node_counts`.  
- The output is an array of length `n` where `output[i]` is the size of the subtree rooted at `i` in the tree rooted at 0.

---

## 2. Full Commented Code

```python
def countSubtreeNodes(n, edges):
    # Build adjacency list representation of the tree
    adjacency_list = [[] for _ in range(n)]
    
    # Populate adjacency list since the tree is undirected
    for parent_node, child_node in edges:
        adjacency_list[parent_node].append(child_node)
        adjacency_list[child_node].append(parent_node)
    
    # Array to store subtree sizes for each node
    subtree_node_counts = [0] * n
    
    def depth_first_search(current_node, parent_node):
        # Start with count = 1 to include the current node itself
        current_subtree_size = 1
        
        # Traverse all adjacent nodes (children)
        for neighbor_node in adjacency_list[current_node]:
            # Avoid revisiting the parent node
            if neighbor_node == parent_node:
                continue
            
            # Recursively calculate subtree size for child
            current_subtree_size += depth_first_search(neighbor_node, current_node)
        
        # Store computed subtree size for current node
        subtree_node_counts[current_node] = current_subtree_size
        
        return current_subtree_size
    
    # Assume node 0 as root for subtree calculations
    depth_first_search(0, -1)
    
    return subtree_node_counts


# Expected Output Example:
# Input:
# n = 6
# edges = [[0,1],[0,2],[1,3],[1,4],[2,5]]
# Output:
# [6, 3, 2, 1, 1, 1]
```

---

## 3. Algorithm Overview

### 3.1. Tree Representation
The tree is given as an undirected graph with `n` vertices and `n-1` edges. The input `edges` list contains tuples `(u, v)` that are interpreted as edges, not necessarily directed. To navigate the tree, we build an **adjacency list** where each node’s list contains its neighbours.  

### 3.2. Subtree Size Definition
When the tree is rooted at node 0, the **subtree** of a node `x` consists of `x` and all its descendants (nodes that are reachable from `x` without passing through its parent). The size of this subtree is the number of nodes it contains.

### 3.3. DFS Approach
We perform a DFS starting from the root (node 0), using a **parent parameter** to avoid going back up the tree.  
- For each node, we initialise `size = 1` (the node itself).  
- For every neighbour that is not the parent, we recursively compute its subtree size and add it to `size`.  
- After processing all children, we store the total size in `subtree_node_counts[current_node]`.  
- The recursion returns the size to the caller, enabling aggregation.

Because the tree has no cycles, this DFS visits each node exactly once.

---

## 4. Step‑by‑Step Workflow with ASCII Diagrams

### 4.1. Input Example
We use the given example:  
`n = 6`  
`edges = [[0,1], [0,2], [1,3], [1,4], [2,5]]`  

The tree looks like this (rooted at 0):

```
        0
       / \
      1   2
     / \   \
    3   4   5
```

### 4.2. Building Adjacency List
Initial `adjacency_list = [[], [], [], [], [], []]`

Process each edge:
- (0,1): add 1 to adj[0], add 0 to adj[1] → adj[0]=[1], adj[1]=[0]
- (0,2): add 2 to adj[0], add 0 to adj[2] → adj[0]=[1,2], adj[2]=[0]
- (1,3): add 3 to adj[1], add 1 to adj[3] → adj[1]=[0,3], adj[3]=[1]
- (1,4): add 4 to adj[1], add 1 to adj[4] → adj[1]=[0,3,4], adj[4]=[1]
- (2,5): add 5 to adj[2], add 2 to adj[5] → adj[2]=[0,5], adj[5]=[2]

Final adjacency list:
```
0: [1,2]
1: [0,3,4]
2: [0,5]
3: [1]
4: [1]
5: [2]
```

### 4.3. DFS Recursion Tree
We call `depth_first_search(0, -1)`. The recursion unfolds as follows:

```
dfs(0, -1)
│
├─ neighbor 1 (parent = -1 → not parent) → dfs(1,0)
│  │
│  ├─ neighbor 0 (parent=1 → skip)
│  ├─ neighbor 3 (parent=1 → not parent) → dfs(3,1)
│  │  │
│  │  └─ neighbor 1 (parent=3 → skip) → returns 1
│  ├─ neighbor 4 (parent=1 → not parent) → dfs(4,1)
│  │  │
│  │  └─ neighbor 1 (parent=4 → skip) → returns 1
│  └─ after children, size = 1 + 1 + 1 = 3 → returns 3
│
├─ neighbor 2 (parent = -1 → not parent) → dfs(2,0)
│  │
│  ├─ neighbor 0 (parent=2 → skip)
│  ├─ neighbor 5 (parent=2 → not parent) → dfs(5,2)
│  │  │
│  │  └─ neighbor 2 (parent=5 → skip) → returns 1
│  └─ size = 1 + 1 = 2 → returns 2
│
└─ after children, size = 1 + 3 + 2 = 6 → returns 6
```

### 4.4. Stack Frame Evolution
We can illustrate the stack as the recursion proceeds (call stack depth increases then unwinds).  

**Initial call:**  
Stack: [dfs(0, -1)]  

**First child of 0 (node 1):**  
Stack: [dfs(0,-1), dfs(1,0)]  

**Node 1 processes its first child (node 3):**  
Stack: [dfs(0,-1), dfs(1,0), dfs(3,1)]  
dfs(3,1) returns 1 → pops → stack: [dfs(0,-1), dfs(1,0)]

**Node 1 processes its second child (node 4):**  
Stack: [dfs(0,-1), dfs(1,0), dfs(4,1)]  
dfs(4,1) returns 1 → pops → stack: [dfs(0,-1), dfs(1,0)]

**Node 1 finishes:** returns 3 to dfs(0,-1) → stack: [dfs(0,-1)]

**Node 0’s second child (node 2):**  
Stack: [dfs(0,-1), dfs(2,0)]  

**Node 2 processes child (node 5):**  
Stack: [dfs(0,-1), dfs(2,0), dfs(5,2)]  
dfs(5,2) returns 1 → pops → stack: [dfs(0,-1), dfs(2,0)]

**Node 2 finishes:** returns 2 to dfs(0,-1) → stack: [dfs(0,-1)]

**Node 0 finishes:** returns 6 → stack empty.

### 4.5. Backtracking and Subtree Size Accumulation
During backtracking, each node’s `subtree_node_counts` is set to the computed size:

- Node 3: `subtree_node_counts[3] = 1`
- Node 4: `subtree_node_counts[4] = 1`
- Node 1: `subtree_node_counts[1] = 1 + 1 + 1 = 3`
- Node 5: `subtree_node_counts[5] = 1`
- Node 2: `subtree_node_counts[2] = 1 + 1 = 2`
- Node 0: `subtree_node_counts[0] = 1 + 3 + 2 = 6`

Final array: `[6, 3, 2, 1, 1, 1]`

---

## 5. Recursion Tree Visualization

The recursion tree (calls only) with returned sizes can be drawn as:

```
                dfs(0,-1)  → returns 6
               /          \
         dfs(1,0) → 3    dfs(2,0) → 2
         /    \          \
    dfs(3,1) dfs(4,1)   dfs(5,2)
       ↓        ↓          ↓
       1        1          1
```

Each leaf returns 1, and internal nodes sum the returns from children plus 1 for themselves.

---

## 6. Complexity Analysis

- **Time:**  
  Building adjacency list: O(n) (since `n-1` edges, each processed once).  
  DFS traversal: each node is visited once, each edge is traversed twice (once in each direction) but only processed when moving away from parent. Hence total O(n).  
  Overall: **O(n)**.

- **Space:**  
  Adjacency list: O(n) (for `n` nodes, each with degree, total edges `n-1`).  
  Recursion stack: worst‑case O(n) for a skewed tree.  
  Subtree array: O(n).  
  Overall: **O(n)**.

---

## 7. Example Usage and Output

```python
n = 6
edges = [[0,1],[0,2],[1,3],[1,4],[2,5]]
result = countSubtreeNodes(n, edges)
print(result)  # Output: [6, 3, 2, 1, 1, 1]
```

**Additional test cases:**

```python
# Single node
print(countSubtreeNodes(1, []))   # Output: [1]

# Line tree (0-1-2-3)
n2 = 4
edges2 = [[0,1],[1,2],[2,3]]
print(countSubtreeNodes(n2, edges2))  # Output: [4, 3, 2, 1]

# Star tree: root 0 connected to 1,2,3
n3 = 4
edges3 = [[0,1],[0,2],[0,3]]
print(countSubtreeNodes(n3, edges3))  # Output: [4, 1, 1, 1]
```

**Outputs:**
```
[6, 3, 2, 1, 1, 1]
[1]
[4, 3, 2, 1]
[4, 1, 1, 1]
```

---

## 8. How to Use the Function

1. **Prepare the tree data:**  
   - `n`: number of nodes (integers from 0 to n-1).  
   - `edges`: list of `[u, v]` pairs where `u` and `v` are connected by an undirected edge. Ensure it forms a tree (exactly `n-1` edges, no cycles).

2. **Call the function:**  
   `result = countSubtreeNodes(n, edges)`

3. **Interpret result:**  
   `result[i]` gives the size of the subtree rooted at node `i` when the tree is rooted at 0.

**Example with a custom tree:**  
Suppose we have nodes 0,1,2,3 with edges 0-1, 0-2, 2-3. The function call:

```python
my_n = 4
my_edges = [[0,1], [0,2], [2,3]]
print(countSubtreeNodes(my_n, my_edges))  # Output: [4, 1, 2, 1]
```

---

## 9. Conclusion

The `countSubtreeNodes` function efficiently computes subtree sizes for all nodes in a tree using a simple depth‑first search. By leveraging recursion and a parent parameter, it avoids revisiting nodes and correctly accumulates sizes. The algorithm runs in linear time and space, making it suitable for trees of any size. The step‑by‑step illustration with ASCII diagrams clarifies the flow of recursion, and the recursion tree visualisation reinforces the aggregation process. This function is a foundational building block for many tree‑based problems (e.g., finding centroids, computing tree DP).