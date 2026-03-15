## Table of Contents

1. [Full Code with Detailed Comments](#code)  
2. [Layer‑by‑Layer Explanation](#layers)  
   2.1 [The ListNode Class – The Foundation](#listnode)  
   2.2 [The reverse_list Function – The Core Logic](#reverse)  
3. [Algorithm – Iterative Pointer Reversal](#algorithm)  
4. [Step‑by‑Step Walkthrough with ASCII Diagrams](#walkthrough)  
   4.1 [Initial Setup](#initial)  
   4.2 [Iteration 1 – Reversing First Node](#iter1)  
   4.3 [Iteration 2 – Reversing Second Node](#iter2)  
   4.4 [Iteration 3 – Reversing Third Node](#iter3)  
   4.5 [Iteration 4 – Reversing Fourth Node](#iter4)  
   4.6 [Iteration 5 – Reversing Fifth Node](#iter5)  
   4.7 [Final State](#final)  
5. [Complexity Analysis](#complexity)  
6. [Recursive Alternative and Recursion Tree](#recursive)  
   6.1 [Recursive Implementation](#reccode)  
   6.2 [Recursion Tree for the Example](#rectree)  
7. [Edge Cases and Additional Considerations](#edgecases)  

---

<a id="code"></a>
## 1. Full Code with Detailed Comments

```python
# Definition for singly linked list node.
class ListNode:
    def __init__(self, val=0, next=None):
        # Store the value contained in this node
        self.val = val
        
        # Reference pointing to the next node in the linked list
        self.next = next


def reverse_list(head):
    """
    Reverses a singly linked list using an iterative pointer manipulation method.

    Algorithm Explanation:
    Three pointers are maintained during traversal. The previous pointer keeps
    track of the already reversed portion of the list, the current pointer
    traverses the list, and a temporary pointer stores the next node so that
    the remaining list is not lost when links are reversed.

    Parameters:
    head (ListNode): Starting node of the singly linked list

    Returns:
    ListNode: New head node after the linked list has been reversed
    """

    # This pointer will represent the head of the reversed list
    previous_node = None

    # This pointer traverses the original linked list
    current_node = head

    # Continue traversal until the end of the linked list is reached
    while current_node is not None:

        # Temporarily store the next node so that the remaining list is preserved
        next_node_reference = current_node.next

        # Reverse the direction of the current node's pointer
        current_node.next = previous_node

        # Move the previous pointer one step forward
        previous_node = current_node

        # Move the current pointer forward using the stored reference
        current_node = next_node_reference

    # When traversal completes, previous_node becomes the new head
    return previous_node
```

---

<a id="layers"></a>
## 2. Layer‑by‑Layer Explanation

We start from the most fundamental building block and work our way up.

<a id="listnode"></a>
### 2.1 The ListNode Class – The Foundation

- **Purpose:** The `ListNode` class defines the structure of each element in a singly linked list.  
- **Constructor `__init__`:**  
  - `val=0` – default value, but we usually pass an integer when creating a node.  
  - `next=None` – initially the node does not point to any other node (end of list).  
- **Instance Variables:**  
  - `self.val` – holds the integer data of the node.  
  - `self.next` – holds a reference (pointer) to the next `ListNode` object, or `None` if there is no next node.  

**Visual Representation of a Single Node:**  

```
+-------+------+
| val   | next |
+-------+------+
|   3   |  o-----> (next node or None)
+-------+------+
```

The arrow indicates that `next` points to another node or terminates at `None` (often drawn as ⊥).

<a id="reverse"></a>
### 2.2 The reverse_list Function – The Core Logic

- **Header:** `def reverse_list(head):`  
  - `head` – reference to the first node of the list (could be `None` for an empty list).  

- **Local Variables:**  
  - `previous_node = None` – initially, the reversed part is empty, so this pointer is `None`. It will eventually become the new head after reversal.  
  - `current_node = head` – starts at the head and will move through the original list.  

- **Loop:** `while current_node is not None:`  
  - We process each node until we reach the end.  

- **Inside the Loop (for each node):**  
  1. `next_node_reference = current_node.next` – store the next node before we change the current node’s pointer. This preserves the remainder of the list.  
  2. `current_node.next = previous_node` – reverse the link: make the current node point to the previous node (which is the head of the already reversed part).  
  3. `previous_node = current_node` – move the `previous_node` pointer forward to include the current node in the reversed part.  
  4. `current_node = next_node_reference` – advance `current_node` to the original next node (the stored reference) to continue traversal.  

- **Return Value:** `return previous_node` – after the loop, `previous_node` points to the last node we processed, which is the original tail, now the new head of the reversed list.  

**Control Flow Summary:**  

```
prev = None
curr = head
while curr exists:
    next_temp = curr.next   (save next)
    curr.next = prev        (reverse link)
    prev = curr             (move prev forward)
    curr = next_temp        (move curr forward)
return prev
```

---

<a id="algorithm"></a>
## 3. Algorithm – Iterative Pointer Reversal

This is a classic in‑place reversal of a singly linked list.  

- **Key Idea:** We traverse the list once, and at each step we redirect the current node’s `next` pointer to point to the previous node (the node we just processed). This gradually builds the reversed list from the tail backwards.  
- **Why Three Pointers?**  
  - `previous_node` keeps track of the already reversed segment.  
  - `current_node` points to the node we are currently processing.  
  - `next_node_reference` saves the original next node so we don’t lose the rest of the list after we change the current node’s pointer.  

- **Invariants:**  
  - Before each iteration, the list starting from `current_node` is the unreversed remainder.  
  - After each iteration, the node `current_node` (now processed) becomes the new head of the reversed part, and `previous_node` points to it.  

- **Termination:** When `current_node` becomes `None`, we have processed all nodes, and `previous_node` points to the new head (the original tail).

---

<a id="walkthrough"></a>
## 4. Step‑by‑Step Walkthrough with ASCII Diagrams

We use the example from the expected output:  
**Original list:** `1 → 2 → 3 → 4 → 5`  
**Expected result:** `5 → 4 → 3 → 2 → 1 →` (end)

We represent nodes as `[value]` and `⊥` for `None`. The pointers are marked:  
- `p` = `previous_node`  
- `c` = `current_node`  
- `n` = `next_node_reference` (shown only when needed)

<a id="initial"></a>
### 4.1 Initial Setup

```
head ──→ [1]──→[2]──→[3]──→[4]──→[5]──→⊥

p = None
c = head (points to [1])
```

<a id="iter1"></a>
### 4.2 Iteration 1 – Reversing First Node

- `next_node_reference = c.next` → store `[2]`.
- `c.next = p` → now `[1]` points to `None` (was pointing to `[2]`).
- `p = c` → `p` now points to `[1]`.
- `c = next_node_reference` → `c` now points to `[2]`.

Diagram after iteration 1:

```
p ──→ [1]──→⊥

c ──→ [2]──→[3]──→[4]──→[5]──→⊥
```

The list is now split into two parts: reversed part `[1]` and remaining `[2]→[3]→[4]→[5]`.

<a id="iter2"></a>
### 4.3 Iteration 2 – Reversing Second Node

- `next_node_reference = c.next` → store `[3]`.
- `c.next = p` → `[2]` now points to `[1]` (instead of `[3]`).
- `p = c` → `p` now points to `[2]`.
- `c = next_node_reference` → `c` now points to `[3]`.

Diagram after iteration 2:

```
p ──→ [2]──→[1]──→⊥

c ──→ [3]──→[4]──→[5]──→⊥
```

Reversed part: `2→1`. Remaining: `3→4→5`.

<a id="iter3"></a>
### 4.4 Iteration 3 – Reversing Third Node

- `next_node_reference = c.next` → store `[4]`.
- `c.next = p` → `[3]` now points to `[2]`.
- `p = c` → `p` now points to `[3]`.
- `c = next_node_reference` → `c` now points to `[4]`.

Diagram after iteration 3:

```
p ──→ [3]──→[2]──→[1]──→⊥

c ──→ [4]──→[5]──→⊥
```

Reversed part: `3→2→1`. Remaining: `4→5`.

<a id="iter4"></a>
### 4.5 Iteration 4 – Reversing Fourth Node

- `next_node_reference = c.next` → store `[5]`.
- `c.next = p` → `[4]` now points to `[3]`.
- `p = c` → `p` now points to `[4]`.
- `c = next_node_reference` → `c` now points to `[5]`.

Diagram after iteration 4:

```
p ──→ [4]──→[3]──→[2]──→[1]──→⊥

c ──→ [5]──→⊥
```

Reversed part: `4→3→2→1`. Remaining: `5`.

<a id="iter5"></a>
### 4.6 Iteration 5 – Reversing Fifth Node

- `next_node_reference = c.next` → store `None` (since `c.next` is `None`).
- `c.next = p` → `[5]` now points to `[4]`.
- `p = c` → `p` now points to `[5]`.
- `c = next_node_reference` → `c` becomes `None`.

Diagram after iteration 5:

```
p ──→ [5]──→[4]──→[3]──→[2]──→[1]──→⊥

c = None
```

<a id="final"></a>
### 4.7 Final State

The loop ends because `c` is `None`. `previous_node` (now `p`) points to node `[5]`. We return `p`, which is the new head.

**Resulting list:** `5 → 4 → 3 → 2 → 1 → ⊥`

---

<a id="complexity"></a>
## 5. Complexity Analysis

- **Time Complexity:** O(n)  
  We traverse each node exactly once, performing a constant number of operations per node.  

- **Space Complexity:** O(1)  
  Only three pointers are used (`previous_node`, `current_node`, `next_node_reference`), independent of the list size. The reversal is done in place, without allocating any new nodes.

---

<a id="recursive"></a>
## 6. Recursive Alternative and Recursion Tree

Although the iterative approach is efficient and easy to understand, we can also reverse a linked list recursively. This helps illustrate how recursion can build the reversed list from the tail backward.

<a id="reccode"></a>
### 6.1 Recursive Implementation

```python
def reverse_list_recursive(head):
    # Base case: empty list or single node
    if head is None or head.next is None:
        return head
    
    # Recursively reverse the rest of the list (starting from head.next)
    new_head = reverse_list_recursive(head.next)
    
    # After reversing the rest, head.next is now the last node of the reversed part
    # Make that node point back to head
    head.next.next = head
    head.next = None   # Set head's next to None (it becomes the new tail)
    
    return new_head
```

**How it works:**  
- Base case: if the list is empty or has one node, it is already reversed; return `head`.  
- Recursive step: assume that calling `reverse_list_recursive(head.next)` correctly reverses the sublist starting from the second node and returns the new head of that reversed sublist (which will be the original tail).  
- After the recursive call, `head.next` is now the **last node** of the reversed sublist (because originally `head.next` pointed to the second node, and after reversal that second node becomes the tail of the reversed sublist).  
- To attach `head` to the end of the reversed sublist, we set `head.next.next = head`. This makes the last node (originally the second node) point back to `head`.  
- Then set `head.next = None` to make `head` the new tail.  
- Return `new_head` (the original tail), which is now the head of the fully reversed list.

<a id="rectree"></a>
### 6.2 Recursion Tree for the Example

List: `1 → 2 → 3 → 4 → 5`

We draw the recursion tree, where each call returns the new head of the reversed sublist.

```
reverse_list_recursive(1)
│
├── head.next = 2, so recursive call: reverse_list_recursive(2)
│   │
│   ├── head.next = 3, call reverse_list_recursive(3)
│   │   │
│   │   ├── head.next = 4, call reverse_list_recursive(4)
│   │   │   │
│   │   │   ├── head.next = 5, call reverse_list_recursive(5)
│   │   │   │   │
│   │   │   │   ├── head.next = None → base case, returns 5
│   │   │   │   │
│   │   │   │   └── after recursive call, new_head = 5
│   │   │   │       head = 4, head.next = 5
│   │   │   │       set head.next.next = head → 5.next = 4
│   │   │   │       set head.next = None → 4.next = None
│   │   │   │       return new_head = 5
│   │   │   │
│   │   │   └── returns 5 to previous call (for node 3)
│   │   │
│   │   ├── after recursive call, new_head = 5 (from node 4's return)
│   │   │   head = 3, head.next = 4
│   │   │   set head.next.next = head → 4.next = 3
│   │   │   set head.next = None → 3.next = None
│   │   │   return new_head = 5
│   │   │
│   │   └── returns 5 to previous call (for node 2)
│   │
│   ├── after recursive call, new_head = 5 (from node 3's return)
│   │   head = 2, head.next = 3
│   │   set head.next.next = head → 3.next = 2
│   │   set head.next = None → 2.next = None
│   │   return new_head = 5
│   │
│   └── returns 5 to previous call (for node 1)
│
└── after recursive call, new_head = 5 (from node 2's return)
    head = 1, head.next = 2
    set head.next.next = head → 2.next = 1
    set head.next = None → 1.next = None
    return new_head = 5
```

The recursion unwinds, and each step reverses one more link. The final returned `new_head` is `5`, and the list becomes `5 → 4 → 3 → 2 → 1`.

**Visualizing the reversal at each level:**  

- Level 5: returns `5` (no change)  
- Level 4: `4 → 5` becomes `5 → 4`  
- Level 3: `3 → (5 → 4)` becomes `5 → 4 → 3`  
- Level 2: `2 → (5 → 4 → 3)` becomes `5 → 4 → 3 → 2`  
- Level 1: `1 → (5 → 4 → 3 → 2)` becomes `5 → 4 → 3 → 2 → 1`

---

<a id="edgecases"></a>
## 7. Edge Cases and Additional Considerations

1. **Empty List (`head is None`)**  
   - Iterative: `current_node` is `None`, loop never runs, returns `previous_node` (which is `None`). Correct.  
   - Recursive: base case returns `None`.

2. **Single Node List**  
   - Iterative: `current_node` exists, but `current_node.next` is `None`. Loop runs once:  
     - `next_node_reference = None`  
     - `current_node.next = None` (already `None`)  
     - `previous_node = current_node`  
     - `current_node = None`  
     - Returns `previous_node` (the single node). Correct.  
   - Recursive: base case returns the node itself.

3. **Two Nodes**  
   - List: `1 → 2`  
   - Iterative:  
     - Iter1: `next = 2`, `1.next = None`, `prev = 1`, `curr = 2`  
     - Iter2: `next = None`, `2.next = 1`, `prev = 2`, `curr = None`  
     - Returns `2`, list becomes `2 → 1`. Correct.  
   - Recursive:  
     - `reverse_list_recursive(1)` calls `reverse_list_recursive(2)` (base) → returns `2`  
     - Then `head.next.next = head` → `2.next = 1`, `head.next = None` → `1.next = None`  
     - Returns `2`. Correct.

4. **Large Lists**  
   - Iterative: O(1) extra space, safe for any size.  
   - Recursive: may cause stack overflow for very long lists (Python recursion limit ~1000). For production, iterative is preferred.

5. **List with Duplicates**  
   - The reversal algorithm works regardless of duplicate values because it only manipulates pointers, not values.

---
