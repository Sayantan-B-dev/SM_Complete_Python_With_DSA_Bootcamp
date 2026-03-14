```py
class Node:
    def __init__(self, data):
        # Store the data value inside the node
        self.data = data
        
        # Pointer referencing the next node in the list
        self.next = None


class LinkedList:
    def __init__(self):
        # Initially the linked list is empty
        self.head = None

    def insert_at_tail(self, head: Node, data: int) -> Node:
        """
        Recursively inserts a node at the tail of the linked list.

        Parameters
        ----------
        head : Node
            Current node being processed during recursion.
        data : int
            Value that must be appended at the end.

        Returns
        -------
        Node
            Updated node reference so recursion reconnects correctly.
        """

        # BASE CASE
        # If the list position is empty, create a new node
        # This node becomes the last node of the linked list
        if head is None:
            return Node(data)

        # RECURSIVE CASE
        # Continue traversal toward the tail node
        # The recursive call returns the updated next reference
        head.next = self.insert_at_tail(head.next, data)

        # Return current node so the previous recursion frame reconnects
        return head
```

## 1.Original Recursive Insert at Tail

### Explanation
This is the classic recursive approach to insert a node at the end of a singly linked list. The method traverses the list recursively until it reaches a `None` reference, then creates a new node and returns it upward, linking each node along the way.

### Approach
- Base case: if the current node (`head`) is `None`, create a new node and return it (this becomes the last node).
- Recursive case: call `insert_at_tail` on `head.next`, and assign the returned node to `head.next`.
- Finally, return the current node to preserve the link structure.

### Algorithm
1. If `head` is `None`, return a new `Node` containing `data`.
2. Otherwise, recursively insert the new node at the tail of the sublist starting at `head.next`.
3. Set `head.next` to the result of the recursive call.
4. Return `head`.

### Pseudocode
```
class LinkedList:
    def insert_at_tail(self, head, data):
        if head is None:
            return Node(data)
        head.next = self.insert_at_tail(head.next, data)
        return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def insert_at_tail(self, head: Node, data: int) -> Node:
        # Base case: reached end of list -> create new node
        if head is None:
            return Node(data)          # Main logic: node creation

        # Recursive case: continue traversal, update next link
        head.next = self.insert_at_tail(head.next, data)  # Crucial: reassign to preserve chain
        return head                      # Return current node to caller

    def display(self):
        current = self.head
        while current:
            print(current.data, end=" -> " if current.next else "")
            current = current.next
        print()

# Example usage
ll = LinkedList()
ll.head = ll.insert_at_tail(ll.head, 10)   # Call: initial empty list
ll.head = ll.insert_at_tail(ll.head, 20)
ll.head = ll.insert_at_tail(ll.head, 30)
ll.display()  # Output: 10 -> 20 -> 30
```

---

## 2.Iterative Insert at Tail (While Loop)

### Explanation
This variant uses a simple `while` loop to traverse to the end of the list and then attaches the new node. It avoids recursion overhead and is straightforward.

### Approach
- If the list is empty (`head` is `None`), create a new node and return it as the new head.
- Otherwise, start from the head and move a `current` pointer until `current.next` is `None`.
- Set `current.next` to the new node.
- Return the original head.

### Algorithm
1. Create a new node with the given data.
2. If `head` is `None`, return the new node.
3. Set `current = head`.
4. While `current.next` is not `None`:
   - `current = current.next`
5. Set `current.next = new_node`.
6. Return `head`.

