```py
class Node:
    def __init__(self, data):
        # Store the value inside the node
        self.data = data
        
        # Pointer that will reference the next node
        self.next = None


def insert_at_tail_recursive(head, data):
    """
    Recursively inserts a new node with given data
    at the end of the linked list.
    """

    # Base Case:
    # If the linked list is empty, create a new node
    # and return it as the head of the list
    if head is None:
        return Node(data)

    # Base Case:
    # If current node is the last node,
    # attach the new node here
    if head.next is None:
        head.next = Node(data)
        return head

    # Recursive Case:
    # Move forward until the last node is found
    head.next = insert_at_tail_recursive(head.next, data)

    # Return unchanged head reference
    return head
```
## 1.Recursive Insert at Tail (Original)

### Explanation
This is the classic recursive approach to insert a node at the end of a singly linked list. It traverses the list recursively until it reaches the last node, then attaches the new node.

### Approach
- Base case: if head is `None`, create a new node and return it (empty list).
- Another base case: if `head.next` is `None`, we are at the last node; attach new node and return head.
- Recursive case: move forward by calling the function on `head.next` and reassign the result to `head.next`.

### Algorithm
1. If `head` is `None`, return a new `Node` with given data.
2. If `head.next` is `None`, set `head.next = Node(data)` and return `head`.
3. Otherwise, recursively call `insert_at_tail_recursive(head.next, data)` and assign to `head.next`.
4. Return `head`.

### 2.Pseudocode
```
function insert_at_tail_recursive(head, data):
    if head is None:
        return Node(data)
    if head.next is None:
        head.next = Node(data)
        return head
    head.next = insert_at_tail_recursive(head.next, data)
    return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_at_tail_recursive(head, data):
    # Base case: empty list -> create and return new node
    if head is None:
        return Node(data)          # Main logic: new node becomes head

    # Base case: reached last node -> attach new node after it
    if head.next is None:
        head.next = Node(data)     # Main logic: link new node
        return head                # Return unchanged head

    # Recursive step: move to next node and update its next
    head.next = insert_at_tail_recursive(head.next, data)  # Crucial: reassign to preserve links
    return head                    # Return original head

# Helper to print list
def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "\n")
        head = head.next

# Example usage
head = None
head = insert_at_tail_recursive(head, 10)  # Call: empty list
head = insert_at_tail_recursive(head, 20)  # Call: add to tail
head = insert_at_tail_recursive(head, 30)  # Call: add to tail
print_list(head)  # Output: 10 -> 20 -> 30
```

---

## Iterative Insert at Tail

### Explanation
This variant uses a simple loop to traverse to the end of the list and then attaches the new node. It avoids recursion overhead.

### Approach
- If head is `None`, return a new node (empty list).
- Otherwise, start from head and move `current` pointer until `current.next` is `None`.
- Set `current.next` to the new node.
- Return the original head.

### Algorithm
1. Create a new node with given data.
2. If head is `None`, return new node.
3. Initialize `current = head`.
4. While `current.next` is not `None`:
   - `current = current.next`
5. Set `current.next = new_node`.
6. Return `head`.

### Pseudocode
```
function insert_at_tail_iterative(head, data):
    new_node = Node(data)
    if head is None:
        return new_node
    current = head
    while current.next is not None:
        current = current.next
    current.next = new_node
    return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_at_tail_iterative(head, data):
    new_node = Node(data)          # Create new node
    if head is None:               # Special case: empty list
        return new_node             # New node becomes head

    current = head                  # Start traversal
    while current.next:             # Loop until last node
        current = current.next       # Move forward
    current.next = new_node          # Main logic: attach at tail
    return head                      # Return original head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "\n")
        head = head.next

# Example usage
head = None
head = insert_at_tail_iterative(head, 5)
head = insert_at_tail_iterative(head, 15)
head = insert_at_tail_iterative(head, 25)
print_list(head)  # Output: 5 -> 15 -> 25
```

---

## 3.Recursive Insert with Single Base Case

### Explanation
This variant combines the two base cases into one by checking if `head` is `None` or if we've reached the end. It's a cleaner recursive implementation.

### Approach
- If `head` is `None` or `head.next` is `None`, we handle the insertion directly.
- Otherwise, recurse on `head.next`.

