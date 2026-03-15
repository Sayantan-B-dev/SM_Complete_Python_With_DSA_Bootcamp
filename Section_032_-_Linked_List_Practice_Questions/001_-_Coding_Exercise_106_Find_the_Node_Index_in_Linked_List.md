## Table of Contents

1. [Introduction to Singly Linked Lists and Problem Statement](#intro)  
2. [Complete Code with Detailed Comments](#code)  
3. [Layer‑by‑Layer Explanation](#layers)  
   - 3.1 [The ListNode Class](#listnode)  
   - 3.2 [The find_index Function](#findindex)  
4. [Algorithm – Linear Search on a Linked List](#algorithm)  
5. [Step‑by‑Step Walkthrough with ASCII Diagrams](#walkthrough)  
   - 5.1 [Initial Setup](#initial)  
   - 5.2 [Traversal and Comparison](#traversal)  
   - 5.3 [Returning the Index](#return)  
6. [Complexity Analysis](#complexity)  
7. [Recursive Alternative and Recursion Tree](#recursive)  
   - 7.1 [Recursive Implementation](#recursivecode)  
   - 7.2 [Recursion Tree for the Example](#recursivetree)  
8. [Edge Cases and Additional Considerations](#edgecases)  

---

<a id="intro"></a>
## 1. Introduction to Singly Linked Lists and Problem Statement

A **singly linked list** is a linear data structure where each element (called a **node**) contains two fields:  
- **`val`** – the data (here an integer)  
- **`next`** – a pointer/reference to the next node in the sequence  

The list begins at a special node called the **head**. The last node’s `next` pointer is `None` (or `null`).  

**Problem:** Given the head of a singly linked list and a target integer `k`, find the **index** (0‑based) of the **first** node whose value equals `k`. If the value does not exist, return `‑1`.  

**Example:**  
List: `10 → 20 → 30 → 40`  
Search for `k = 30` → returns `2`  

The provided code solves this exactly.

---

<a id="code"></a>
## 2. Complete Code with Detailed Comments

```python
# Definition for a singly linked list node.
class ListNode:
    def __init__(self, val=0, next=None):
        # Store the value of the node
        self.val = val
        
        # Pointer to the next node in the linked list
        self.next = next


def find_index(head, k):
    """
    Function that searches a singly linked list and returns
    the index of the first node whose value equals k.

    Parameters:
    head (ListNode): The starting node of the linked list
    k (int): Target value to search inside the linked list

    Returns:
    int: Index of the node containing value k
         Returns -1 if the value does not exist in the list
    """

    # Initialize a pointer to traverse the linked list starting from head
    current_node_pointer = head

    # Initialize an index counter to track the position during traversal
    current_index_position = 0

    # Traverse the linked list until the pointer reaches the end
    while current_node_pointer is not None:

        # Check if the current node contains the target value
        if current_node_pointer.val == k:

            # If the value matches, return the current index
            return current_index_position

        # Move the pointer to the next node in the linked list
        current_node_pointer = current_node_pointer.next

        # Increase the index counter because we moved forward in the list
        current_index_position += 1

    # If the loop finishes without finding the value,
    # it means the value does not exist in the linked list
    return -1
```

---

<a id="layers"></a>
## 3. Layer‑by‑Layer Explanation

We now dissect every component, from the bottom‑level node structure up to the high‑level search logic.

<a id="listnode"></a>
### 3.1 The ListNode Class

- **Purpose:** Defines the building block of a singly linked list.  
- **Constructor `__init__`:**  
  - `val=0` – default value is 0, but typically we pass a meaningful integer when creating a node.  
  - `next=None` – by default, a new node points to nothing (end of list).  
- **Fields:**  
  - `self.val` – stores the integer data.  
  - `self.next` – holds a reference to another `ListNode` object, or `None`.  

**Visual representation of a node:**  

```
+-------+------+
| val   | next |
+-------+------+
|   30  |   o-----> (next node or None)
+-------+------+
```

<a id="findindex"></a>
### 3.2 The find_index Function

- **Header:** `def find_index(head, k):`  
  - `head` – a reference to the first node of the list (could be `None` for an empty list).  
  - `k` – the integer we are looking for.  

- **Local variables:**  
  - `current_node_pointer` – starts at `head`; we move it forward one node at a time.  
  - `current_index_position` – starts at `0`; increments each time we move to the next node.  

- **Loop:** `while current_node_pointer is not None:`  
  - The loop runs as long as we haven’t reached the end.  
  - **Inside the loop:**  
    1. Check `if current_node_pointer.val == k` – compare the current node’s value with `k`.  
       - If equal, immediately `return current_index_position`.  
    2. Move to next node: `current_node_pointer = current_node_pointer.next`  
    3. Increment index: `current_index_position += 1`  

- **After loop:** If the loop exits (meaning we traversed the whole list without finding `k`), `return -1`.

**Control flow summary:**  
```
Start with head, index=0
  while node exists:
    if node.val == k -> return index
    node = node.next
    index++
return -1
```

---

<a id="algorithm"></a>
## 4. Algorithm – Linear Search on a Linked List

This is a classic **linear search** adapted to a linked list:  

1. Initialize a pointer to the head.  
2. Initialize a counter `i = 0`.  
3. While the pointer is not `None`:  
   - If the current node’s value equals the target, return `i`.  
   - Move the pointer to the next node.  
   - Increment `i`.  
4. If the loop ends, return `‑1`.  

Because linked lists do not support random access (like arrays), we must traverse sequentially from the head until we either find the value or exhaust the list.

---

<a id="walkthrough"></a>
## 5. Step‑by‑Step Walkthrough with ASCII Diagrams

Let’s trace the example:  

**List:** `10 → 20 → 30 → 40`  
**Target:** `k = 30`  
**Expected output:** `2`  

We’ll represent nodes as boxes with value and a pointer arrow. `None` is shown as a ground symbol (⊥).

<a id="initial"></a>
### 5.1 Initial Setup

```
head ──→ [10]──→[20]──→[30]──→[40]──→⊥

current_node_pointer = head  (points to node with 10)
current_index_position = 0
```

<a id="traversal"></a>
### 5.2 Traversal and Comparison

**Step 1 (index = 0):**  
```
current_node_pointer ──→ [10]──→[20]──→[30]──→[40]──→⊥
```
Check `current_node_pointer.val == 30?` → `10 == 30` → false.  
Move to next: `current_node_pointer = current_node_pointer.next` → now points to node with 20.  
Increment index → `current_index_position = 1`.

**Step 2 (index = 1):**  
```
current_node_pointer ──→ [20]──→[30]──→[40]──→⊥
```
Check `20 == 30?` → false.  
Move to next: points to node with 30.  
Index = 2.

**Step 3 (index = 2):**  
```
current_node_pointer ──→ [30]──→[40]──→⊥
```
Check `30 == 30?` → true.  

<a id="return"></a>
### 5.3 Returning the Index

Because the condition is true, the function executes:  
`return current_index_position` → returns `2`.

The function terminates immediately; no further traversal occurs.

---

<a id="complexity"></a>
## 6. Complexity Analysis

- **Time Complexity:** O(n) in the worst case, where n is the number of nodes. We may have to examine every node (if the value is at the end or not present).  
- **Space Complexity:** O(1). We only use two extra variables (`current_node_pointer` and `current_index_position`) regardless of list size.

---

<a id="recursive"></a>
## 7. Recursive Alternative and Recursion Tree

Although the given code is iterative, searching a linked list can also be implemented recursively. This helps illustrate the underlying recursive nature of linked list traversal.

<a id="recursivecode"></a>
### 7.1 Recursive Implementation

```python
def find_index_recursive(node, k, index=0):
    # Base case 1: empty node -> value not found
    if node is None:
        return -1
    # Base case 2: found the value
    if node.val == k:
        return index
    # Recursive case: search in the rest of the list
    return find_index_recursive(node.next, k, index + 1)
```

- **Parameters:** `node` – current node, `k` – target, `index` – current position (default 0).  
- **Base cases:**  
  - If `node is None` → reached end → return `‑1`.  
  - If `node.val == k` → return `index`.  
- **Recursive step:** Call the function with `node.next` and `index+1`.

<a id="recursivetree"></a>
### 7.2 Recursion Tree for the Example

List: `10 → 20 → 30 → 40`  
Call: `find_index_recursive(node=10, k=30, index=0)`

We draw each recursive call as a node in a tree. The return values propagate upwards.

```
Level 0: find_index_recursive(10, 30, 0)
           |
           | (10 != 30, so go to node.next, index+1)
           v
Level 1: find_index_recursive(20, 30, 1)
           |
           | (20 != 30)
           v
Level 2: find_index_recursive(30, 30, 2)
           |
           | (30 == 30) → return 2
           v
Level 1 receives 2 and returns 2
           |
Level 0 receives 2 and returns 2
```

ASCII representation:

```
find_index_recursive(10,30,0)
│
└── find_index_recursive(20,30,1)
    │
    └── find_index_recursive(30,30,2)   ← returns 2
        (no further calls)
```

The recursion depth equals the index where the value is found (2 in this case) plus one. If the value were missing, we would go down to the `None` node and then unwind returning `‑1`.

---

<a id="edgecases"></a>
## 8. Edge Cases and Additional Considerations

1. **Empty list (`head is None`)**  
   - `current_node_pointer` starts as `None`.  
   - Loop condition `while current_node_pointer is not None` is false immediately.  
   - Function returns `‑1`. Correct.

2. **Value not present**  
   - Traversal goes through all nodes, loop ends, returns `‑1`.

3. **Duplicate values**  
   - The function returns the index of the **first** occurrence because it returns as soon as a match is found.  
   - Example: `5 → 3 → 3 → 7`, search for `3` → returns `1` (not `2`).

4. **Very large list**  
   - Works with O(1) extra space; no stack overflow (iterative version). Recursive version would cause stack overflow for very long lists (Python recursion limit ~1000).

5. **Node value type**  
   - Code assumes `k` and `node.val` are comparable. Here both are integers.

---

