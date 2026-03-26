# Singly Linked List for Hash Map Chaining

## 1. Definition and Concept Overview

A singly linked list is a linear data structure where each element (node) contains a key-value pair and a reference to the next node. In the context of a hash map using separate chaining, each bucket of the hash table holds a linked list. Collisions—where multiple keys hash to the same index—are resolved by storing all colliding entries in the same list.

The implementation defines two classes: `LLNode` representing a single entry and `LinkedList` managing the chain. The list supports insertion at the head (O(1)), search by key (O(n)), and deletion by key (O(n)). No ordering is maintained beyond the sequence of insertions.

## 2. Core Principles and Internal Mechanics

- **Node Structure**: Each node stores a `key`, a `value`, and a `next` pointer to the subsequent node.
- **Head Pointer**: The list maintains a reference (`self.head`) to the first node. An empty list is indicated by `head is None`.
- **Insertion**: New nodes are added at the front to avoid traversing the list. This makes insertion O(1) but results in a LIFO order.
- **Search**: Traversal from head until either the key matches or the end is reached. Returns the node or `None`.
- **Deletion**: Requires keeping a `previous` pointer to reconnect the list when the target node is removed. Two cases: removing the head or removing an internal/tail node.
- **Traversal**: Used for debugging; iterates over all nodes and prints key-value pairs.

## 3. Step-by-Step Logical Breakdown

### 3.1 Insertion (`add`)
1. Create a new `LLNode` with the given key and value.
2. Set the new node’s `next` to the current `head`.
3. Update `self.head` to the new node.

### 3.2 Search (`search`)
1. Initialize `current = self.head`.
2. While `current` is not `None`:
   - If `current.key == key`, return `current`.
   - Otherwise, advance `current = current.next`.
3. Return `None` (key not found).

### 3.3 Deletion (`delete`)
1. Set `current = self.head`, `previous = None`.
2. While `current` is not `None`:
   - If `current.key == key`:
     - If `previous is None` → the target is the head. Set `self.head = current.next`.
     - Else → bypass the node: `previous.next = current.next`.
     - Return `True`.
   - Else, advance: `previous = current`, `current = current.next`.
3. Return `False` (key not found).

### 3.4 Traversal (`traverse`)
1. Set `current = self.head`.
2. While `current` is not `None`:
   - Print `current.key: current.value --> `.
   - Advance `current = current.next`.
3. Print `None` to indicate list termination.

## 4. Implementation

```python
class LLNode:
    """Node in a singly linked list storing a key-value pair."""
    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.next = None

class LinkedList:
    """
    Singly linked list used as a collision bucket in a hash map.
    Operations: add (head insertion), search, delete, traverse.
    """
    def __init__(self):
        self.head = None

    def add(self, key, value):
        """
        Insert a new key-value pair at the front of the list.
        Time: O(1)
        """
        new_node = LLNode(key, value)
        new_node.next = self.head   # Link to existing head
        self.head = new_node        # Update head to new node

    def search(self, key):
        """
        Return the node with the given key, or None if not found.
        Time: O(n) where n is the number of nodes in this list.
        """
        current = self.head
        while current:
            if current.key == key:
                return current
            current = current.next
        return None

    def delete(self, key):
        """
        Remove the node with the specified key.
        Return True if successful, False otherwise.
        Time: O(n)
        """
        current = self.head
        previous = None

        while current:
            if current.key == key:
                if previous is None:          # Removing head
                    self.head = current.next
                else:                         # Removing internal or tail node
                    previous.next = current.next
                return True
            previous = current
            current = current.next
        return False

    def traverse(self):
        """
        Print all nodes from head to tail.
        Used for debugging.
        """
        current = self.head
        while current:
            print(f"{current.key}: {current.value} --> ", end=" ")
            current = current.next
        print("None")
```

## 5. Time and Space Complexity

| Operation | Time Complexity | Space Complexity |
|-----------|-----------------|------------------|
| `add`     | O(1)            | O(1)             |
| `search`  | O(n)            | O(1)             |
| `delete`  | O(n)            | O(1)             |
| `traverse`| O(n)            | O(1)             |

- **n**: number of nodes in the linked list (i.e., size of the bucket).
- Space overhead: each node stores two references (`key`, `value`) and a `next` pointer; the list itself stores only the head reference.

## 6. Edge Cases and Failure Scenarios

- **Empty list**:
  - `search` returns `None`.
  - `delete` returns `False`.
  - `traverse` prints only `None`.
- **Deleting the only node**:
  - `previous is None` and `current.next is None`. Setting `self.head = None` correctly empties the list.
- **Deleting a non‑existent key**:
  - The loop exhausts without finding the key; returns `False`. No modification occurs.
- **Duplicate keys**:
  - The implementation does not prevent duplicate keys. Insertion adds a new node at the front, leaving older nodes with the same key in the list. `search` returns the first encountered (the most recently added). `delete` removes the first occurrence encountered during traversal. If duplicates exist, only one is removed per call.
- **Large lists**:
  - Operations scale linearly with bucket size. In a well‑balanced hash map, bucket sizes remain small, so linear overhead is acceptable.

## 7. Practical Use Cases

- **Hash map with separate chaining**: The primary intended use. Each bucket maintains a linked list of entries whose keys hash to the same index.
- **Simple cache with LRU semantics** (if extended with a tail pointer and deletion from any position).
- **Undo/redo stack** when order is not critical.
- **Memory pools or freelists** where nodes are added and removed from the front for O(1) operations.

## 8. Limitations and Trade-offs

- **No tail pointer**: Operations that require appending to the end would be O(n) unless the list is modified to maintain a tail reference.
- **No prevention of duplicate keys**: The calling code (e.g., the hash map) must enforce key uniqueness if desired.
- **Forward-only traversal**: Cannot traverse backwards, making certain operations (e.g., deletion from the end with only a reference to the node) impossible without additional data structures.
- **Worst‑case performance**: If the hash function distributes keys poorly, a single bucket can accumulate many nodes, causing O(n) per operation and degrading the hash map to O(n) average.
- **Memory overhead**: Each node carries an extra reference (`next`) that may be significant for very large datasets compared to a compact array‑based alternative.