### Algorithm
1. If `head is None`:
   - Return `Node(data)`.
2. If `head.next is None`:
   - Set `head.next = Node(data)`.
   - Return `head`.
3. Else:
   - `head.next = insert_at_tail_single_base(head.next, data)`
   - Return `head`.

### Pseudocode
```
function insert_at_tail_single_base(head, data):
    if head is None:
        return Node(data)
    if head.next is None:
        head.next = Node(data)
        return head
    head.next = insert_at_tail_single_base(head.next, data)
    return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_at_tail_single_base(head, data):
    # Base case: empty list -> new node
    if head is None:
        return Node(data)          # Main logic: new head

    # Base case: last node -> attach new node after it
    if head.next is None:
        head.next = Node(data)     # Main logic: link new node
        return head

    # Recursive step: go deeper
    head.next = insert_at_tail_single_base(head.next, data)  # Crucial: reassign
    return head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "\n")
        head = head.next

# Example
head = None
head = insert_at_tail_single_base(head, 100)
head = insert_at_tail_single_base(head, 200)
head = insert_at_tail_single_base(head, 300)
print_list(head)  # 100 -> 200 -> 300
```

---

## 4.Recursive with Helper Function (Accumulator)

### Explanation
This version uses an inner helper function that accumulates the path and performs the insertion. It demonstrates using a nested function for recursion.

### Approach
- Define a helper that takes the current node and recursively traverses, returning the modified node.
- The outer function calls the helper and returns the result.

### Algorithm
1. Define `helper(node)`:
   - If `node` is `None`, return `Node(data)`.
   - If `node.next` is `None`, set `node.next = Node(data)` and return `node`.
   - Else, set `node.next = helper(node.next)` and return `node`.
2. In outer function, call `helper(head)` and return it.

### Pseudocode
```
function insert_at_tail_helper(head, data):
    function recurse(node):
        if node is None:
            return Node(data)
        if node.next is None:
            node.next = Node(data)
            return node
        node.next = recurse(node.next)
        return node
    return recurse(head)
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_at_tail_helper(head, data):
    def recurse(node):
        # Base: empty sublist -> new node
        if node is None:
            return Node(data)          # Main logic: new node

        # Base: last node -> attach new node
        if node.next is None:
            node.next = Node(data)     # Main logic: link
            return node

        # Recursive: continue and reassign
        node.next = recurse(node.next)  # Crucial: update link
        return node

    return recurse(head)                # Call helper

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "\n")
        head = head.next

# Example
head = None
head = insert_at_tail_helper(head, 7)
head = insert_at_tail_helper(head, 14)
head = insert_at_tail_helper(head, 21)
print_list(head)  # 7 -> 14 -> 21
```

---

## 5.Tail Recursive Insert at Tail

### Explanation
Although Python does not optimize tail recursion, this version demonstrates a tail-recursive style where the recursive call is the last operation. It uses an extra parameter to carry the current node and a reference to the head.

### Approach
- Use a helper function that takes `current` and `head` (to eventually return) and updates the list.
- When `current.next` is `None`, attach new node and return head.
- Otherwise, recurse with `current.next` and same head.

### Algorithm
1. If `head` is `None`, return `Node(data)`.
2. Define `tail_recursive(current, head)`:
   - If `current.next` is `None`:
     - `current.next = Node(data)`
     - Return `head`
   - Else:
     - Return `tail_recursive(current.next, head)`
3. Call helper with `head` as both current and head.

### Pseudocode
```
function insert_at_tail_tailrec(head, data):
    if head is None:
        return Node(data)
    function helper(current, head):
        if current.next is None:
            current.next = Node(data)
            return head
        else:
            return helper(current.next, head)
    return helper(head, head)
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_at_tail_tailrec(head, data):
    if head is None:                     # Empty list case
        return Node(data)                  # Main logic: new head

    def helper(current, original_head):
        # Base: reached last node
        if current.next is None:
            current.next = Node(data)     # Attach new node
            return original_head           # Return original head (tail call)
        # Recursive: tail call to next node
        return helper(current.next, original_head)  # Tail recursion

    return helper(head, head)             # Start with head as current

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "\n")
        head = head.next

# Example
head = None
head = insert_at_tail_tailrec(head, 3)
head = insert_at_tail_tailrec(head, 6)
head = insert_at_tail_tailrec(head, 9)
print_list(head)  # 3 -> 6 -> 9
```

