# Linked List – A Comprehensive Guide

A **linked list** is a linear data structure where elements, called **nodes**, are not stored in contiguous memory locations. Instead, each node contains a data field and a **pointer** (or **reference**) to the next node in the sequence. This pointer‑based linking allows dynamic memory allocation and efficient insertions/deletions compared to arrays.

---

## 1. The Node – The Building Block

At the lowest level, a node is a structure that holds two pieces of information:

- **Data**: The actual value (can be any type – integer, string, object, etc.).
- **Next**: A reference (or pointer) to the next node in the list. If there is no next node, this reference is set to `null` (or `None` in Python).

### Memory Perspective

In languages like C, a node is a `struct` containing the data and a pointer to another node. The pointer stores the memory address of the next node.  
In Python, every variable is a reference to an object; the `next` attribute simply holds a reference to another node object.

**Conceptual diagram of a single node:**

```
+---------+------+
|  data   | next |
+---------+------+
```

---

## 2. The Linked List Structure

A linked list is built by chaining such nodes together. Two special references are used:

- **Head**: A reference to the **first** node of the list. The list is accessed starting from the head.
- **Tail** (optional): A reference to the **last** node. Maintaining a tail makes appending at the end very fast.

The last node’s `next` is always `null` (or `None`), marking the end of the list.

**Diagram of a singly linked list with three nodes:**

```
 head
   ↓
+-------+    +-------+    +-------+    
| 10  | ●--->| 20  | ●--->| 30  | ●---> null
+-------+    +-------+    +-------+
```

- The arrows represent the `next` pointers.
- The head points to the first node (data 10).
- The first node’s next points to the second node (data 20).
- The second node’s next points to the third node (data 30).
- The third node’s next is `null`.

---

## 3. Types of Linked Lists

1. **Singly Linked List** – Each node has one `next` pointer. Traversal is only forward.
2. **Doubly Linked List** – Each node has two pointers: `next` (to the next node) and `prev` (to the previous node). Allows backward traversal.
3. **Circular Linked List** – The last node’s `next` points back to the first node, forming a circle. Can be singly or doubly circular.

We will focus on **singly linked list** for depth, and later briefly mention variations.

---

## 4. Core Operations (with ASCII Diagrams & Code)

We will implement a singly linked list in **Python** with detailed comments. Python’s object references perfectly model pointers without the complexity of manual memory management, making it ideal for learning the concepts.

### 4.1 Node Class

```python
class Node:
    """Represents a single node in the linked list."""
    def __init__(self, data):
        self.data = data   # The value stored in the node
        self.next = None   # Reference to the next node (initially None)
```

### 4.2 LinkedList Class

```python
class LinkedList:
    """Singly linked list implementation."""
    def __init__(self):
        self.head = None   # Start with an empty list: head is None
        self.tail = None   # Optional: keep track of the last node for efficiency
        self.size = 0      # Optional: keep track of the number of nodes
```

### 4.3 Insertion Operations

#### a) Insert at the Beginning (Prepend)

**Steps:**
1. Create a new node.
2. Set its `next` to the current head.
3. Update head to point to the new node.
4. If the list was empty, also set tail to the new node.

**Diagram before insertion (list: 20 → 30 → null):**

```
 head
   ↓
+-------+    +-------+    +-------+
| 20  | ●--->| 30  | ●--->| null |
+-------+    +-------+    +-------+
```

**After inserting 10 at beginning:**

```
 head
   ↓
+-------+    +-------+    +-------+    +-------+
| 10  | ●--->| 20  | ●--->| 30  | ●--->| null |
+-------+    +-------+    +-------+    +-------+
```

**Code:**

```python
def prepend(self, data):
    """Insert a new node at the beginning of the list."""
    new_node = Node(data)      # Step 1: create node
    new_node.next = self.head  # Step 2: link new node to old head
    self.head = new_node       # Step 3: head now points to new node
    if self.tail is None:      # If list was empty, tail also points to new node
        self.tail = new_node
    self.size += 1
```

#### b) Insert at the End (Append)

**Steps:**
1. Create a new node.
2. If the list is empty, set head and tail to the new node.
3. Otherwise, set the current tail’s `next` to the new node, and update tail to the new node.

**Diagram: list 10 → 20 → null, appending 30:**

Before:
```
 head
   ↓
+-------+    +-------+    +-------+
| 10  | ●--->| 20  | ●--->| null |
+-------+    +-------+    +-------+
               ↑
              tail
```