### Pseudocode
```
class LinkedList:
    def insert_at_tail(self, head, data):
        new_node = Node(data)
        if head is None:
            return new_node
        current = head
        while current.next:
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

class LinkedList:
    def __init__(self):
        self.head = None

    def insert_at_tail(self, head: Node, data: int) -> Node:
        new_node = Node(data)                # Create new node
        if head is None:                      # Empty list case
            return new_node                     # Main logic: new node becomes head

        current = head
        while current.next:                    # Traverse to last node
            current = current.next
        current.next = new_node                # Main logic: attach at tail
        return head                              # Return unchanged head

    def display(self):
        current = self.head
        while current:
            print(current.data, end=" -> " if current.next else "")
            current = current.next
        print()

# Example
ll = LinkedList()
ll.head = ll.insert_at_tail(ll.head, 5)
ll.head = ll.insert_at_tail(ll.head, 15)
ll.head = ll.insert_at_tail(ll.head, 25)
ll.display()  # Output: 5 -> 15 -> 25
```

---

## 3.Insert at Tail with Tail Pointer

### Explanation
Maintaining a separate `tail` pointer in the linked list allows O(1) insertion at the end. This is a professional optimization for frequent tail insertions.

### Approach
- In the `LinkedList` class, keep both `head` and `tail` attributes.
- For an empty list, set both `head` and `tail` to the new node.
- Otherwise, attach the new node after the current tail, then update the tail to the new node.

### Algorithm
1. Create a new node.
2. If `head` is `None` (empty list):
   - Set `head = new_node`
   - Set `tail = new_node`
3. Else:
   - Set `tail.next = new_node`
   - Update `tail = new_node`
4. Return `head` (head unchanged unless list was empty).

### Pseudocode
```
class LinkedList:
    def __init__(self):
        self.head = None
        self.tail = None

    def insert_at_tail(self, head, data):
        new_node = Node(data)
        if self.head is None:
            self.head = new_node
            self.tail = new_node
        else:
            self.tail.next = new_node
            self.tail = new_node
        return self.head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
        self.tail = None                      # Special: tail pointer

    def insert_at_tail(self, head: Node, data: int) -> Node:
        new_node = Node(data)
        if self.head is None:                  # Empty list
            self.head = new_node
            self.tail = new_node                 # Set both head and tail
        else:
            self.tail.next = new_node            # Main logic: attach at tail
            self.tail = new_node                  # Update tail pointer
        return self.head                         # Return head (unchanged)

    def display(self):
        current = self.head
        while current:
            print(current.data, end=" -> " if current.next else "")
            current = current.next
        print()

# Example
ll = LinkedList()
ll.head = ll.insert_at_tail(ll.head, 2)
ll.head = ll.insert_at_tail(ll.head, 4)
ll.head = ll.insert_at_tail(ll.head, 6)
ll.display()  # Output: 2 -> 4 -> 6
```

---

## 4.Recursive Insert with Helper Function

### Explanation
This version uses an inner helper function to perform the recursion, keeping the method interface clean and avoiding confusion with the `self` parameter.

### Approach
- Define a nested function `recurse(node)` that handles the recursion.
- In the outer method, call `recurse(head)` and return its result.
- The helper has access to `data` via closure.

### Algorithm
1. Define `recurse(node)`:
   - If `node` is `None`, return `Node(data)`.
   - Otherwise, set `node.next = recurse(node.next)` and return `node`.
2. Call `recurse(head)` and return the result.

### Pseudocode
```
class LinkedList:
    def insert_at_tail(self, head, data):
        def recurse(node):
            if node is None:
                return Node(data)
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

class LinkedList:
    def __init__(self):
        self.head = None

    def insert_at_tail(self, head: Node, data: int) -> Node:
        def recurse(node):
            # Base: end of list -> new node
            if node is None:
                return Node(data)          # Main logic: create node

            # Recursive step: update next link
            node.next = recurse(node.next)  # Crucial: recursive call and reassign
            return node                      # Return current node

        return recurse(head)                 # Call helper

    def display(self):
        current = self.head
        while current:
            print(current.data, end=" -> " if current.next else "")
            current = current.next
        print()

# Example
ll = LinkedList()
ll.head = ll.insert_at_tail(ll.head, 8)
ll.head = ll.insert_at_tail(ll.head, 16)
ll.head = ll.insert_at_tail(ll.head, 24)
ll.display()  # Output: 8 -> 16 -> 24
```