---

## 6.Using a Dummy/Sentinel Node

### Explanation
A dummy node simplifies edge cases (like empty list) by providing a permanent node that sits before the real head. Insertion logic becomes uniform.

### Approach
- Create a dummy node with `next` pointing to head.
- Start traversal from dummy (or use a pointer that starts at dummy).
- Move to the last node, then attach new node.
- Return dummy.next as new head.

### Algorithm
1. Create a dummy node with `next = head`.
2. Set `current = dummy`.
3. While `current.next` is not `None`:
   - `current = current.next`
4. `current.next = Node(data)`
5. Return `dummy.next`.

### Pseudocode
```
function insert_at_tail_dummy(head, data):
    dummy = Node(None)
    dummy.next = head
    current = dummy
    while current.next is not None:
        current = current.next
    current.next = Node(data)
    return dummy.next
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_at_tail_dummy(head, data):
    dummy = Node(None)               # Dummy node, data irrelevant
    dummy.next = head                 # Link dummy to head

    current = dummy                    # Start from dummy
    while current.next:                 # Traverse until last node (current.next is None)
        current = current.next           # Move forward

    current.next = Node(data)          # Attach new node at tail
    return dummy.next                   # New head (original or updated)

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "\n")
        head = head.next

# Example
head = None
head = insert_at_tail_dummy(head, 2)
head = insert_at_tail_dummy(head, 4)
head = insert_at_tail_dummy(head, 6)
print_list(head)  # 2 -> 4 -> 6
```

---

## 7.Recursive Insert Returning New Tail

### Explanation
Instead of returning the head, this recursion returns the new tail node. The head is passed unchanged through the call stack. It's an alternative recursive design.

