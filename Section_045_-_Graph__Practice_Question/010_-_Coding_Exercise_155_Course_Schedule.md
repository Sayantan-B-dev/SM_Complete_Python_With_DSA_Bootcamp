## Table of Contents

1. **Title and Description**  
2. **Full Commented Code**  
3. **Algorithm Overview**  
   - 3.1. Problem Definition  
   - 3.2. Graph Interpretation  
   - 3.3. Topological Sort (Kahn’s Algorithm)  
4. **Step‑by‑Step Workflow with ASCII Diagrams**  
   - 4.1. Example Input  
   - 4.2. Building Graph and Indegrees  
   - 4.3. Initialising Queue with Zero‑Indegree Courses  
   - 4.4. Processing Courses (Kahn’s Algorithm)  
   - 4.5. Final Check  
5. **Conceptual Recursion/DFS Alternative**  
6. **Complexity Analysis**  
7. **Example Usage and Output**  
8. **How to Use the Function**  
9. **Conclusion**

---

## 1. Title and Description

**Title:** Can Finish All Courses (Course Schedule)

**Description:**  
This function `can_finish_courses(N, prerequisites)` determines whether it is possible to finish all `N` courses (labeled `1` to `N`) given a list of prerequisites. Each prerequisite is a pair `[course, prerequisite]` meaning that `course` depends on `prerequisite` – you must complete the prerequisite before taking the course.  

The problem reduces to checking if the directed graph of courses (vertices) and dependencies (edges) has a **topological order**. If a topological order exists, then there is a valid sequence to take all courses. Otherwise, a cycle in the dependency graph makes it impossible.  

The solution uses **Kahn’s algorithm** (topological sort based on indegree) to iteratively remove courses with no remaining prerequisites and count how many can be completed. If all courses can be processed, the answer is `True`; otherwise `False`.

---

## 2. Full Commented Code

```python
def can_finish_courses(N, prerequisites):
    # Build adjacency list and indegree count for each course
    adjacency_list = {i: [] for i in range(1, N + 1)}
    indegree_count = {i: 0 for i in range(1, N + 1)}

    # Populate graph and indegree based on prerequisites
    for course, prerequisite in prerequisites:
        adjacency_list[prerequisite].append(course)
        indegree_count[course] += 1

    # Queue to process courses with no prerequisites
    from collections import deque
    processing_queue = deque()

    for course in range(1, N + 1):
        if indegree_count[course] == 0:
            processing_queue.append(course)

    completed_courses_count = 0

    # Kahn's Algorithm for Topological Sorting
    while processing_queue:
        current_course = processing_queue.popleft()
        completed_courses_count += 1

        for dependent_course in adjacency_list[current_course]:
            indegree_count[dependent_course] -= 1
            if indegree_count[dependent_course] == 0:
                processing_queue.append(dependent_course)

    # If all courses are processed, it is possible to finish all courses
    return completed_courses_count == N


# Expected Output Example:
# Input:
# N = 2
# prerequisites = [[2,1]]
# Output:
# True
```

---

## 3. Algorithm Overview

### 3.1. Problem Definition
We have `N` courses labelled `1` to `N`. The `prerequisites` list contains pairs `[course, prerequisite]`, indicating that `course` requires `prerequisite` to be taken first. The goal is to determine if there exists an ordering of all courses that respects all prerequisites. This is exactly the problem of finding a topological order in a directed graph.

### 3.2. Graph Interpretation
We model each course as a vertex. For each prerequisite `[course, prerequisite]`, we add a directed edge from `prerequisite` → `course`. That means `prerequisite` must come before `course`.

The existence of a topological order is equivalent to the graph being **acyclic** (a DAG). If there is a cycle, it’s impossible to satisfy all dependencies.

### 3.3. Topological Sort (Kahn’s Algorithm)
Kahn’s algorithm works by repeatedly removing vertices that have no incoming edges (indegree zero) and decreasing the indegree of their neighbours. Steps:
1. Compute indegree (number of incoming edges) for each vertex.
2. Initialise a queue with all vertices that have indegree 0.
3. While queue not empty:
   - Pop a vertex `u` – it can be taken now.
   - For each neighbour `v` of `u`:
     - Decrease indegree of `v` by 1 (because we are removing `u`).
     - If indegree of `v` becomes 0, push `v` into the queue.
4. Count how many vertices were processed. If the count equals `N`, a topological order exists; otherwise, a cycle exists.

---

## 4. Step‑by‑Step Workflow with ASCII Diagrams

We will use the example:  
`N = 4`  
`prerequisites = [[2,1], [3,1], [4,2], [4,3]]`

This graph represents:
- Course 2 depends on 1
- Course 3 depends on 1
- Course 4 depends on 2 and 3

### 4.1. Example Input
Courses: 1, 2, 3, 4.  
Dependencies:
```
1 → 2
1 → 3
2 → 4
3 → 4
```
The graph:

```
      1
     / \
    2   3
     \ /
      4
```

### 4.2. Building Graph and Indegrees
We initialise:
- `adjacency_list` for each course (1..4) as empty list.
- `indegree_count` for each course as 0.