After:
```
 head
   ↓
+-------+    +-------+    +-------+
| 10  | ●--->| 20  | ●--->| 30  | ●---> null
+-------+    +-------+    +-------+
                            ↑
                           tail
```

**Code:**

```python
def append(self, data):
    """Insert a new node at the end of the list."""
    new_node = Node(data)
    if self.head is None:          # Empty list
        self.head = new_node
        self.tail = new_node
    else:
        self.tail.next = new_node  # Link the old tail to new node
        self.tail = new_node       # Update tail to new node
    self.size += 1
```

#### c) Insert at a Given Position (e.g., after a node with specific value)

**Steps:**
1. Traverse to find the node after which insertion should happen.
2. Create a new node.
3. Set new node’s next to the found node’s next.
4. Set found node’s next to new node.
5. If inserting after the tail, update tail.

**Diagram: insert 25 after node with data 20 in list 10 → 20 → 30 → null:**

Before:
```
 head
   ↓
+-------+    +-------+    +-------+    +-------+
| 10  | ●--->| 20  | ●--->| 30  | ●--->| null |
+-------+    +-------+    +-------+    +-------+
```

After:
```
 head
   ↓
+-------+    +-------+    +-------+    +-------+    +-------+
| 10  | ●--->| 20  | ●--->| 25  | ●--->| 30  | ●--->| null |
+-------+    +-------+    +-------+    +-------+    +-------+
```

**Code:**

```python
def insert_after(self, target_data, new_data):
    """Insert a new node after the first occurrence of target_data."""
    current = self.head
    while current and current.data != target_data:
        current = current.next
    if current is None:
        raise ValueError(f"Node with data {target_data} not found")
    new_node = Node(new_data)
    new_node.next = current.next
    current.next = new_node
    if current == self.tail:   # If we inserted after the tail, update tail
        self.tail = new_node
    self.size += 1
```

### 4.4 Deletion Operations

#### a) Delete from the Beginning

**Steps:**
1. If list is empty, do nothing.
2. Store the current head in a temporary variable.
3. Move head to head.next.
4. If the list becomes empty, set tail to None.
5. (Optional) Delete the old head (Python garbage collector handles it).

**Diagram: delete 10 from list 10 → 20 → 30 → null:**

Before:
```
 head
   ↓
+-------+    +-------+    +-------+    +-------+
| 10  | ●--->| 20  | ●--->| 30  | ●--->| null |
+-------+    +-------+    +-------+    +-------+
```

After:
```
 head
   ↓
+-------+    +-------+    +-------+
| 20  | ●--->| 30  | ●--->| null |
+-------+    +-------+    +-------+
```

**Code:**

```python
def delete_first(self):
    """Remove the first node of the list."""
    if self.head is None:
        return  # Empty list
    self.head = self.head.next
    if self.head is None:      # List became empty
        self.tail = None
    self.size -= 1
```

#### b) Delete from the End

**Steps:**
1. If list is empty, do nothing.
2. If only one node, set head and tail to None.
3. Otherwise, traverse to the second‑last node (node before tail).
4. Set its next to None.
5. Update tail to that node.

**Diagram: delete 30 from list 10 → 20 → 30 → null:**

Before:
```
 head
   ↓
+-------+    +-------+    +-------+    +-------+
| 10  | ●--->| 20  | ●--->| 30  | ●--->| null |
+-------+    +-------+    +-------+    +-------+
                            ↑
                           tail
```

After:
```
 head
   ↓
+-------+    +-------+    +-------+
| 10  | ●--->| 20  | ●--->| null |
+-------+    +-------+    +-------+
               ↑
              tail
```

**Code:**

```python
def delete_last(self):
    """Remove the last node of the list."""
    if self.head is None:
        return
    if self.head == self.tail:   # Only one node
        self.head = None
        self.tail = None
        self.size -= 1
        return
    # Traverse to the second‑last node
    current = self.head
    while current.next != self.tail:
        current = current.next
    current.next = None
    self.tail = current
    self.size -= 1
```

#### c) Delete by Value (first occurrence)

**Steps:**
1. Handle empty list.
2. If the node to delete is the head, use `delete_first`.
3. Otherwise, traverse keeping a `prev` pointer to the node before the one to delete.
4. Set `prev.next` to `current.next`.
5. If we are deleting the tail, update tail to `prev`.
6. (Optional) If value not found, raise error.

**Diagram: delete 20 from list 10 → 20 → 30 → null:**

Before:
```
 head
   ↓
+-------+    +-------+    +-------+    +-------+
| 10  | ●--->| 20  | ●--->| 30  | ●--->| null |
+-------+    +-------+    +-------+    +-------+
               ↑ (to delete)
```