### Approach
- If head is `None`, return new node (which is both head and tail).
- If we are at the last node (head.next is None), attach new node and return new node as tail.
- Otherwise, recurse on head.next, but ignore the returned tail (since we don't need it) and return head unchanged.

### Algorithm
1. If `head` is `None`, return `Node(data)`.
2. If `head.next` is `None`:
   - `head.next = Node(data)`
   - Return `head.next` (the new tail)
3. Else:
   - Call `insert_at_tail_return_tail(head.next, data)` and ignore result.
   - Return `head`.

### Pseudocode
```
function insert_at_tail_return_tail(head, data):
    if head is None:
        return Node(data)
    if head.next is None:
        head.next = Node(data)
        return head.next
    insert_at_tail_return_tail(head.next, data)
    return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_at_tail_return_tail(head, data):
    # Base: empty list -> new node is both head and tail
    if head is None:
        return Node(data)          # Returns new head (caller must assign)

    # Base: last node -> attach new node and return it as tail
    if head.next is None:
        head.next = Node(data)     # Main logic: link new node
        return head.next            # Return new tail (not used further up)

    # Recursive: move forward, ignore returned tail
    insert_at_tail_return_tail(head.next, data)  # Recursive call (result discarded)
    return head                      # Return original head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "\n")
        head = head.next

# Example
head = None
head = insert_at_tail_return_tail(head, 11)  # Returns head
head = insert_at_tail_return_tail(head, 22)  # Returns head
head = insert_at_tail_return_tail(head, 33)  # Returns head
print_list(head)  # 11 -> 22 -> 33
```

---

## 8.Two-Pointer Iterative

### Explanation
This variant uses two pointers: one to traverse and another to keep track of the previous node. It's a common technique for insertions and deletions.

### Approach
- If head is `None`, return new node.
- Use `prev` and `curr` pointers. Start `prev = None`, `curr = head`.
- Traverse until `curr` is `None` (end of list). Actually we want to stop when `curr` is `None`, then `prev` is the last node.
- Set `prev.next = new_node`.

### Algorithm
1. Create `new_node = Node(data)`.
2. If head is `None`, return `new_node`.
3. Set `prev = None`, `curr = head`.
4. While `curr` is not `None`:
   - `prev = curr`
   - `curr = curr.next`
5. `prev.next = new_node`.
6. Return `head`.

### Pseudocode
```
function insert_at_tail_two_pointer(head, data):
    new_node = Node(data)
    if head is None:
        return new_node
    prev = None
    curr = head
    while curr is not None:
        prev = curr
        curr = curr.next
    prev.next = new_node
    return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_at_tail_two_pointer(head, data):
    new_node = Node(data)
    if head is None:                     # Empty list
        return new_node

    prev = None
    curr = head
    while curr:                            # Traverse until curr becomes None
        prev = curr                         # prev always one step behind
        curr = curr.next

    prev.next = new_node                   # Main logic: attach new node after last node
    return head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "\n")
        head = head.next

# Example
head = None
head = insert_at_tail_two_pointer(head, 8)
head = insert_at_tail_two_pointer(head, 16)
head = insert_at_tail_two_pointer(head, 24)
print_list(head)  # 8 -> 16 -> 24
```

---

## 9.Using List to Collect and Link

### Explanation
This unusual variant first collects all existing node data into a Python list, then rebuilds the linked list from scratch including the new data at the end. It's not efficient but shows a different perspective.

### Approach
- Traverse the list and collect all data into a list.
- Append the new data to the list.
- Rebuild the linked list from the list, creating new nodes.

### Algorithm
1. If head is `None`, return `Node(data)` (simple case).
2. Initialize `values = []`.
3. Traverse the list: for each node, append its data to `values`.
4. Append `data` to `values`.
5. Rebuild: create a new head from `values[0]`, then link subsequent nodes.
6. Return new head.

### Pseudocode
```
function insert_at_tail_rebuild(head, data):
    if head is None:
        return Node(data)
    values = []
    current = head
    while current:
        values.append(current.data)
        current = current.next
    values.append(data)
    new_head = Node(values[0])
    current = new_head
    for val in values[1:]:
        current.next = Node(val)
        current = current.next
    return new_head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_at_tail_rebuild(head, data):
    # Edge case: empty list
    if head is None:
        return Node(data)

    # Collect existing data
    values = []
    current = head
    while current:
        values.append(current.data)   # Store each node's data
        current = current.next

    # Append new data
    values.append(data)                # Main logic: add new value to list

    # Rebuild list from values
    new_head = Node(values[0])         # First node
    current = new_head
    for val in values[1:]:              # Link remaining nodes
        current.next = Node(val)
        current = current.next

    return new_head                      # Return new head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "\n")
        head = head.next

# Example
head = None
head = insert_at_tail_rebuild(head, 1)
head = insert_at_tail_rebuild(head, 2)
head = insert_at_tail_rebuild(head, 3)
print_list(head)  # 1 -> 2 -> 3
```

---

## 10.Recursive with Global/Nonlocal

### Explanation
This variant uses a nested function with `nonlocal` to modify a head reference without returning it. It's a demonstration of closure and nonlocal variables.

### Approach
- Define an outer function that holds a variable `head_ref` (could be a list or use `nonlocal`).
- Inner recursive function traverses and updates the list by mutating the head via the outer variable.

### Algorithm
1. If head is `None`, we create a new node and assign it to the outer head reference.
2. Otherwise, we traverse recursively, and when we reach the last node, we set its `next` to new node.
3. The outer head reference is updated only for the empty case.

### Pseudocode
```
function insert_at_tail_nonlocal(head, data):
    # Using a wrapper to hold head
    container = [head]  # or use nonlocal in nested function
    def recurse(node):
        if node is None:
            container[0] = Node(data)
        elif node.next is None:
            node.next = Node(data)
        else:
            recurse(node.next)
    recurse(container[0])
    return container[0]
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_at_tail_nonlocal(head, data):
    # Use a mutable container to hold head (since nonlocal works in nested function)
    head_ref = [head]                    # List to hold reference

    def recurse(node):
        if node is None:                  # Empty list case
            head_ref[0] = Node(data)       # Update head via container
        elif node.next is None:            # Last node
            node.next = Node(data)          # Main logic: attach new node
        else:
            recurse(node.next)               # Recursive traversal

    recurse(head_ref[0])                   # Start recursion
    return head_ref[0]                      # Return possibly updated head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "\n")
        head = head.next

# Example
head = None
head = insert_at_tail_nonlocal(head, 50)
head = insert_at_tail_nonlocal(head, 60)
head = insert_at_tail_nonlocal(head, 70)
print_list(head)  # 50 -> 60 -> 70
```