---

## 5.Tail Recursive Insert with Accumulator

### Explanation
Although Python does not optimize tail recursion, this variant demonstrates a tail‑recursive style where the recursive call is the last operation. It uses an extra parameter to carry the current node and returns the head unchanged.

### Approach
- If the list is empty, return a new node.
- Define a helper `tail_rec(current, original_head)`.
- If `current.next` is `None`, attach new node and return `original_head`.
- Otherwise, recursively call with `current.next` and the same `original_head`.

### Algorithm
1. If `head` is `None`, return `Node(data)`.
2. Define helper `tail_rec(current, head)`:
   - If `current.next` is `None`:
     - `current.next = Node(data)`
     - Return `head`
   - Else:
     - Return `tail_rec(current.next, head)`
3. Call `tail_rec(head, head)` and return result.

### Pseudocode
```
class LinkedList:
    def insert_at_tail(self, head, data):
        if head is None:
            return Node(data)
        def tail_rec(current, head):
            if current.next is None:
                current.next = Node(data)
                return head
            else:
                return tail_rec(current.next, head)
        return tail_rec(head, head)
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def insert_at_tail(self, head: Node, data: int) -> Node:
        if head is None:                       # Empty list
            return Node(data)                     # Main logic: new head

        def tail_rec(current, original_head):
            # Base: last node found
            if current.next is None:
                current.next = Node(data)        # Main logic: attach new node
                return original_head              # Return original head (tail call)
            # Recursive: move to next node
            return tail_rec(current.next, original_head)  # Tail recursion

        return tail_rec(head, head)              # Start recursion

    def display(self):
        current = self.head
        while current:
            print(current.data, end=" -> " if current.next else "")
            current = current.next
        print()

# Example
ll = LinkedList()
ll.head = ll.insert_at_tail(ll.head, 3)
ll.head = ll.insert_at_tail(ll.head, 6)
ll.head = ll.insert_at_tail(ll.head, 9)
ll.display()  # Output: 3 -> 6 -> 9
```

---

## 6.Insert at Tail Using Dummy/Sentinel Node

### Explanation
A dummy node simplifies edge cases by providing a permanent node that sits before the real head. This makes the insertion logic identical for empty and non‑empty lists.

### Approach
- Create a dummy node whose `next` points to the current head.
- Start from the dummy and traverse to the last node (where `next` is `None`).
- Attach the new node there.
- Return `dummy.next` as the new head.

### Algorithm
1. Create `dummy = Node(None)` and set `dummy.next = head`.
2. Set `current = dummy`.
3. While `current.next` is not `None`:
   - `current = current.next`
4. `current.next = Node(data)`.
5. Return `dummy.next`.

### Pseudocode
```
class LinkedList:
    def insert_at_tail(self, head, data):
        dummy = Node(None)
        dummy.next = head
        current = dummy
        while current.next:
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

class LinkedList:
    def __init__(self):
        self.head = None

    def insert_at_tail(self, head: Node, data: int) -> Node:
        dummy = Node(None)                # Sentinel node, data irrelevant
        dummy.next = head                   # Link dummy to list

        current = dummy
        while current.next:                  # Traverse until last node
            current = current.next
        current.next = Node(data)            # Main logic: attach new node

        return dummy.next                     # New head (could be original or updated)

    def display(self):
        current = self.head
        while current:
            print(current.data, end=" -> " if current.next else "")
            current = current.next
        print()

# Example
ll = LinkedList()
ll.head = ll.insert_at_tail(ll.head, 7)
ll.head = ll.insert_at_tail(ll.head, 14)
ll.head = ll.insert_at_tail(ll.head, 21)
ll.display()  # Output: 7 -> 14 -> 21
```

---

## 7.Two-Pointer Iterative Insert