After:
```
 head
   ↓
+-------+    +-------+    +-------+
| 10  | ●--->| 30  | ●--->| null |
+-------+    +-------+    +-------+
```

**Code:**

```python
def delete_value(self, value):
    """Delete the first node containing the given value."""
    if self.head is None:
        return
    # Special case: deleting the head
    if self.head.data == value:
        self.delete_first()
        return
    # General case
    prev = self.head
    current = self.head.next
    while current and current.data != value:
        prev = current
        current = current.next
    if current is None:   # Value not found
        raise ValueError(f"Value {value} not found in list")
    prev.next = current.next
    if current == self.tail:   # If we deleted the tail, update tail
        self.tail = prev
    self.size -= 1
```

### 4.5 Traversal and Searching

**Traversal** means visiting each node from head to tail, usually to print or process data.

```python
def display(self):
    """Print the entire list in a readable format."""
    elements = []
    current = self.head
    while current:
        elements.append(str(current.data))
        current = current.next
    print(" -> ".join(elements) + " -> null")
```

**Searching** for a value:

```python
def search(self, value):
    """Return True if value exists in the list, else False."""
    current = self.head
    while current:
        if current.data == value:
            return True
        current = current.next
    return False
```

### 4.6 Reversing a Linked List

Reversing is a classic operation. We change the direction of the `next` pointers so that the last node becomes the head.

**Algorithm (iterative):**
1. Initialize three pointers: `prev = None`, `current = head`, `next = None`.
2. While `current` is not None:
   - Store `next = current.next` (save the next node).
   - Reverse the link: `current.next = prev`.
   - Move `prev` to `current`, `current` to `next`.
3. After loop, set `head = prev` and update `tail` accordingly.

**Diagram: reversing list 10 → 20 → 30 → null**

Initial:
```
 head (current)
   ↓
+-------+    +-------+    +-------+
| 10  | ●--->| 20  | ●--->| 30  | ●---> null
+-------+    +-------+    +-------+
 prev = null
```

