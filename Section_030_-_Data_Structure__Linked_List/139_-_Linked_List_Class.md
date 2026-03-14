```py
class Node:
    def __init__(self, data):
        # Store value inside node
        self.data = data
        
        # Pointer to next node in the linked list
        self.next = None


class LinkedList:
    def __init__(self):
        # Head initially points to nothing
        self.head = None

    def insertAtHead(self, data):
        # Create a new node with given data
        new_node = Node(data)

        # New node should point to current head
        new_node.next = self.head

        # Update head to new node
        self.head = new_node


    def insertAtIndex(self, data, index):
        # If index is 0, insertion happens at head
        if index == 0:
            self.insertAtHead(data)
            return

        # Create the new node that will be inserted
        new_node = Node(data)

        # Temporary pointer used to traverse the list
        temp = self.head

        # Counter used to reach index - 1 position
        count = 0

        # Traverse until reaching the node before insertion index
        while temp is not None and count < index - 1:
            temp = temp.next
            count += 1

        # If position exists, perform insertion
        if temp is not None:
            # New node points to next node
            new_node.next = temp.next

            # Previous node now points to new node
            temp.next = new_node


    def updateNode(self, valToUpdate, index):
        # Temporary pointer used for traversal
        temp = self.head

        # Counter to track current index
        count = 0

        # Traverse until reaching the desired index
        while temp is not None:
            if count == index:
                # Update node data
                temp.data = valToUpdate
                return
            temp = temp.next
            count += 1


    def print_LL(self):
        # Start traversal from head
        temp = self.head

        # Traverse entire linked list
        while temp is not None:
            print(temp.data, end=" -> ")
            temp = temp.next

        # Indicate end of list
        print("None")


    def length_LL(self):
        # Counter to count number of nodes
        count = 0

        # Temporary pointer to traverse list
        temp = self.head

        # Traverse until end of list
        while temp is not None:
            count += 1
            temp = temp.next

        # Return total node count
        return count


# ---------- Expected Output Demonstration ----------

ll1 = LinkedList()

ll1.insertAtHead(10)
ll1.insertAtHead(20)
ll1.insertAtHead(30)

ll1.insertAtIndex(15, 2)

ll1.updateNode(100, 1)

ll1.print_LL()

print("Length:", ll1.length_LL())

# Expected Output
# 30 -> 100 -> 15 -> 10 -> None
# Length: 4
```
Below is a detailed breakdown of the provided **singly linked list** implementation in Python. We'll examine each class, method, and line of code, and then trace through the demonstration to see how the final output is produced.

---

## 1. The `Node` Class

```python
class Node:
    def __init__(self, data):
        self.data = data      # stores the value
        self.next = None       # pointer to the next node (initially None)
```

- **Purpose**: Represents a single element (node) in the linked list.
- **Attributes**:
  - `data`: holds the actual value (any type).
  - `next`: a reference (pointer) to the next node in the sequence. When a node is first created, it points to `None`, meaning it is the last node (or isolated).

---

## 2. The `LinkedList` Class

### `__init__` – Constructor

```python
class LinkedList:
    def __init__(self):
        self.head = None      # initially, the list is empty
```

- Initializes an empty linked list.
- `self.head` is the entry point of the list. It either points to the first node or is `None` if the list is empty.

---

### `insertAtHead(self, data)`

```python
def insertAtHead(self, data):
    new_node = Node(data)      # 1. create a new node
    new_node.next = self.head   # 2. link new node to the current first node
    self.head = new_node        # 3. make the new node the head
```

- **Operation**: Inserts a new node at the beginning of the list.
- **Steps**:
  1. Create a `Node` object containing the provided `data`.
  2. Set the new node’s `next` pointer to whatever `self.head` currently points to (could be `None` or an existing node).
  3. Update `self.head` to point to the new node, making it the first element.
- **Complexity**: O(1) – constant time, because no traversal is needed.

---

### `insertAtIndex(self, data, index)`

```python
def insertAtIndex(self, data, index):
    if index == 0:
        self.insertAtHead(data)
        return

    new_node = Node(data)
    temp = self.head
    count = 0

    # traverse to the node just before the insertion point
    while temp is not None and count < index - 1:
        temp = temp.next
        count += 1

    # if we found a valid position (temp is not None)
    if temp is not None:
        new_node.next = temp.next   # new node points to the next node
        temp.next = new_node        # previous node points to new node
```

- **Operation**: Inserts a new node at a specified index (0‑based).
- **Logic**:
  - If `index == 0`, insertion is at the head – delegate to `insertAtHead`.
  - Otherwise:
    - Create the new node.
    - Use a temporary pointer `temp` to traverse from the head.
    - `count` tracks the current position. We stop when `count == index - 1` (i.e., the node **before** the insertion point) or we reach the end of the list (`temp is None`).
    - If `temp` is not `None`, the index is within the list bounds. Insert the new node:
      - `new_node.next = temp.next` – the new node now points to the node that used to come after `temp`.
      - `temp.next = new_node` – the previous node now points to the new node.
