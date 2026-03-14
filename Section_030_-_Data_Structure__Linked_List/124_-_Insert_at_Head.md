# Insertion at Head in Linked Lists

## 1. Definition and Concept Overview

Insertion at the **head** refers to adding a new node at the beginning of a linked list so that the new node becomes the first element of the structure. This operation modifies the list’s entry point because the **head pointer must be updated** to reference the newly inserted node.

The insertion operation differs slightly depending on the list structure.

| Linked List Type     | Structural Property                               |
| -------------------- | ------------------------------------------------- |
| Singly Linked List   | Node contains `data` and `next` pointer           |
| Doubly Linked List   | Node contains `data`, `next`, and `prev` pointers |
| Circular Linked List | Last node connects back to the head               |

Insertion at the head is typically **O(1)** because it only requires pointer manipulation without traversal.

---

# 2. Insertion in Singly Linked List

## Core Mechanics

A singly linked list node contains:

| Field  | Purpose             |
| ------ | ------------------- |
| `data` | Stores value        |
| `next` | Points to next node |

### Initial Structure

```
head
 |
 v
10 -> 20 -> 30 -> None
```

### After Inserting `5` at Head

```
head
 |
 v
5 -> 10 -> 20 -> 30 -> None
```

---

## Step-by-Step Logical Breakdown

1. Create a new node containing the desired value.
2. Assign the new node's `next` pointer to the current head.
3. Update the head pointer to the new node.

---

## Python Implementation

```python
class Node:
    """
    Node structure for a singly linked list.
    Stores a value and a reference to the next node.
    """

    def __init__(self, value):

        # Store the value inside the node
        self.data = value

        # Reference to the next node
        self.next = None


def insert_at_head(head, value):
    """
    Inserts a new node at the beginning of the linked list.
    Returns the updated head pointer.
    """

    # Create a node containing the provided value
    new_node = Node(value)

    # Link new node to the current head
    new_node.next = head

    # Update head pointer to the new node
    head = new_node

    return head
```

### Expected Output

```
Initial List
10 -> 20 -> 30

After insertion at head
5 -> 10 -> 20 -> 30
```

---

# 3. Insertion in Doubly Linked List

## Core Mechanics

A doubly linked list node stores two directional pointers.

| Field  | Purpose                 |
| ------ | ----------------------- |
| `data` | Node value              |
| `next` | Next node reference     |
| `prev` | Previous node reference |

### Initial Structure

```
None <- 10 <-> 20 <-> 30 -> None
          ^
         head
```

### After Inserting `5`

```
None <- 5 <-> 10 <-> 20 <-> 30 -> None
         ^
        head
```

---

## Step-by-Step Logical Breakdown

1. Create a new node.
2. Set `new_node.next = head`.
3. Set `head.prev = new_node`.
4. Update head pointer to new node.

---

## Python Implementation

```python
class DoublyNode:
    """
    Node for a doubly linked list.
    Stores references to both previous and next nodes.
    """

    def __init__(self, value):

        # Data stored in the node
        self.data = value

        # Pointer to next node
        self.next = None

        # Pointer to previous node
        self.prev = None


def insert_head_doubly(head, value):
    """
    Inserts a node at the beginning of a doubly linked list.
    """

    # Create a new node
    new_node = DoublyNode(value)

    # If list already contains nodes
    if head is not None:

        # Connect new node with current head
        new_node.next = head

        # Update backward link of previous head
        head.prev = new_node

    # New node becomes the head
    head = new_node

    return head
```

### Expected Output

```
Initial
10 <-> 20 <-> 30

After insertion
5 <-> 10 <-> 20 <-> 30
```

---

# 4. Insertion in Circular Linked List

## Core Mechanics

A circular linked list has no `None` pointer at the end.

The last node connects back to the head.

### Initial Structure

```
      +------------------+
      |                  |
      v                  |
10 -> 20 -> 30 ----------
^
|
head
```

### After Inserting `5`

```
      +----------------------+
      |                      |
      v                      |
5 -> 10 -> 20 -> 30 ---------
^
|
head
```

---

## Step-by-Step Logical Breakdown

1. Create a new node.
2. If list is empty:

   * Make the node point to itself.
3. Otherwise:

   * Traverse to the last node.
   * Set last node's `next` to the new node.
   * Set new node's `next` to the old head.
4. Update head pointer.

---

## Python Implementation

```python
class CircularNode:
    """
    Node used in circular linked lists.
    """

    def __init__(self, value):

        # Store node value
        self.data = value

        # Pointer to next node
        self.next = None


def insert_head_circular(head, value):
    """
    Inserts a node at the beginning of a circular linked list.
    """

    new_node = CircularNode(value)

    # Case 1: Empty list
    if head is None:

        # Node points to itself
        new_node.next = new_node
        return new_node

    # Traverse to locate the last node
    temp = head
    while temp.next != head:
        temp = temp.next

    # Connect last node to the new node
    temp.next = new_node

    # New node points to previous head
    new_node.next = head

    # Update head
    head = new_node

    return head
```

### Expected Output

```
Initial Circular List
10 -> 20 -> 30 -> (back to 10)

After insertion
5 -> 10 -> 20 -> 30 -> (back to 5)
```

---

# 5. Time and Space Complexity Analysis

| List Type            | Time Complexity | Reason                    |
| -------------------- | --------------- | ------------------------- |
| Singly Linked List   | O(1)            | Direct pointer update     |
| Doubly Linked List   | O(1)            | No traversal required     |
| Circular Linked List | O(n)            | Last node must be located |

Space complexity remains constant.

| Metric           | Complexity |
| ---------------- | ---------- |
| Auxiliary Memory | O(1)       |

---

# 6. Edge Cases and Failure Scenarios

### Empty Linked List

Insertion must initialize the list correctly.

```
head = None
```

After insertion:

```
head -> newNode
```

---

### Single Node Circular List

Improper pointer updates may break circular connectivity.

Correct structure:

```
node.next = node
```

---

### Missing Pointer Updates in Doubly Linked List

Failing to update `prev` pointer corrupts backward traversal.

Example faulty structure:

```
5 -> 10 <-> 20
```

Backward traversal becomes invalid.

---

# 7. Practical Use Cases

| Domain            | Application                 |
| ----------------- | --------------------------- |
| Operating Systems | Task scheduling queues      |
| Memory Allocation | Free list structures        |
| Networking        | Packet buffering            |
| Game Engines      | Circular player rotation    |
| Databases         | Page replacement strategies |

Insertion at head is especially useful for **stack-like behaviors** and **constant-time updates**.

---

# 8. Limitations and Trade-offs

| Structure            | Limitation                                      |
| -------------------- | ----------------------------------------------- |
| Singly Linked List   | No backward traversal                           |
| Doubly Linked List   | Extra memory overhead                           |
| Circular Linked List | Traversal termination conditions become complex |

