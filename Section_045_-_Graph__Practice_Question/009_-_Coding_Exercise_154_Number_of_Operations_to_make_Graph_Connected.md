## Table of Contents

1. **Title and Description**  
2. **Full Commented Code**  
3. **Algorithm Overview**  
   - 3.1. Problem Definition  
   - 3.2. Key Insight: Feasibility Condition  
   - 3.3. Graph Representation and Connected Components  
   - 3.4. Minimum Operations Formula  
4. **Step‑by‑Step Workflow with ASCII Diagrams**  
   - 4.1. Example Input  
   - 4.2. Building Adjacency List  
   - 4.3. DFS to Count Components  
   - 4.4. Computing Minimum Operations  
5. **DFS Traversal Tree (Conceptual)**  
6. **Complexity Analysis**  
7. **Example Usage and Output**  
8. **How to Use the Function**  
9. **Conclusion**

---

## 1. Title and Description

**Title:** Minimum Operations to Connect All Computers (Network Connectivity)

**Description:**  
This Python function `minOperationsToConnectComputers(n, connections)` computes the **minimum number of cable operations** required to make all `n` computers (numbered 0 to n‑1) connected into a single network, given an initial set of `connections` (undirected cables). A single operation consists of removing a cable and plugging it into another pair of computers. If it is impossible to connect all computers (i.e., there are fewer than `n‑1` cables in total), the function returns `-1`.  

The solution works by:
- Checking the feasibility condition: a network of `n` computers needs at least `n‑1` cables to be connected.
- Building an adjacency list from the given connections.
- Using iterative DFS to count the number of connected components.
- Returning `components - 1` as the minimal number of cable moves, because each operation can reduce the number of components by 1.

---

## 2. Full Commented Code

```python
def minOperationsToConnectComputers(n, connections):
    # If there are fewer than (n - 1) cables, it is impossible to connect all computers
    if len(connections) < n - 1:
        return -1

    # Build adjacency list representation of the network
    adjacency_list = [[] for _ in range(n)]

    for computer_a, computer_b in connections:
        adjacency_list[computer_a].append(computer_b)
        adjacency_list[computer_b].append(computer_a)

    # Track visited computers
    visited_computers = [False] * n

    def depth_first_search(start_computer):
        # Stack-based DFS to explore a connected component
        stack = [start_computer]
        visited_computers[start_computer] = True

        while stack:
            current_computer = stack.pop()

            for neighbor_computer in adjacency_list[current_computer]:
                if not visited_computers[neighbor_computer]:
                    visited_computers[neighbor_computer] = True
                    stack.append(neighbor_computer)

    # Count number of connected components
    connected_components_count = 0

    for computer in range(n):
        if not visited_computers[computer]:
            depth_first_search(computer)
            connected_components_count += 1

    # Minimum operations needed = (number of components - 1)
    return connected_components_count - 1


# Expected Output Example:
# Input:
# n = 4
# connections = [[0,1],[0,2],[1,2]]
# Output:
# 1
```

---

## 3. Algorithm Overview

### 3.1. Problem Definition
We have `n` computers (vertices) and a list of `connections` (undirected edges). The goal is to connect all computers into a single network (spanning tree) by moving cables. Each operation consists of:
- Removing one existing cable (edge).
- Plugging it between two computers that are not already directly connected (possibly creating a new edge).

We want the **minimum number** of such moves. If it’s impossible (i.e., there are fewer than `n‑1` cables in total), return `-1`.

### 3.2. Key Insight: Feasibility Condition
A connected graph on `n` vertices must have at least `n‑1` edges. Therefore, if `len(connections) < n‑1`, it is impossible to connect all computers regardless of how we rearrange the cables, because we lack enough cables. In that case, we immediately return `-1`.

### 3.3. Graph Representation and Connected Components
The initial network forms an undirected graph. The number of connected components in this graph determines how many separate clusters of computers exist. Each component already has enough internal connections to be internally connected (though not necessarily a tree; it could have cycles).

### 3.4. Minimum Operations Formula
When we have `k` connected components, we need at least `k‑1` additional connections to join them into a single component. Each operation allows us to take a cable from a cycle (if any) or from a component with extra cables and use it to connect two different components. Because we have at least `n‑1` cables total, there are enough cables to build a spanning tree. The minimal number of moves required is exactly `k‑1`. This is a standard result: we can treat each component as a super‑node, and we need `k‑1` edges to connect them; each move supplies one such edge by reusing an existing cable.

Thus the algorithm:
1. Check total cables; if < n‑1 → -1.
2. Build adjacency list from connections.
3. Perform DFS to count connected components.
4. Return components - 1.

---

## 4. Step‑by‑Step Workflow with ASCII Diagrams

We will use the example:  
`n = 4`  
`connections = [[0,1], [0,2], [1,2]]`

### 4.1. Example Input
This represents 4 computers (0,1,2,3) and cables connecting 0‑1, 0‑2, 1‑2. So computers 0,1,2 form a triangle (cycle), and computer 3 is isolated.

Visualisation:

```
0 — 1
| \  |
|   \|
2     3 (isolated)
```

Actually 0‑1‑2 are fully connected (triangle), 3 is separate.

### 4.2. Building Adjacency List
Initial `adj = [[], [], [], []]`

Process each connection:

- `[0,1]`:  
  `adj[0].append(1)` → `[1]`  
  `adj[1].append(0)` → `[0]`

