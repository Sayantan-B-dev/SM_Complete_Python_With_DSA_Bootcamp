## Table of Contents

1. **Title and Description**  
2. **Full Commented Code**  
3. **Algorithm Overview**  
   - 3.1. Problem Definition  
   - 3.2. Graph Interpretation  
   - 3.3. Iterative DFS (Stack)  
4. **Step‑by‑Step Workflow with ASCII Diagrams**  
   - 4.1. Example Input (All Rooms Reachable)  
   - 4.2. Initial State  
   - 4.3. DFS Traversal Steps  
   - 4.4. Final Check  
   - 4.5. Example Input (Not All Rooms Reachable)  
5. **DFS Traversal Tree (Conceptual)**  
6. **Complexity Analysis**  
7. **Example Usage and Output**  
8. **How to Use the Function**  
9. **Conclusion**

---

## 1. Title and Description

**Title:** Can Visit All Rooms (Keys and Rooms)

**Description:**  
This function `can_visit_all_rooms(rooms)` determines whether it is possible to visit every room in a building given that each room may contain keys to other rooms. The input is a list `rooms` where `rooms[i]` is a list of keys (room numbers) found in room `i`. Initially, only room `0` is unlocked and accessible. You can pick up keys as you enter a room and use them to unlock other rooms. The function returns `True` if you can eventually visit all rooms, otherwise `False`.

The solution models the problem as a **directed graph** where each node is a room and each edge `i → j` exists if room `i` contains a key to room `j`. Starting from node `0`, we perform an **iterative depth‑first search (DFS)** using a stack, marking rooms as visited when they are opened. At the end, we check whether all rooms have been visited.

---

## 2. Full Commented Code

```python
def can_visit_all_rooms(rooms):
    # Total number of rooms
    total_rooms = len(rooms)

    # Track visited rooms
    visited_rooms = [False] * total_rooms

    # Stack for DFS traversal starting from room 0
    stack = [0]
    visited_rooms[0] = True

    while stack:
        current_room = stack.pop()

        # Explore all keys in the current room
        for key in rooms[current_room]:
            if not visited_rooms[key]:
                visited_rooms[key] = True
                stack.append(key)

    # Check if all rooms have been visited
    return all(visited_rooms)


# Expected Output Example:
# Input:
# rooms = [[1],[2],[3],[]]
# Output:
# True
```

---

## 3. Algorithm Overview

### 3.1. Problem Definition
We have `n` rooms labeled `0` to `n‑1`. Initially, only room `0` is unlocked. Each room contains a set of keys (room numbers) that can unlock other rooms. When you enter a room, you collect all keys there and can then open any unlocked room. The question is: can you eventually open all rooms?

### 3.2. Graph Interpretation
We can think of the rooms and keys as a **directed graph**:
- Nodes = rooms.
- There is a directed edge from `i` to `j` if room `i` contains a key to room `j` (i.e., `j in rooms[i]`).
Starting from node `0`, we can traverse the graph following these directed edges. The set of rooms reachable from `0` is exactly the set of rooms that can be opened. Thus, the problem reduces to: **Is node 0 connected to all other nodes in this directed graph?**

### 3.3. Iterative DFS (Stack)
We perform a DFS starting from node `0`:
- Use a stack (LIFO) to simulate recursion.
- Maintain a boolean `visited_rooms` array.
- Initially push `0` and mark it visited.
- While the stack is not empty:
  - Pop the current room.
  - For each key (neighbour) in `rooms[current_room]`:
    - If that room has not been visited, mark it visited and push it onto the stack.
- After the stack is empty, all rooms that are reachable from `0` have been visited.
- Return `True` if every room is visited, otherwise `False`.

---

## 4. Step‑by‑Step Workflow with ASCII Diagrams

### 4.1. Example Input (All Rooms Reachable)
We use the example:  
`rooms = [[1], [2], [3], []]`

This corresponds to:
- Room 0 contains key to room 1.
- Room 1 contains key to room 2.
- Room 2 contains key to room 3.
- Room 3 contains no keys.

The directed graph:

```
0 → 1 → 2 → 3
```

### 4.2. Initial State
```
rooms = [[1], [2], [3], []]
total_rooms = 4
visited_rooms = [False, False, False, False]
stack = [0]
visited_rooms[0] = True
```

Now `visited_rooms = [True, False, False, False]`.

### 4.3. DFS Traversal Steps
We simulate the stack:

**Step 1:** `stack = [0]` → pop `0` → `current_room = 0`
- For `key` in `rooms[0] = [1]`:
  - key = 1, `visited_rooms[1]` is False → set True, push 1.
