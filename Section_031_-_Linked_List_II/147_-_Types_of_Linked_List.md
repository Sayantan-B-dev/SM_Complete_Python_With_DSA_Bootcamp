## Linked Lists in Python

Linked lists are fundamental data structures where elements are stored in nodes. Each node contains data and one or more pointers (references) to other nodes. Based on the number of pointers and the way nodes are connected, linked lists can be classified into four main types:

- **Singly Linked List**
- **Doubly Linked List**
- **Circular Linked List** (singly circular)
- **Doubly Circular Linked List**

Below we discuss each type, their typical use cases, advantages, disadvantages, and provide a complete, well‑commented Python implementation for each.

---

### 1. Singly Linked List

A **singly linked list** consists of nodes that have a data field and a `next` pointer pointing to the next node in the sequence. The last node’s `next` is `None`. Traversal is only possible in one direction (from head to tail).

#### Use Cases
- When memory is limited (only one pointer per node).
- Implementing stacks, queues (with proper tail pointer), or adjacency lists for graphs.
- Simple applications where backward traversal is not required.

#### Advantages
- Simple and easy to implement.
- Less memory per node compared to doubly linked lists.

#### Disadvantages
- No backward traversal – to access a previous node you must start from the head.
- Deletion of a node requires knowledge of its predecessor (or a two‑pointer scan).
- Operations at the end (insert/delete) take O(n) if no tail pointer is maintained.

#### Python Implementation

```python
class Node:
    """Represents a node in a singly linked list."""
    def __init__(self, data):
        self.data = data   # holds the data
        self.next = None   # pointer to next node


class SinglyLinkedList:
    """Singly Linked List implementation."""
    def __init__(self):
        self.head = None   # initially the list is empty

    def insert_at_beginning(self, data):
        """Insert a new node at the beginning of the list."""
        new_node = Node(data)
        new_node.next = self.head   # new node points to the old head
        self.head = new_node        # head now points to new node

    def insert_at_end(self, data):
        """Insert a new node at the end of the list."""
        new_node = Node(data)
        if self.head is None:       # if list is empty
            self.head = new_node
            return
        # traverse to the last node
        current = self.head
        while current.next:
            current = current.next
        current.next = new_node     # last node now points to new node

    def delete_node(self, key):
        """Delete the first node containing the specified data (key)."""
        current = self.head

        # If head node itself holds the key
        if current and current.data == key:
            self.head = current.next   # move head to next node
            current = None             # free the node (optional in Python)
            return

        # Search for the node to be deleted, keep track of the previous node
        prev = None
        while current and current.data != key:
            prev = current
            current = current.next

        # If key was not present in the list
        if current is None:
            print(f"Key {key} not found.")
            return

        # Unlink the node from the list
        prev.next = current.next
        current = None   # free (optional)

    def display(self):
        """Print the entire list."""
        elements = []
        current = self.head
        while current:
            elements.append(str(current.data))
            current = current.next
        print(" -> ".join(elements) + " -> None")


# Example usage:
if __name__ == "__main__":
    sll = SinglyLinkedList()
    sll.insert_at_end(10)
    sll.insert_at_beginning(5)
    sll.insert_at_end(20)
    sll.display()          # 5 -> 10 -> 20 -> None
    sll.delete_node(10)
    sll.display()          # 5 -> 20 -> None
```

---

### 2. Doubly Linked List

A **doubly linked list** extends the singly linked list by adding a `prev` pointer in each node. This allows traversal in both forward and backward directions. The head’s `prev` is `None` and the tail’s `next` is `None`.

#### Use Cases
- Implementing LRU (Least Recently Used) caches.
- When backward traversal is frequently needed (e.g., in text editors for undo/redo).
- More efficient deletion/insertion of nodes when the node itself is known (no need to traverse from head to find predecessor).

#### Advantages
- Bidirectional traversal – forward and backward.
- Deletion of a node given only its reference is O(1) (since we have `prev` pointer).
- Easier to implement reverse iteration.

#### Disadvantages
- Extra memory for the `prev` pointer.
- More complex insertion and deletion operations (must update two pointers).

#### Python Implementation