### Explanation
This variant uses two pointers: one to traverse (`curr`) and one to keep track of the previous node (`prev`). It's a common technique that can be adapted for other operations like deletion.

### Approach
- If the list is empty, return a new node.
- Initialize `prev = None` and `curr = head`.
- Traverse until `curr` becomes `None` (meaning `prev` is the last node).
- Set `prev.next` to the new node.

### Algorithm
1. Create `new_node = Node(data)`.
2. If `head` is `None`, return `new_node`.
3. Set `prev = None`, `curr = head`.
4. While `curr` is not `None`:
   - `prev = curr`
   - `curr = curr.next`
5. `prev.next = new_node`.
6. Return `head`.

### Pseudocode
```
class LinkedList:
    def insert_at_tail(self, head, data):
        new_node = Node(data)
        if head is None:
            return new_node
        prev = None
        curr = head
        while curr:
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

class LinkedList:
    def __init__(self):
        self.head = None

    def insert_at_tail(self, head: Node, data: int) -> Node:
        new_node = Node(data)
        if head is None:                     # Empty list
            return new_node                     # Main logic: new head

        prev = None
        curr = head
        while curr:                            # Traverse until curr becomes None
            prev = curr                          # prev always one step behind
            curr = curr.next

        prev.next = new_node                   # Main logic: attach new node after last node
        return head                              # Return original head

    def display(self):
        current = self.head
        while current:
            print(current.data, end=" -> " if current.next else "")
            current = current.next
        print()

# Example
ll = LinkedList()
ll.head = ll.insert_at_tail(ll.head, 1)
ll.head = ll.insert_at_tail(ll.head, 2)
ll.head = ll.insert_at_tail(ll.head, 3)
ll.display()  # Output: 1 -> 2 -> 3
```

---

## 8.Recursive Insert with Nonlocal Reference

### Explanation
This version uses a nested function with a `nonlocal` variable to update the head reference without returning it. It demonstrates closures and mutation of outer variables.

### Approach
- Store the head in a mutable container (like a list) or use `nonlocal` inside a nested function.
- Define a recursive helper that traverses and modifies the list, updating the head when necessary.
- The outer method returns the updated head from the container.

### Algorithm
1. Create a list `head_ref = [head]` to hold the head reference.
2. Define `recurse(node)`:
   - If `node` is `None`:
     - `head_ref[0] = Node(data)` (update head)
   - Else if `node.next` is `None`:
     - `node.next = Node(data)`
   - Else:
     - `recurse(node.next)`
3. Call `recurse(head_ref[0])`.
4. Return `head_ref[0]`.

### Pseudocode
```
class LinkedList:
    def insert_at_tail(self, head, data):
        head_ref = [head]
        def recurse(node):
            if node is None:
                head_ref[0] = Node(data)
            elif node.next is None:
                node.next = Node(data)
            else:
                recurse(node.next)
        recurse(head_ref[0])
        return head_ref[0]
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def insert_at_tail(self, head: Node, data: int) -> Node:
        head_ref = [head]                     # Mutable container

        def recurse(node):
            if node is None:                    # Base: empty list
                head_ref[0] = Node(data)          # Update head via container (special)
            elif node.next is None:              # Base: last node
                node.next = Node(data)            # Main logic: attach new node
            else:
                recurse(node.next)                # Recursive traversal

        recurse(head_ref[0])                    # Start recursion
        return head_ref[0]                       # Return updated head

    def display(self):
        current = self.head
        while current:
            print(current.data, end=" -> " if current.next else "")
            current = current.next
        print()

# Example
ll = LinkedList()
ll.head = ll.insert_at_tail(ll.head, 50)
ll.head = ll.insert_at_tail(ll.head, 60)
ll.head = ll.insert_at_tail(ll.head, 70)
ll.display()  # Output: 50 -> 60 -> 70
```

---