- After loop, `visited_rooms = [True, True, False, False]`, `stack = [1]`.

**Step 2:** pop `1` → `current_room = 1`
- keys = `rooms[1] = [2]` → key = 2, not visited → mark True, push 2.
- `visited_rooms = [True, True, True, False]`, `stack = [2]`.

**Step 3:** pop `2` → `current_room = 2`
- keys = `rooms[2] = [3]` → key = 3, not visited → mark True, push 3.
- `visited_rooms = [True, True, True, True]`, `stack = [3]`.

**Step 4:** pop `3` → `current_room = 3`
- keys = `rooms[3] = []` → no keys.
- `stack = []` → loop ends.

### 4.4. Final Check
`visited_rooms = [True, True, True, True]` → all True → return `True`.

### 4.5. Example Input (Not All Rooms Reachable)
Consider `rooms = [[1], [2], [], [3]]` (room 3 has key to itself, but no key to room 3 from reachable rooms).

Graph:
```
0 → 1 → 2
3 → 3 (self‑loop)
```

**Simulation:**
- Start: stack=[0], visited[0]=True
- Pop 0 → push 1 → visited[1]=True, stack=[1]
- Pop 1 → push 2 → visited[2]=True, stack=[2]
- Pop 2 → no keys → stack empty
- visited = [True, True, True, False] → not all True → return False.

---

## 5. DFS Traversal Tree (Conceptual)

Although we used an iterative stack, the traversal order corresponds to a depth‑first search tree. For the reachable example, the DFS tree is a simple chain:

```
    0
    |
    1
    |
    2
    |
    3
```

The stack‑based DFS traverses nodes in the order 0 → 1 → 2 → 3 (depth‑first). In a more branching example, the stack would explore one branch deeply before backtracking.

---

## 6. Complexity Analysis

- **Time:**  
  Each room is visited at most once (when first opened). For each visited room, we iterate through its list of keys. Total iterations = sum of lengths of `rooms[i]` over all visited rooms, which is at most total number of keys (including duplicates) but here each key is a room number, and we only iterate over keys of visited rooms. In worst case, all rooms visited, so time = O(total number of keys) = O(n + m), where m is total number of keys (sum of lengths). Since each key is a directed edge, this is O(V + E) for the reachable subgraph.  
  Overall: **O(n + total_keys)**.

- **Space:**  
  `visited_rooms` array: O(n).  
  Stack: worst case O(n) (if graph is a chain).  
  Total: **O(n)**.

---

## 7. Example Usage and Output

```python
# Example 1: All rooms reachable
rooms1 = [[1], [2], [3], []]
print(can_visit_all_rooms(rooms1))  # Output: True

# Example 2: Not all rooms reachable (room 3 isolated)
rooms2 = [[1], [2], [], [3]]
print(can_visit_all_rooms(rooms2))  # Output: False

# Example 3: Multiple keys, all reachable
rooms3 = [[1,2], [3], [3], []]
print(can_visit_all_rooms(rooms3))  # Output: True

# Example 4: Cycle (still reachable if all nodes in cycle)
rooms4 = [[1], [0]]  # two rooms, each has key to the other
print(can_visit_all_rooms(rooms4))  # Output: True

# Example 5: Single room
rooms5 = [[]]
print(can_visit_all_rooms(rooms5))  # Output: True
```

**Outputs:**
```
True
False
True
True
True
```

---

## 8. How to Use the Function

1. **Prepare the input** as a list of lists `rooms` where `rooms[i]` contains the keys (room numbers) found in room `i`. The indices must be from `0` to `n‑1` inclusive, and keys must be valid room numbers.
2. **Call** `can_visit_all_rooms(rooms)`.
3. The function returns a boolean: `True` if all rooms can be visited, `False` otherwise.

**Example call:**
```python
rooms = [[1, 2], [3], [3], []]
result = can_visit_all_rooms(rooms)
print("All rooms can be visited?" , result)  # True
```

---

## 9. Conclusion

The `can_visit_all_rooms` function efficiently determines whether all rooms are reachable by modelling the problem as a directed graph reachability from node `0`. Using an iterative DFS with a stack, it visits every reachable room and then checks the visited array. The solution runs in linear time relative to the number of rooms and keys, and uses only O(n) extra space. The step‑by‑step walkthrough with ASCII diagrams demonstrates the stack‑based traversal and clearly shows how the visited array evolves. This approach is simple, intuitive, and widely applicable to similar graph reachability problems.