- `[0,2]`:  
  `adj[0].append(2)` → `[1,2]`  
  `adj[2].append(0)` → `[0]`

- `[1,2]`:  
  `adj[1].append(2)` → `[0,2]`  
  `adj[2].append(1)` → `[0,1]`

Final adjacency list:
```
0: [1,2]
1: [0,2]
2: [0,1]
3: []
```

### 4.3. DFS to Count Components
`visited = [False, False, False, False]`

**Iteration for computer 0:**  
visited[0] = False → start DFS(0)  
- stack = [0], visited[0]=True  
- pop 0: neighbours 1,2  
  - 1 not visited → visited[1]=True, push 1  
  - 2 not visited → visited[2]=True, push 2  
- stack = [1,2] (order may vary; assume LIFO, so 2 popped next)  
- pop 2: neighbours 0,1 (both visited)  
- pop 1: neighbours 0,2 (both visited)  
- stack empty.  
- `connected_components_count` = 1

**Now visited = [True, True, True, False]**

**Iteration for computer 1:** visited → skip  
**Iteration for computer 2:** visited → skip  
**Iteration for computer 3:** visited[3] = False → start DFS(3)  
- stack = [3], visited[3]=True  
- pop 3: neighbours []  
- stack empty  
- `connected_components_count` = 2

**After loop, `connected_components_count = 2`.**

### 4.4. Computing Minimum Operations
`connected_components_count - 1 = 2 - 1 = 1`.

Thus only **1 operation** is needed.

**Why?**  
We have two components: {0,1,2} and {3}. We can take a cable from the triangle (e.g., between 1 and 2) and reconnect it to connect component {3} to any computer in the triangle (say, connect 3 to 0). After that, all computers are connected.

---

## 5. DFS Traversal Tree (Conceptual)

The DFS tree for the first component (starting at 0) with the order above (using stack LIFO) is:

```
0
├─ 2
└─ 1
```

In actual traversal, we visited 0 → then 2 → then back to 0 → then 1, but the tree edges are (0,2) and (0,1). The edge (1,2) is a back edge, indicating a cycle. This doesn't affect the component count.

The second component (node 3) is just a single node with no edges.

Thus the forest has 2 trees.

---

## 6. Complexity Analysis

- **Time:**  
  Building adjacency list: O(m), where m = len(connections).  
  DFS: each vertex is visited once, each edge is examined twice (once per endpoint) → O(n + m).  
  Total: **O(n + m)**.

- **Space:**  
  Adjacency list: O(n + m).  
  Visited array: O(n).  
  Stack: O(n) in worst case.  
  Total: **O(n + m)**.

---

## 7. Example Usage and Output

```python
# Example 1: As given
n1 = 4
connections1 = [[0,1],[0,2],[1,2]]
print(minOperationsToConnectComputers(n1, connections1))  # Output: 1

# Example 2: Already connected (n-1 edges)
n2 = 3
connections2 = [[0,1],[1,2]]
print(minOperationsToConnectComputers(n2, connections2))  # Output: 0

# Example 3: Impossible (not enough cables)
n3 = 5
connections3 = [[0,1],[2,3]]
print(minOperationsToConnectComputers(n3, connections3))  # Output: -1

# Example 4: Multiple components with extra cables
n4 = 6
connections4 = [[0,1],[0,2],[1,2],[3,4],[4,5]]  # components: {0,1,2} (3 nodes, 3 edges) and {3,4,5} (3 nodes, 2 edges)
print(minOperationsToConnectComputers(n4, connections4))  # Output: 1 (components=2)

# Example 5: All isolated
n5 = 4
connections5 = []
print(minOperationsToConnectComputers(n5, connections5))  # Output: 3 (components=4, need 3 moves but also check: 4*? actually total cables=0 < 3? n-1=3, 0<3 → impossible, so -1. Wait: len(connections)=0 < 3 → -1. So output -1.
```

**Outputs:**
```
1
0
-1
1
-1
```

---

## 8. How to Use the Function

1. **Define `n`** – the number of computers (vertices).  
2. **Define `connections`** – a list of pairs `[a, b]` representing an undirected cable between computers `a` and `b`.  
3. **Call** `minOperationsToConnectComputers(n, connections)`.  
4. The function returns:
   - `-1` if it’s impossible to connect all computers (insufficient cables).
   - Otherwise, the minimal number of cable moves required.

**Example:**
```python
n = 5
connections = [[0,1], [2,3], [3,4]]
result = minOperationsToConnectComputers(n, connections)
print("Minimum moves:", result)  # Minimum moves: 1? Wait: components: {0,1}, {2,3,4} → 2 components → answer 1. But check cables: 3 cables, n-1=4, so 3<4 → actually impossible? 3<4 → -1. So output -1. Indeed.
```

Always ensure you understand the feasibility condition.

---

## 9. Conclusion

The `minOperationsToConnectComputers` function efficiently determines the minimum number of cable moves needed to connect all computers in a network. It first checks whether there are enough cables (at least n‑1). Then it counts connected components using iterative DFS and returns `components - 1`. The algorithm runs in O(n + m) time and O(n + m) space, making it suitable for large networks. The step‑by‑step illustration with ASCII diagrams clarifies the DFS traversal and component counting. This problem is a classic example of applying graph connectivity concepts to a practical networking scenario.