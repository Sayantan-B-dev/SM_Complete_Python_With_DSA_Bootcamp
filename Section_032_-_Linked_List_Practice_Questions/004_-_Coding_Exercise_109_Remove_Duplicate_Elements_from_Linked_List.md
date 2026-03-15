## Table of Contents

1. [Full Code with Detailed Comments](#code)  
2. [Layer‑by‑Layer Explanation](#layers)  
   2.1 [The ListNode Class – The Foundation](#listnode)  
   2.2 [The delete_duplicates Function – The Core Logic](#delete)  
3. [Algorithm – Linear Scan with Adjacent Comparison](#algorithm)  
4. [Step‑by‑Step Walkthrough with ASCII Diagrams](#walkthrough)  
   4.1 [Initial Setup](#initial)  
   4.2 [Processing First Pair of 1s](#step1)  
   4.3 [Processing Second 1 and 2](#step2)  
   4.4 [Processing 2 and First 3](#step3)  
   4.5 [Processing Pair of 3s](#step4)  
   4.6 [Processing 3 and 4](#step5)  
   4.7 [Processing Pair of 4s](#step6)  
   4.8 [Processing 4 and 5](#step7)  
   4.9 [Final List](#final)  
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
        # Value stored inside the node
        self.val = val
        
        # Reference to the next node in the linked list
        self.next = next


def delete_duplicates(head):
    """
    Removes duplicate values from a sorted singly linked list.

    Algorithm Explanation:
    Because the linked list is already sorted, duplicate values
    will always appear next to each other. The algorithm simply
    compares the current node with the next node. If both values
    are equal, the next node is skipped by updating the pointer.

    Parameters:
    head (ListNode): Head node of the sorted linked list

    Returns:
    ListNode: Head of the linked list after duplicates removal
    """

    # Pointer used to traverse the linked list
    current_node = head

    # Traverse until the end of the list is reached
    while current_node is not None and current_node.next is not None:

        # If the current node value equals the next node value
        if current_node.val == current_node.next.val:

            # Skip the duplicate node by adjusting the pointer
            current_node.next = current_node.next.next

        else:
            # Move forward only when nodes are different
            current_node = current_node.next

    # Return the original head which now represents the cleaned list
    return head
```

---

<a id="layers"></a>
## 2. Layer‑by‑Layer Explanation

We start from the most fundamental component and build up to the complete function.

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

<a id="delete"></a>
### 2.2 The delete_duplicates Function – The Core Logic

- **Header:** `def delete_duplicates(head):`  
  - `head` – reference to the first node of the sorted linked list (could be `None` for an empty list).  

- **Local Variable:**  
  - `current_node = head` – a pointer that will traverse the list.  

- **Loop:** `while current_node is not None and current_node.next is not None:`  
  - We check that both the current node and the next node exist. This ensures we can safely compare their values and also allows us to update pointers when skipping duplicates.  

- **Inside the Loop:**  
  - **If** `current_node.val == current_node.next.val`:  
    - This means the next node is a duplicate. We remove it by setting `current_node.next` to point to the node after the duplicate (`current_node.next.next`).  
    - **Important:** We do **not** advance `current_node` in this case. This is because the new `current_node.next` might also have the same value (if there were more than two duplicates). The loop will re‑check the new next node.  
  - **Else** (values differ):  
    - Move `current_node` forward to `current_node.next` (the next node, which is safe because it has a different value).  

- **Return Value:** `return head` – the original head pointer still points to the first node of the list (which might be unchanged, or if the head itself had duplicates, they were removed but the head remains the same because we never delete the current node, only nodes after it).  

**Control Flow Summary:**  

```
current = head
while current and current.next exist:
    if current.val == current.next.val:
        current.next = current.next.next   (skip the duplicate)
    else:
        current = current.next
return head
```

---

<a id="algorithm"></a>
## 3. Algorithm – Linear Scan with Adjacent Comparison

Because the input list is **sorted**, all duplicate values are guaranteed to be consecutive. This allows a simple single‑pass algorithm:

1. Start at the head.  
2. While there is a current node and a next node:  
   - Compare their values.  
   - If equal, bypass the next node (remove it) by pointing the current node directly to the node after the next.  
   - If not equal, move the current pointer to the next node (advance).  
3. When the loop ends, all duplicates have been removed, and the list remains sorted.  

This algorithm modifies the list in place and does not require extra memory.

---

<a id="walkthrough"></a>
## 4. Step‑by‑Step Walkthrough with ASCII Diagrams

We use the example from the expected output:  
**Original sorted list:** `1 → 1 → 2 → 3 → 3 → 4 → 4 → 5`  
**Expected result:** `1 → 2 → 3 → 4 → 5 →` (end)

We represent nodes as `[value]` and `⊥` for `None`. The current pointer is marked as `c`.

<a id="initial"></a>
### 4.1 Initial Setup

```
head ──→ [1]──→[1]──→[2]──→[3]──→[3]──→[4]──→[4]──→[5]──→⊥

c = head (points to first node [1])
```

<a id="step1"></a>
### 4.2 Processing First Pair of 1s

- `c.val` = 1, `c.next.val` = 1 → they are equal.  
- Remove the duplicate (the second `1`): set `c.next = c.next.next`.  
  The second `1` is now bypassed, and the first `1` now points directly to the node `[2]`.  

```
Before:
c
↓
[1]──→[1]──→[2]──→[3]──→[3]──→[4]──→[4]──→[5]──→⊥
       (to be removed)

After:
c
↓
[1]──────────→[2]──→[3]──→[3]──→[4]──→[4]──→[5]──→⊥
```

`c` still points to the first `[1]` (we did not advance). Now we check the new `c.next` which is `[2]`.

<a id="step2"></a>
### 4.3 Processing Second 1 and 2

- `c.val` = 1, `c.next.val` = 2 → they are different.  
- Advance `c` to `c.next` (now points to `[2]`).

```
c
↓
[1]──→[2]──→[3]──→[3]──→[4]──→[4]──→[5]──→⊥
```

<a id="step3"></a>
### 4.4 Processing 2 and First 3

- `c.val` = 2, `c.next.val` = 3 → different.  
- Advance `c` to `[3]`.

```
        c
        ↓
[1]──→[2]──→[3]──→[3]──→[4]──→[4]──→[5]──→⊥
```

<a id="step4"></a>
### 4.5 Processing Pair of 3s

- `c.val` = 3, `c.next.val` = 3 → equal.  
- Remove the duplicate (the second `3`): set `c.next = c.next.next`.  
  The first `3` now points directly to `[4]`.

```
Before:
        c
        ↓
[1]──→[2]──→[3]──→[3]──→[4]──→[4]──→[5]──→⊥
              (to be removed)

After:
        c
        ↓
[1]──→[2]──→[3]──────────→[4]──→[4]──→[5]──→⊥
```

`c` still points to the first `[3]`.

<a id="step5"></a>
### 4.6 Processing 3 and 4

- `c.val` = 3, `c.next.val` = 4 → different.  
- Advance `c` to `[4]`.

```
                c
                ↓
[1]──→[2]──→[3]──→[4]──→[4]──→[5]──→⊥
```

<a id="step6"></a>
### 4.7 Processing Pair of 4s

- `c.val` = 4, `c.next.val` = 4 → equal.  
- Remove the duplicate (the second `4`): set `c.next = c.next.next`.  
  The first `4` now points directly to `[5]`.

```
Before:
                c
                ↓
[1]──→[2]──→[3]──→[4]──→[4]──→[5]──→⊥
                      (to be removed)

After:
                c
                ↓
[1]──→[2]──→[3]──→[4]──────────→[5]──→⊥
```

`c` still points to the first `[4]`.

<a id="step7"></a>
### 4.8 Processing 4 and 5

- `c.val` = 4, `c.next.val` = 5 → different.  
- Advance `c` to `[5]`.

```
                        c
                        ↓
[1]──→[2]──→[3]──→[4]──→[5]──→⊥
```

<a id="final"></a>
### 4.9 Final List

Now `c.next` is `None`, so the loop condition fails. The list starting from `head` is:

```
[1]──→[2]──→[3]──→[4]──→[5]──→⊥
```

All duplicates have been removed. The function returns `head`, which still points to the first node `[1]`.

---

<a id="complexity"></a>
## 5. Complexity Analysis

- **Time Complexity:** O(n)  
  We traverse the list once, with each node examined at most once. Each deletion (pointer adjustment) is O(1).  

- **Space Complexity:** O(1)  
  Only a single pointer (`current_node`) is used, regardless of list size. The modifications are done in place, no extra data structures.

---

<a id="recursive"></a>
## 6. Recursive Alternative and Recursion Tree

Although the iterative solution is simple and efficient, we can also solve this problem recursively. This helps illustrate how recursion naturally handles linked list traversal and modification.

<a id="reccode"></a>
### 6.1 Recursive Implementation

```python
def delete_duplicates_recursive(head):
    # Base case: empty list or single node
    if head is None or head.next is None:
        return head
    
    # Recursively process the rest of the list
    head.next = delete_duplicates_recursive(head.next)
    
    # If current node's value equals the next node's value, skip the next node
    if head.val == head.next.val:
        return head.next   # skip current node (actually we skip the duplicate next node)
    else:
        return head        # keep current node
```

**How it works:**  
- Base case: if the list is empty or has only one node, return `head` (nothing to remove).  
- Recursive call: first, clean the tail (`head.next`) by recursively calling the function on it, and assign the result back to `head.next`. This ensures that the tail is already duplicate‑free.  
- Then, compare the current node’s value with the value of its **new** next node (which is the head of the cleaned tail).  
  - If they are equal, it means the current node is a duplicate of the next node. In a sorted list, this would imply that the current node is part of a duplicate pair. To remove the duplicate, we skip the current node and return `head.next` (the cleaned tail).  
  - If they are different, we keep the current node and return `head`.  

**Note:** This recursive implementation actually removes the **current** node when it duplicates the next, which is a slightly different perspective. In the sorted list, if `head.val == head.next.val`, then the current node is the first of the duplicates, and we want to keep only one – either the first or the second. The recursive version above keeps the **second** occurrence (the one from the cleaned tail) and discards the first. This also yields a correct duplicate‑free list, but the head might change if the original head is a duplicate. Let's test with our example.

<a id="rectree"></a>
### 6.2 Recursion Tree for the Example

List: `1 → 1 → 2 → 3 → 3 → 4 → 4 → 5`, remove duplicates.

We draw the recursion tree, where each call returns the new head of the processed sublist.

```
delete_duplicates_recursive(1)
│
├── head.next = delete_duplicates_recursive(1)
│   │
│   ├── head.next = delete_duplicates_recursive(2)
│   │   │
│   │   ├── head.next = delete_duplicates_recursive(3)
│   │   │   │
│   │   │   ├── head.next = delete_duplicates_recursive(3)
│   │   │   │   │
│   │   │   │   ├── head.next = delete_duplicates_recursive(4)
│   │   │   │   │   │
│   │   │   │   │   ├── head.next = delete_duplicates_recursive(4)
│   │   │   │   │   │   │
│   │   │   │   │   │   ├── head.next = delete_duplicates_recursive(5)
│   │   │   │   │   │   │   │
│   │   │   │   │   │   │   ├── head.next = delete_duplicates_recursive(None) → returns None
│   │   │   │   │   │   │   │
│   │   │   │   │   │   │   └── head.val == 5, head.next is None → return head (5)
│   │   │   │   │   │   │
│   │   │   │   │   │   └── head.val == 4, head.next.val == 5 → different → return head (4)
│   │   │   │   │   │
│   │   │   │   │   └── head.val == 4, head.next.val == 4 → equal → return head.next (which is 4 from the cleaned tail)
│   │   │   │   │
│   │   │   │   └── head.val == 3, head.next.val == 4 → different → return head (3)
│   │   │   │
│   │   │   └── head.val == 3, head.next.val == 3 → equal → return head.next (which is 3 from the cleaned tail)
│   │   │
│   │   └── head.val == 2, head.next.val == 3 → different → return head (2)
│   │
│   └── head.val == 1, head.next.val == 2 → different → return head (1)
│
└── head.val == 1, head.next.val == 1 → equal → return head.next (which is 1 from the cleaned tail)
```

Reading from the bottom up, the final returned head is the node that originally was the **second** `1` (value 1). So the resulting list is `1 → 2 → 3 → 4 → 5`, which is correct. Note that the head changed from the first `1` to the second `1`. This is acceptable because the function returns the new head.

**Recursion tree interpretation:**  
- The deepest call for `5` returns `5`.  
- The call for the second `4` gets `5`, compares `4` with `5`, keeps `4`, returns `4→5`.  
- The call for the first `4` gets `4→5`, compares `4` with `4` (equal), skips itself and returns `4→5`.  
- The call for the second `3` gets `4→5`, compares `3` with `4`, keeps `3`, returns `3→4→5`.  
- The call for the first `3` gets `3→4→5`, compares `3` with `3`, skips itself and returns `3→4→5`.  
- The call for `2` gets `3→4→5`, compares `2` with `3`, keeps `2`, returns `2→3→4→5`.  
- The call for the second `1` gets `2→3→4→5`, compares `1` with `2`, keeps `1`, returns `1→2→3→4→5`.  
- The call for the first `1` gets `1→2→3→4→5`, compares `1` with `1`, skips itself and returns `1→2→3→4→5`.  

Thus the final list is `1→2→3→4→5`, with the head being the second original `1`.

---

<a id="edgecases"></a>
## 7. Edge Cases and Additional Considerations

1. **Empty List (`head is None`)**  
   - Iterative: loop condition fails, returns `None`. Correct.  
   - Recursive: base case returns `None`.

2. **Single Node**  
   - Loop condition `current_node.next` is `None`, so loop never runs, returns the node unchanged. Correct.

3. **All Nodes Are the Same Value**  
   - Example: `5 → 5 → 5 → 5`  
   - Iterative: first comparison removes the second `5`, then current still at first `5`, new next is the third `5`, remove it, and so on, until only one `5` remains. Works.

4. **No Duplicates**  
   - The loop will simply advance through all nodes without any deletions, returning the original list.

5. **Duplicates at the End**  
   - Example: `1 → 2 → 3 → 3`  
   - The last pair will be handled correctly: when `current` is at the first `3`, `current.next` is the second `3`, they are equal, so `current.next` is set to `None`, removing the last duplicate. The loop then ends because `current.next` is `None`.

6. **Large Lists**  
   - Iterative version uses O(1) extra space and is safe. Recursive version may cause stack overflow for very long lists (Python recursion limit ~1000).

---