Process each prerequisite `[course, prerequisite]`:
- `[2,1]`: add edge 1→2: `adj[1].append(2)`, `indegree[2]++` → indegrees: [1:0, 2:1, 3:0, 4:0]
- `[3,1]`: add edge 1→3: `adj[1].append(3)`, `indegree[3]++` → [1:0, 2:1, 3:1, 4:0]
- `[4,2]`: add edge 2→4: `adj[2].append(4)`, `indegree[4]++` → [1:0, 2:1, 3:1, 4:1]
- `[4,3]`: add edge 3→4: `adj[3].append(4)`, `indegree[4]++` → [1:0, 2:1, 3:1, 4:2]

Final:
```
adjacency_list:
1: [2,3]
2: [4]
3: [4]
4: []

indegree_count:
1:0, 2:1, 3:1, 4:2
```

### 4.3. Initialising Queue with Zero‑Indegree Courses
Courses with indegree 0: course 1.  
`queue = deque([1])`

### 4.4. Processing Courses (Kahn’s Algorithm)
We simulate step by step:

**Step 1:** queue = [1]  
- Pop 1 → completed = 1  
- For each dependent in adj[1] = [2,3]:
  - 2: indegree[2] = 1 → becomes 0 → push 2 → queue = [2]
  - 3: indegree[3] = 1 → becomes 0 → push 3 → queue = [2,3]

**Step 2:** queue = [2,3]  
- Pop 2 → completed = 2  
- Dependents of 2: [4]  
  - 4: indegree[4] = 2 → becomes 1 → not zero → no push  
- Queue now = [3]

**Step 3:** queue = [3]  
- Pop 3 → completed = 3  
- Dependents of 3: [4]  
  - 4: indegree[4] = 1 → becomes 0 → push 4 → queue = [4]

**Step 4:** queue = [4]  
- Pop 4 → completed = 4  
- Dependents of 4: [] → nothing  
- Queue empty.

**Completed = 4, N = 4 → True**.

### 4.5. Final Check
All courses processed → no cycle → returns `True`.

If there were a cycle (e.g., prerequisites [[1,2],[2,1]]), the indegrees would never all become zero, and some courses would remain unprocessed.

---

## 5. Conceptual Recursion/DFS Alternative

Although the algorithm uses iterative BFS (Kahn), the problem can also be solved with DFS‑based cycle detection. We can think of a DFS that attempts to assign a topological order recursively. The recursion tree for a DFS approach would look like this for the same graph:

```
DFS(1)
├─ DFS(2)
│   └─ DFS(4) → returns
└─ DFS(3)
    └─ DFS(4) → returns
```

If a cycle exists, the recursion would detect a back edge. Kahn’s algorithm is often simpler for this problem.

---

## 6. Complexity Analysis

- **Time:**  
  Building adjacency list and indegree: O(P), where P = len(prerequisites).  
  Each vertex is enqueued once and processed once. For each edge, we decrement indegree once.  
  Total: **O(N + P)**.

- **Space:**  
  Adjacency list: O(N + P).  
  Indegree array: O(N).  
  Queue: O(N) in worst case.  
  Total: **O(N + P)**.

---

## 7. Example Usage and Output

```python
# Example 1: Possible
N1 = 2
prereq1 = [[2,1]]
print(can_finish_courses(N1, prereq1))  # True

# Example 2: Possible (linear chain)
N2 = 4
prereq2 = [[2,1],[3,2],[4,3]]
print(can_finish_courses(N2, prereq2))  # True

# Example 3: Cycle → impossible
N3 = 3
prereq3 = [[1,2],[2,3],[3,1]]
print(can_finish_courses(N3, prereq3))  # False

# Example 4: Multiple dependencies, no cycle
N4 = 5
prereq4 = [[2,1],[3,1],[4,2],[5,3],[5,4]]
print(can_finish_courses(N4, prereq4))  # True

# Example 5: No prerequisites
N5 = 3
prereq5 = []
print(can_finish_courses(N5, prereq5))  # True
```

**Outputs:**
```
True
True
False
True
True
```

---

## 8. How to Use the Function

1. **Define `N`** – the total number of courses (positive integer).  
2. **Define `prerequisites`** – a list of pairs `[course, prerequisite]` where `course` depends on `prerequisite`.  
3. **Call** `can_finish_courses(N, prerequisites)`.  
4. The function returns `True` if all courses can be finished, `False` otherwise.

**Example call:**
```python
courses = 3
deps = [[2,1],[3,2]]
result = can_finish_courses(courses, deps)
print("Can finish all courses?", result)   # True
```

---

## 9. Conclusion

The `can_finish_courses` function determines the feasibility of completing all courses by modelling dependencies as a directed graph and applying Kahn’s topological sort algorithm. It efficiently checks for cycles and produces a clear boolean answer. The step‑by‑step walkthrough with ASCII diagrams demonstrates the evolution of indegrees and the queue, making the algorithm easy to follow. The solution runs in linear time and space relative to the number of courses and prerequisites. This approach is a classic application of graph theory in scheduling problems.