After one step (point 10's next to null):
```
 prev     current   next
   ↓        ↓        ↓
+-------+    +-------+    +-------+
| 10  | ●--->| 20  | ●--->| 30  | ●---> null
+-------+    +-------+    +-------+
  (now 10.next = null)
```

After loop ends, prev becomes new head:

```
 head (prev)
   ↓
+-------+    +-------+    +-------+
| 30  | ●--->| 20  | ●--->| 10  | ●---> null
+-------+    +-------+    +-------+
```

**Code:**

```python
def reverse(self):
    """Reverse the linked list in place."""
    prev = None
    current = self.head
    self.tail = current   # The old head becomes the new tail
    while current:
        next_node = current.next  # Save next
        current.next = prev       # Reverse link
        prev = current            # Move prev forward
        current = next_node       # Move current forward
    self.head = prev              # New head is the last node processed
```

---

## 5. Complete Example with All Methods

Below is a full, runnable Python implementation with extensive comments.

```python
class Node:
    """A node in a singly linked list."""
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    """Singly linked list with head and tail tracking."""
    def __init__(self):
        self.head = None
        self.tail = None
        self.size = 0

    def is_empty(self):
        """Check if the list is empty."""
        return self.head is None

    def prepend(self, data):
        """Insert at the beginning."""
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node
        if self.tail is None:    # First node
            self.tail = new_node
        self.size += 1

    def append(self, data):
        """Insert at the end."""
        new_node = Node(data)
        if self.head is None:    # Empty list
            self.head = new_node
            self.tail = new_node
        else:
            self.tail.next = new_node
            self.tail = new_node
        self.size += 1

    def insert_after(self, target_data, new_data):
        """Insert new_data after the first occurrence of target_data."""
        current = self.head
        while current and current.data != target_data:
            current = current.next
        if current is None:
            raise ValueError(f"Target {target_data} not found")
        new_node = Node(new_data)
        new_node.next = current.next
        current.next = new_node
        if current == self.tail:   # Inserting after tail updates tail
            self.tail = new_node
        self.size += 1

    def delete_first(self):
        """Delete the first node."""
        if self.head is None:
            return
        self.head = self.head.next
        if self.head is None:      # List became empty
            self.tail = None
        self.size -= 1

    def delete_last(self):
        """Delete the last node."""
        if self.head is None:
            return
        if self.head == self.tail:  # Only one node
            self.head = None
            self.tail = None
            self.size -= 1
            return
        # Traverse to second‑last node
        current = self.head
        while current.next != self.tail:
            current = current.next
        current.next = None
        self.tail = current
        self.size -= 1

    def delete_value(self, value):
        """Delete first node with given value."""
        if self.head is None:
            return
        if self.head.data == value:
            self.delete_first()
            return
        prev = self.head
        current = self.head.next
        while current and current.data != value:
            prev = current
            current = current.next
        if current is None:
            raise ValueError(f"Value {value} not found")
        prev.next = current.next
        if current == self.tail:
            self.tail = prev
        self.size -= 1

    def search(self, value):
        """Return True if value exists."""
        current = self.head
        while current:
            if current.data == value:
                return True
            current = current.next
        return False

    def reverse(self):
        """Reverse the list in place."""
        prev = None
        current = self.head
        self.tail = current   # Old head becomes new tail
        while current:
            next_node = current.next
            current.next = prev
            prev = current
            current = next_node
        self.head = prev

    def display(self):
        """Print the list."""
        elements = []
        current = self.head
        while current:
            elements.append(str(current.data))
            current = current.next
        print(" -> ".join(elements) + " -> null")

# Example usage
if __name__ == "__main__":
    ll = LinkedList()
    ll.append(10)
    ll.append(20)
    ll.append(30)
    ll.display()                # 10 -> 20 -> 30 -> null

    ll.prepend(5)
    ll.display()                # 5 -> 10 -> 20 -> 30 -> null

    ll.insert_after(20, 25)
    ll.display()                # 5 -> 10 -> 20 -> 25 -> 30 -> null

    ll.delete_value(20)
    ll.display()                # 5 -> 10 -> 25 -> 30 -> null

    ll.reverse()
    ll.display()                # 30 -> 25 -> 10 -> 5 -> null

    print("Size:", ll.size)     # Size: 4
    print("Search 10:", ll.search(10))   # True
    print("Search 99:", ll.search(99))   # False
```

---

## 6. Complexity Analysis

| Operation        | Time Complexity (Singly Linked List) | Explanation |
|------------------|--------------------------------------|-------------|
| Access (by index)| O(n)                                 | Must traverse from head. |
| Search (by value)| O(n)                                 | In worst case, traverse all nodes. |
| Insert at head   | O(1)                                 | Just update head pointer. |
| Insert at tail   | O(1) (with tail) / O(n) (without)    | With tail pointer, direct; without, need traversal. |
| Insert after given node | O(1) (if node reference is known) / O(n) (if need to find node) | Finding the node takes O(n); insertion itself is O(1). |
| Delete head      | O(1)                                 | Update head. |
| Delete tail      | O(n)                                 | Need to find second‑last node (unless doubly linked). |
| Delete by value  | O(n)                                 | Search + deletion. |
| Reverse          | O(n)                                 | Single pass through list. |

**Space Complexity:** O(n) for storing n nodes.

---

## 7. Advantages and Disadvantages

### Advantages
- **Dynamic size:** Memory is allocated as needed; no need to pre‑specify size.
- **Efficient insertions/deletions:** Especially at the beginning (O(1)), compared to arrays (O(n) for shifting).
- **Memory efficient for insertions:** No wasted space as in arrays (but each node has overhead for the pointer).

### Disadvantages
- **No random access:** You cannot directly access the i‑th element; you must traverse from head.
- **Extra memory per node:** Each node stores a pointer, which adds overhead (especially for small data).
- **Not cache‑friendly:** Nodes may be scattered in memory, reducing cache locality compared to arrays.
- **Traversal is only forward** (in singly linked list); backward traversal requires doubly linked list.

---

## 8. Variations

### Doubly Linked List
Each node has two pointers: `next` and `prev`. This allows traversal in both directions and makes deletion of a given node (without knowing its predecessor) O(1). However, it uses extra memory.

**Diagram:**

```
 null ←───┤ prev │ data │ next ├───→ ● ...
          └──────┴──────┴──────┘
```

### Circular Linked List
The last node’s `next` points back to the first node. Can be used for round‑robin scheduling, etc.

**Diagram (singly circular):**

```
 head
   ↓
+-------+    +-------+    +-------+
| 10  | ●--->| 20  | ●--->| 30  | ●--┐
+-------+    +-------+    +-------+   │
   ↑__________________________________┘
```

---

## 9. Conclusion

A linked list is a fundamental data structure that demonstrates dynamic memory allocation and pointer manipulation. Understanding it thoroughly—how nodes link, how head and tail are maintained, and how each operation rewires pointers—is essential for mastering more advanced structures like trees and graphs. The provided code and diagrams give you a solid foundation to experiment and build upon.