```python
class DNode:
    """Represents a node in a doubly linked list."""
    def __init__(self, data):
        self.data = data
        self.next = None   # pointer to next node
        self.prev = None   # pointer to previous node


class DoublyLinkedList:
    """Doubly Linked List implementation."""
    def __init__(self):
        self.head = None
        self.tail = None   # optional: keep tail for efficient end operations

    def insert_at_beginning(self, data):
        """Insert a node at the beginning."""
        new_node = DNode(data)
        if self.head is None:          # empty list
            self.head = self.tail = new_node
        else:
            new_node.next = self.head
            self.head.prev = new_node
            self.head = new_node

    def insert_at_end(self, data):
        """Insert a node at the end."""
        new_node = DNode(data)
        if self.tail is None:          # empty list
            self.head = self.tail = new_node
        else:
            self.tail.next = new_node
            new_node.prev = self.tail
            self.tail = new_node

    def delete_node(self, key):
        """Delete the first node with the given data."""
        current = self.head

        # Traverse the list
        while current:
            if current.data == key:
                # If it's the head node
                if current == self.head:
                    self.head = current.next
                    if self.head:          # if new head exists
                        self.head.prev = None
                    else:                  # list becomes empty
                        self.tail = None
                # If it's the tail node
                elif current == self.tail:
                    self.tail = current.prev
                    self.tail.next = None
                else:                       # node in the middle
                    current.prev.next = current.next
                    current.next.prev = current.prev

                current = None   # free node (optional)
                return
            current = current.next

        print(f"Key {key} not found.")

    def display_forward(self):
        """Print list from head to tail."""
        elements = []
        current = self.head
        while current:
            elements.append(str(current.data))
            current = current.next
        print(" -> ".join(elements) + " -> None")

    def display_backward(self):
        """Print list from tail to head."""
        elements = []
        current = self.tail
        while current:
            elements.append(str(current.data))
            current = current.prev
        print(" <- ".join(elements) + " <- None")


# Example usage:
if __name__ == "__main__":
    dll = DoublyLinkedList()
    dll.insert_at_end(10)
    dll.insert_at_beginning(5)
    dll.insert_at_end(20)
    dll.display_forward()   # 5 -> 10 -> 20 -> None
    dll.display_backward()  # 20 <- 10 <- 5 <- None
    dll.delete_node(10)
    dll.display_forward()   # 5 -> 20 -> None
```

---

### 3. Circular Linked List (Singly Circular)

In a **circular linked list**, the last node points back to the first node instead of `None`. Thus the list forms a circle. It can be traversed starting from any node and eventually return to that node. This is typically implemented with a single pointer per node (singly circular).

#### Use Cases
- Round‑robin scheduling in operating systems.
- Repeatedly cycling through a list of items (e.g., playlist loop).
- Implementing a circular buffer.

#### Advantages
- No `None` references – you can traverse endlessly.
- Any node can be the starting point to traverse the whole list.
- Simpler for applications that require cyclic iteration.

#### Disadvantages
- Slightly more complex to handle the circular condition to avoid infinite loops.
- If not handled carefully, modifications (insert/delete) can break the circular structure.

#### Python Implementation

```python
class CNode:
    """Node for a circular linked list."""
    def __init__(self, data):
        self.data = data
        self.next = None


class CircularLinkedList:
    """Singly Circular Linked List implementation."""
    def __init__(self):
        self.head = None

    def insert_at_beginning(self, data):
        """Insert a node at the beginning of the circular list."""
        new_node = CNode(data)
        if self.head is None:
            # Only node: it points to itself
            new_node.next = new_node
            self.head = new_node
        else:
            # Find the last node
            last = self.head
            while last.next != self.head:
                last = last.next
            # new node points to current head
            new_node.next = self.head
            # last node now points to new node
            last.next = new_node
            # update head
            self.head = new_node

    def insert_at_end(self, data):
        """Insert a node at the end of the circular list."""
        new_node = CNode(data)
        if self.head is None:
            new_node.next = new_node
            self.head = new_node
        else:
            # Find the last node
            last = self.head
            while last.next != self.head:
                last = last.next
            # last node points to new node
            last.next = new_node
            # new node points back to head
            new_node.next = self.head

    def delete_node(self, key):
        """Delete the first node with the given data."""
        if self.head is None:
            print("List is empty.")
            return

        current = self.head
        prev = None

        # If head node itself holds the key
        if current.data == key:
            # If only one node
            if current.next == self.head:
                self.head = None
            else:
                # Find the last node
                last = self.head
                while last.next != self.head:
                    last = last.next
                # Update head and last node's next
                self.head = current.next
                last.next = self.head
            current = None
            return

        # Search for the key, stop when we loop back to head
        prev = current
        current = current.next
        while current != self.head:
            if current.data == key:
                prev.next = current.next
                current = None
                return
            prev = current
            current = current.next

        print(f"Key {key} not found.")

    def display(self):
        """Print the circular list (stop when we return to head)."""
        if self.head is None:
            print("Empty list")
            return
        elements = []
        current = self.head
        while True:
            elements.append(str(current.data))
            current = current.next
            if current == self.head:
                break
        print(" -> ".join(elements) + " -> (back to head)")


# Example usage:
if __name__ == "__main__":
    cll = CircularLinkedList()
    cll.insert_at_end(10)
    cll.insert_at_beginning(5)
    cll.insert_at_end(20)
    cll.display()           # 5 -> 10 -> 20 -> (back to head)
    cll.delete_node(10)
    cll.display()           # 5 -> 20 -> (back to head)
```