- **Edge Cases**:
  - If `index` is larger than the current list length, the `while` loop will exit because `temp` becomes `None`, and the insertion is silently skipped (no error).
  - The method does **not** handle negative indices.
- **Complexity**: O(n) in the worst case (traversal to the insertion point).

---

### `updateNode(self, valToUpdate, index)`

```python
def updateNode(self, valToUpdate, index):
    temp = self.head
    count = 0

    while temp is not None:
        if count == index:
            temp.data = valToUpdate   # replace the data
            return
        temp = temp.next
        count += 1
```

- **Operation**: Updates the data value of the node at the given index.
- **How it works**:
  - Traverse the list from the head, keeping a `count` of the current index.
  - When `count == index`, replace that node’s `data` with `valToUpdate` and exit the method.
  - If the index is out of range (i.e., the loop finishes without finding the index), the method simply returns without making any change. No error is raised.
- **Complexity**: O(n) in the worst case.

---

### `print_LL(self)`

```python
def print_LL(self):
    temp = self.head
    while temp is not None:
        print(temp.data, end=" -> ")
        temp = temp.next
    print("None")
```

- **Operation**: Prints the entire linked list in a readable format.
- **Process**:
  - Start from `head`.
  - For each node, print its `data` followed by `" -> "`.
  - Move to the next node.
  - After the loop, print `"None"` to indicate the end of the list.
- **Complexity**: O(n) – must visit every node.

---

### `length_LL(self)`

```python
def length_LL(self):
    count = 0
    temp = self.head
    while temp is not None:
        count += 1
        temp = temp.next
    return count
```

- **Operation**: Returns the number of nodes in the linked list.
- **Process**:
  - Initialize `count` to 0.
  - Traverse from head, incrementing `count` for each node visited.
  - Return the final count.
- **Complexity**: O(n).

---

## 3. Demonstration and Expected Output

The provided test code creates a linked list, performs several operations, and prints the result. Let’s walk through each step.

```python
ll1 = LinkedList()
```

- **State**: `head = None` (empty list).

```python
ll1.insertAtHead(10)
```

- Create node with data `10`. New node’s `next` becomes `None` (current head). Head now points to node `10`.  
  **List**: `10 -> None`

```python
ll1.insertAtHead(20)
```

- Create node with data `20`. New node’s `next` becomes current head (node `10`). Head now points to node `20`.  
  **List**: `20 -> 10 -> None`

```python
ll1.insertAtHead(30)
```

- Create node with data `30`. New node’s `next` becomes current head (node `20`). Head now points to node `30`.  
  **List**: `30 -> 20 -> 10 -> None`

```python
ll1.insertAtIndex(15, 2)
```

- `index = 2` (0‑based). Since index is not 0, we traverse:
  - `temp` starts at head (node `30`, count=0).  
    `count < 1` → move to next node (`20`), count=1.  
    `count < 1`? No, loop stops. `temp` now points to node `20` (the node at index 1).
  - `temp` is not `None`, so insertion proceeds:
    - `new_node` with data `15`.
    - `new_node.next = temp.next` → temp.next currently points to node `10`, so new_node now points to node `10`.
    - `temp.next = new_node` → node `20` now points to new node `15`.
  - **Resulting list**: `30 -> 20 -> 15 -> 10 -> None`

```python
ll1.updateNode(100, 1)
```

- Traverse from head:
  - count=0, temp = node `30` → not index 1.
  - count=1, temp = node `20` → index matches, update its data to `100`.  
  - **List becomes**: `30 -> 100 -> 15 -> 10 -> None`

```python
ll1.print_LL()
```

- Prints: `30 -> 100 -> 15 -> 10 -> None` (exactly the expected output).

```python
print("Length:", ll1.length_LL())
```

- Traverses and counts nodes: `30`, `100`, `15`, `10` → 4 nodes.  
  Output: `Length: 4`

---

## 4. Summary of Expected Output

```
30 -> 100 -> 15 -> 10 -> None
Length: 4
```

The code produces this output correctly, demonstrating:
- Insertions at the head and at a specific index.
- Updating an existing node’s value.
- Printing the list and computing its length.

---

## 5. Potential Improvements / Edge Cases

- **Out‑of‑range indices** in `insertAtIndex` and `updateNode` are silently ignored. It might be better to raise an exception (e.g., `IndexError`) or return a boolean to indicate success.
- **Negative indices** are not handled; they would be treated as regular indices but would fail because `count` never matches a negative number.
- **Update method** could be more efficient by reusing traversal from `insertAtIndex` or by storing node references, but for a simple learning implementation it’s fine.
- **Memory management** is handled automatically by Python’s garbage collector.

This implementation provides a clear, step‑by‑step illustration of how a singly linked list works in Python.