## 9.Insert at Tail Using List Rebuild

### Explanation
This unusual variant first collects all existing node data into a Python list, appends the new data, then rebuilds the linked list from scratch. It is not efficient but illustrates a different mindset.

### Approach
- If the list is empty, simply return a new node.
- Traverse the list and collect each node's data into a list.
- Append the new data to the list.
- Rebuild the linked list by creating new nodes from the list values.

### Algorithm
1. If `head` is `None`, return `Node(data)`.
2. Initialize `values = []`.
3. Traverse from `head`, appending each node's data to `values`.
4. Append `data` to `values`.
5. Create a new head from `values[0]`.
6. Iterate through remaining values, linking new nodes.
7. Return the new head.

### Pseudocode
```
class LinkedList:
    def insert_at_tail(self, head, data):
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

class LinkedList:
    def __init__(self):
        self.head = None

    def insert_at_tail(self, head: Node, data: int) -> Node:
        if head is None:                         # Empty list
            return Node(data)                       # Simple case

        # Collect existing data
        values = []
        current = head
        while current:
            values.append(current.data)            # Store each node's data
            current = current.next

        # Append new data
        values.append(data)                        # Main logic: add to list

        # Rebuild linked list
        new_head = Node(values[0])                  # First node
        current = new_head
        for val in values[1:]:                      # Link remaining nodes
            current.next = Node(val)
            current = current.next

        return new_head                              # Return newly built head

    def display(self):
        current = self.head
        while current:
            print(current.data, end=" -> " if current.next else "")
            current = current.next
        print()

# Example
ll = LinkedList()
ll.head = ll.insert_at_tail(ll.head, 100)
ll.head = ll.insert_at_tail(ll.head, 200)
ll.head = ll.insert_at_tail(ll.head, 300)
ll.display()  # Output: 100 -> 200 -> 300
```

---

## 10.Insert at Tail Using Stack

### Explanation
This variant uses an explicit stack (Python list) to simulate recursion. Nodes are pushed onto a stack during traversal, then popped to reconnect after inserting the new node at the tail.

### Approach
- Traverse the list, pushing each node onto a stack.
- When reaching the end, create a new node.
- Then pop nodes from the stack and reassign their `next` pointers accordingly (though in a singly linked list we only need to update the last node's next).

### Algorithm
1. If `head` is `None`, return `Node(data)`.
2. Initialize an empty stack.
3. Set `current = head`.
4. While `current` is not `None`:
   - Push `current` onto stack.
   - `current = current.next`
5. Pop the last node from stack (or simply use the top) – actually we need the last node to attach new node.
   - Since we pushed all nodes, the last node is the top of stack.
6. Set `last_node = stack[-1]` and set `last_node.next = Node(data)`.
7. Return `head` (unchanged).

### Pseudocode
```
class LinkedList:
    def insert_at_tail(self, head, data):
        if head is None:
            return Node(data)
        stack = []
        current = head
        while current:
            stack.append(current)
            current = current.next
        last_node = stack[-1]
        last_node.next = Node(data)
        return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def insert_at_tail(self, head: Node, data: int) -> Node:
        if head is None:                           # Empty list
            return Node(data)                         # Main logic: new head

        stack = []                                   # Use list as stack
        current = head
        while current:                                # Traverse and push nodes
            stack.append(current)
            current = current.next

        last_node = stack[-1]                         # Last node is top of stack
        last_node.next = Node(data)                   # Main logic: attach new node

        return head                                    # Head unchanged

    def display(self):
        current = self.head
        while current:
            print(current.data, end=" -> " if current.next else "")
            current = current.next
        print()

# Example
ll = LinkedList()
ll.head = ll.insert_at_tail(ll.head, 9)
ll.head = ll.insert_at_tail(ll.head, 18)
ll.head = ll.insert_at_tail(ll.head, 27)
ll.display()  # Output: 9 -> 18 -> 27
```