---

### 4. Doubly Circular Linked List

A **doubly circular linked list** combines the features of doubly linked and circular linked lists. Each node has `next` and `prev` pointers. The `next` of the last node points to the first node, and the `prev` of the first node points to the last node. This creates a fully circular structure with bidirectional traversal.

#### Use Cases
- Advanced navigation systems where you need to move both forward and backward in a circular manner.
- Implementing Fibonacci heaps.
- When you need constant‑time access to the head from the tail and vice versa.

#### Advantages
- All the benefits of doubly linked lists and circular lists:
  - Bidirectional traversal.
  - Circular iteration.
  - Both `head.prev` gives tail, `tail.next` gives head (no need to traverse).
- Insertion and deletion are symmetric and efficient.

#### Disadvantages
- Highest memory overhead (two pointers per node).
- Most complex to implement correctly (must update both pointers while maintaining circularity).

#### Python Implementation

```python
class DCNode:
    """Node for a doubly circular linked list."""
    def __init__(self, data):
        self.data = data
        self.next = None
        self.prev = None


class DoublyCircularLinkedList:
    """Doubly Circular Linked List implementation."""
    def __init__(self):
        self.head = None

    def insert_at_beginning(self, data):
        """Insert a node at the beginning."""
        new_node = DCNode(data)
        if self.head is None:
            # Only node: it points to itself both ways
            new_node.next = new_node
            new_node.prev = new_node
            self.head = new_node
        else:
            tail = self.head.prev      # tail is previous of head
            # new_node links
            new_node.next = self.head
            new_node.prev = tail
            # adjust surrounding nodes
            self.head.prev = new_node
            tail.next = new_node
            # update head
            self.head = new_node

    def insert_at_end(self, data):
        """Insert a node at the end."""
        new_node = DCNode(data)
        if self.head is None:
            new_node.next = new_node
            new_node.prev = new_node
            self.head = new_node
        else:
            tail = self.head.prev
            # new_node links
            new_node.next = self.head
            new_node.prev = tail
            # adjust tail and head
            tail.next = new_node
            self.head.prev = new_node

    def delete_node(self, key):
        """Delete the first node with the given data."""
        if self.head is None:
            print("List is empty.")
            return

        current = self.head

        # If head node holds the key
        if current.data == key:
            # If only one node
            if current.next == self.head:
                self.head = None
            else:
                tail = self.head.prev
                # Update head and close the circle
                self.head = current.next
                self.head.prev = tail
                tail.next = self.head
            current = None
            return

        # Search for the key, stop when we loop back to head
        current = current.next
        while current != self.head:
            if current.data == key:
                # Adjust neighbours
                current.prev.next = current.next
                current.next.prev = current.prev
                current = None
                return
            current = current.next

        print(f"Key {key} not found.")

    def display_forward(self):
        """Print list from head to tail (forward circular)."""
        if self.head is None:
            print("Empty list")
            return
        elements = []
        current = self.head
        while True:
            elements.append(str(current.data))
            current = current.next
            if current == self.head:
                break
        print(" -> ".join(elements) + " -> (back to head)")

    def display_backward(self):
        """Print list from tail to head (backward circular)."""
        if self.head is None:
            print("Empty list")
            return
        elements = []
        # start from tail (head.prev)
        current = self.head.prev
        while True:
            elements.append(str(current.data))
            current = current.prev
            if current == self.head.prev:   # when we return to tail
                break
        print(" <- ".join(elements) + " <- (back to tail)")


# Example usage:
if __name__ == "__main__":
    dcll = DoublyCircularLinkedList()
    dcll.insert_at_end(10)
    dcll.insert_at_beginning(5)
    dcll.insert_at_end(20)
    dcll.display_forward()   # 5 -> 10 -> 20 -> (back to head)
    dcll.display_backward()  # 20 <- 10 <- 5 <- (back to tail)
    dcll.delete_node(10)
    dcll.display_forward()   # 5 -> 20 -> (back to head)
```

---

## Summary

| Type                     | Use Cases                                           | Advantages                                   | Disadvantages                              |
|--------------------------|-----------------------------------------------------|----------------------------------------------|--------------------------------------------|
| **Singly Linked List**   | Simple stacks/queues, memory‑constrained systems    | Low memory, simple implementation            | No backward traversal, deletion costly     |
| **Doubly Linked List**   | LRU cache, undo/redo, bidirectional traversal       | Bidirectional, efficient deletion            | Extra memory for `prev` pointer            |
| **Circular Linked List** | Round‑robin scheduling, cyclic iteration            | No `None`, traverse from any node            | Slightly complex circular handling         |
| **Doubly Circular List** | Advanced navigation, Fibonacci heap                 | All benefits of doubly + circular            | Highest memory, most complex               |
