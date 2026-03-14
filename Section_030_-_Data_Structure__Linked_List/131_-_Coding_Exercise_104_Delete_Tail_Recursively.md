## Recursive Delete at Tail (Original)

### Explanation
This is the classic recursive approach to delete the last node from a singly linked list. It traverses recursively until it reaches the last node, then removes it by returning `None` to the previous node, effectively unlinking the last node.

### Approach
- Base case 1: if `head` is `None`, the list is empty → return `None`.
- Base case 2: if `head.next` is `None`, this is the last node → delete it by returning `None`.
- Recursive case: call `delete_at_tail_recursive` on `head.next`, assign the result to `head.next`, and return `head`.

### Algorithm
1. If `head` is `None`, return `None`.
2. If `head.next` is `None`, return `None` (the previous node's next becomes `None`).
3. Set `head.next = delete_at_tail_recursive(head.next)`.
4. Return `head`.

### Pseudocode
```
function delete_at_tail_recursive(head):
    if head is None:
        return None
    if head.next is None:
        return None
    head.next = delete_at_tail_recursive(head.next)
    return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def delete_at_tail_recursive(head):
    # Base case: empty list
    if head is None:
        return None          # Return None for empty list

    # Base case: only one node (last node)
    if head.next is None:
        return None          # Main logic: delete this node by returning None

    # Recursive step: move to next node, update next link
    head.next = delete_at_tail_recursive(head.next)   # Crucial: reassign to maintain list
    return head               # Return current node unchanged

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = Node(1)
head.next = Node(2)
head.next.next = Node(3)
print("Before:", end=" ")
print_list(head)   # 1 -> 2 -> 3
head = delete_at_tail_recursive(head)
print("After: ", end=" ")
print_list(head)   # 1 -> 2
```

---

## Iterative Delete at Tail

### Explanation
This variant uses a simple loop to traverse to the second‑last node, then removes the last node by setting its `next` to `None`. It is iterative and avoids recursion.

### Approach
- If the list is empty or has only one node, return `None`.
- Otherwise, start from `head` and move a pointer until it points to the second‑last node.
- Set that node's `next` to `None`.
- Return the (unchanged) head.

### Algorithm
1. If `head` is `None` or `head.next` is `None`, return `None`.
2. Set `current = head`.
3. While `current.next.next` is not `None`:
   - `current = current.next`
4. Set `current.next = None`.
5. Return `head`.

### Pseudocode
```
function delete_at_tail_iterative(head):
    if head is None or head.next is None:
        return None
    current = head
    while current.next.next is not None:
        current = current.next
    current.next = None
    return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def delete_at_tail_iterative(head):
    # If list is empty or has one node, result is empty
    if head is None or head.next is None:
        return None          # Main logic: nothing left

    current = head
    # Stop at the second-last node
    while current.next.next:          # Check if there are at least two nodes ahead
        current = current.next

    current.next = None                # Main logic: remove last node
    return head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = Node(10)
head.next = Node(20)
head.next.next = Node(30)
print("Before:", end=" ")
print_list(head)   # 10 -> 20 -> 30
head = delete_at_tail_iterative(head)
print("After: ", end=" ")
print_list(head)   # 10 -> 20
```

---

## Recursive Delete with Helper Function (Inner Recursion)

### Explanation
This version encapsulates the recursion inside an inner helper function. The outer function simply calls the helper and returns its result. This keeps the recursion logic self‑contained.

### Approach
- Define an inner function `recurse(node)` that performs the recursive deletion.
- Call `recurse(head)` and return its result.
- The inner function follows the same logic as the original.

### Algorithm
1. Define `recurse(node)`:
   - If `node is None`: return `None`.
   - If `node.next is None`: return `None`.
   - Set `node.next = recurse(node.next)`.
   - Return `node`.
2. Return `recurse(head)`.

### Pseudocode
```
function delete_at_tail_helper(head):
    function recurse(node):
        if node is None:
            return None
        if node.next is None:
            return None
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

def delete_at_tail_helper(head):
    def recurse(node):
        # Base cases
        if node is None:
            return None
        if node.next is None:
            return None          # Main logic: delete last node
        # Recursive step
        node.next = recurse(node.next)   # Crucial: update link
        return node

    return recurse(head)                  # Call helper

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = Node(5)
head.next = Node(10)
head.next.next = Node(15)
print("Before:", end=" ")
print_list(head)   # 5 -> 10 -> 15
head = delete_at_tail_helper(head)
print("After: ", end=" ")
print_list(head)   # 5 -> 10
```

---

## Tail Recursive Delete at Tail (with Previous Pointer)

### Explanation
This variant uses a tail‑recursive helper that carries both the current node and the previous node. When the end is reached, the previous node's `next` is set to `None`, and the head is propagated back unchanged. The recursion is tail‑recursive because the helper's last action is the recursive call.

### Approach
- Define a helper `tail_rec(node, prev)`.
- If `node` is `None`, the list is empty → return `None`.
- If `node.next` is `None` (last node):
   - Set `prev.next = None` (if `prev` exists).
   - Return the original head (which is captured in an outer variable or passed along).
- Otherwise, recursively call `tail_rec(node.next, node)`.

### Algorithm
1. If `head` is `None`, return `None`.
2. If `head.next` is `None`, return `None` (single node case).
3. Define helper `tail_rec(node, prev)`:
   - If `node.next is None`:
     - `prev.next = None`
     - Return (no further action needed)
   - Else:
     - `tail_rec(node.next, node)`
4. Call `tail_rec(head.next, head)` (start with second node, prev = head).
5. Return `head`.

### Pseudocode
```
function delete_at_tail_tailrec(head):
    if head is None or head.next is None:
        return None
    function helper(node, prev):
        if node.next is None:
            prev.next = None
            return
        helper(node.next, node)
    helper(head.next, head)
    return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def delete_at_tail_tailrec(head):
    # Handle empty or single-node list
    if head is None or head.next is None:
        return None

    def helper(node, prev):
        if node.next is None:                # Found last node
            prev.next = None                  # Main logic: remove last node
            return                              # Tail return (no further recursion)
        helper(node.next, node)                # Tail recursive call

    helper(head.next, head)                    # Start with second node and its previous
    return head                                 # Return original head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = Node(2)
head.next = Node(4)
head.next.next = Node(6)
print("Before:", end=" ")
print_list(head)   # 2 -> 4 -> 6
head = delete_at_tail_tailrec(head)
print("After: ", end=" ")
print_list(head)   # 2 -> 4
```

---

## Two‑Pointer Iterative Delete at Tail

### Explanation
This iterative version uses two pointers: one (`curr`) that traverses and another (`prev`) that stays one step behind. When `curr` reaches the last node, `prev.next` is set to `None`.

### Approach
- If the list is empty or has one node, return `None`.
- Initialize `prev = None` and `curr = head`.
- Move `curr` until `curr.next` is `None` (last node). Keep `prev` as the node before `curr`.
- Set `prev.next = None`.
- Return head.

### Algorithm
1. If `head` is `None` or `head.next` is `None`, return `None`.
2. Set `prev = None`, `curr = head`.
3. While `curr.next` is not `None`:
   - `prev = curr`
   - `curr = curr.next`
4. `prev.next = None`.
5. Return `head`.

### Pseudocode
```
function delete_at_tail_two_pointer(head):
    if head is None or head.next is None:
        return None
    prev = None
    curr = head
    while curr.next:
        prev = curr
        curr = curr.next
    prev.next = None
    return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def delete_at_tail_two_pointer(head):
    if head is None or head.next is None:
        return None

    prev = None
    curr = head
    # Traverse until curr is the last node
    while curr.next:
        prev = curr
        curr = curr.next

    prev.next = None                         # Main logic: remove last node
    return head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = Node(3)
head.next = Node(6)
head.next.next = Node(9)
print("Before:", end=" ")
print_list(head)   # 3 -> 6 -> 9
head = delete_at_tail_two_pointer(head)
print("After: ", end=" ")
print_list(head)   # 3 -> 6
```

---

## Recursive Delete Returning (New Head, Deleted Data)

### Explanation
This variant not only deletes the tail but also returns the data of the deleted node, along with the new head. It uses a tuple or list to return both values, demonstrating how to extract information during recursion.

### Approach
- Base cases: empty list → return `(None, None)`; single node → return `(None, head.data)`.
- Recursive case: call on `head.next`, receiving `(new_next, deleted_data)`.
- Update `head.next` to `new_next`.
- Return `(head, deleted_data)`.

### Algorithm
1. If `head is None`: return `(None, None)`.
2. If `head.next is None`: return `(None, head.data)`.
3. `(new_next, deleted_data) = delete_at_tail_return_data(head.next)`
4. `head.next = new_next`
5. Return `(head, deleted_data)`.

### Pseudocode
```
function delete_at_tail_return_data(head):
    if head is None:
        return (None, None)
    if head.next is None:
        return (None, head.data)
    (new_next, deleted_data) = delete_at_tail_return_data(head.next)
    head.next = new_next
    return (head, deleted_data)
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def delete_at_tail_return_data(head):
    # Empty list
    if head is None:
        return (None, None)          # Return None head and no data

    # Single node: delete it, return its data
    if head.next is None:
        return (None, head.data)      # Main logic: return (None, data)

    # Recursive step
    new_next, deleted_data = delete_at_tail_return_data(head.next)
    head.next = new_next               # Crucial: update link
    return (head, deleted_data)        # Return head and deleted data

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = Node(7)
head.next = Node(14)
head.next.next = Node(21)
print("Before:", end=" ")
print_list(head)   # 7 -> 14 -> 21
head, deleted = delete_at_tail_return_data(head)
print(f"After (deleted {deleted}):", end=" ")
print_list(head)   # 7 -> 14
```

---

## Recursive Delete Using Stack (Simulated Recursion)

### Explanation
This variant uses an explicit stack (Python list) to simulate the recursive calls. It pushes nodes while traversing, then after reaching the end, it rebuilds the list without the last node by popping and re‑linking.

### Approach
- Push all nodes onto a stack while traversing.
- After traversal, if the stack is empty, list was empty → return `None`.
- If the stack has only one node, that node is the only one → return `None`.
- Otherwise, pop nodes until we have the second‑last node (the one just below the top). Set its `next` to `None`. The remaining nodes on the stack are the earlier ones, but they are already linked correctly because we haven't modified them. Actually, we only need to update the second‑last node's `next`. The rest remain unchanged.

### Algorithm
1. If `head is None`, return `None`.
2. Initialize an empty stack.
3. Set `current = head`.
4. While `current` is not `None`:
   - Push `current` onto stack.
   - `current = current.next`
5. If stack has only one node, return `None` (single node).
6. Pop the last node (top of stack) – it will be discarded.
7. The new top of stack is the second‑last node. Set its `next = None`.
8. Return the original head (which is still at the bottom of stack).

### Pseudocode
```
function delete_at_tail_stack(head):
    if head is None:
        return None
    stack = []
    current = head
    while current:
        stack.append(current)
        current = current.next
    if len(stack) == 1:
        return None
    stack.pop()          # remove last node
    second_last = stack[-1]
    second_last.next = None
    return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def delete_at_tail_stack(head):
    if head is None:
        return None

    stack = []
    current = head
    # Push all nodes onto stack
    while current:
        stack.append(current)
        current = current.next

    # If only one node, list becomes empty
    if len(stack) == 1:
        return None

    stack.pop()                              # Discard last node
    second_last = stack[-1]                   # Get second-last node
    second_last.next = None                    # Main logic: remove last node
    return head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = Node(4)
head.next = Node(8)
head.next.next = Node(12)
print("Before:", end=" ")
print_list(head)   # 4 -> 8 -> 12
head = delete_at_tail_stack(head)
print("After: ", end=" ")
print_list(head)   # 4 -> 8
```

---

## Recursive Delete Using List Rebuild

### Explanation
This variant collects all node data into a Python list, removes the last element, and then rebuilds the linked list from the remaining data. It is simple but inefficient for large lists.

### Approach
- Traverse the list and collect each node's data into a list.
- If the list is empty or becomes empty after removal, return `None`.
- Otherwise, rebuild the list from the data list (excluding the last element).

### Algorithm
1. If `head is None`, return `None`.
2. Initialize `values = []`.
3. `current = head`
4. While `current`:
   - Append `current.data` to `values`
   - `current = current.next`
5. If `len(values) == 1`, return `None`.
6. Remove last element: `values.pop()`.
7. Rebuild list: create new head from `values[0]`, then link subsequent nodes.
8. Return new head.

### Pseudocode
```
function delete_at_tail_rebuild(head):
    if head is None:
        return None
    values = []
    current = head
    while current:
        values.append(current.data)
        current = current.next
    if len(values) == 1:
        return None
    values.pop()   # remove last
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

def delete_at_tail_rebuild(head):
    if head is None:
        return None

    # Collect all data
    values = []
    current = head
    while current:
        values.append(current.data)
        current = current.next

    if len(values) == 1:
        return None

    values.pop()                              # Remove last element (main logic)

    # Rebuild list
    new_head = Node(values[0])
    current = new_head
    for val in values[1:]:
        current.next = Node(val)
        current = current.next

    return new_head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = Node(9)
head.next = Node(18)
head.next.next = Node(27)
print("Before:", end=" ")
print_list(head)   # 9 -> 18 -> 27
head = delete_at_tail_rebuild(head)
print("After: ", end=" ")
print_list(head)   # 9 -> 18
```

---

## Class‑Based Delete Method (Object‑Oriented)

### Explanation
This variant encapsulates the linked list in a class with a `head` attribute. The `delete_tail` method works on `self.head` directly and does not return anything (void), updating the head internally. This is a professional object‑oriented style.

### Approach
- Maintain `self.head` as the list's head.
- `delete_tail` method handles empty and single‑node cases by updating `self.head`.
- For longer lists, it traverses iteratively to the second‑last node and removes the last.

### Algorithm
1. If `self.head is None`, do nothing.
2. If `self.head.next is None`, set `self.head = None`.
3. Otherwise, set `current = self.head`.
4. While `current.next.next` is not `None`:
   - `current = current.next`
5. `current.next = None`.

### Pseudocode
```
class LinkedList:
    def __init__(self):
        self.head = None

    def delete_tail(self):
        if self.head is None:
            return
        if self.head.next is None:
            self.head = None
            return
        current = self.head
        while current.next.next:
            current = current.next
        current.next = None
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

    def delete_tail(self):
        # Empty list
        if self.head is None:
            return

        # Single node
        if self.head.next is None:
            self.head = None                     # Main logic: list becomes empty
            return

        # Traverse to second-last node
        current = self.head
        while current.next.next:                  # Check if two nodes ahead exist
            current = current.next
        current.next = None                        # Main logic: remove last node

    def display(self):
        current = self.head
        while current:
            print(current.data, end=" -> " if current.next else "")
            current = current.next
        print()

# Example
ll = LinkedList()
ll.head = Node(1)
ll.head.next = Node(2)
ll.head.next.next = Node(3)
print("Before:", end=" ")
ll.display()   # 1 -> 2 -> 3
ll.delete_tail()
print("After: ", end=" ")
ll.display()   # 1 -> 2
```

---

## Recursive Delete with Dummy Node (Simplified Base Cases)

### Explanation
This variant uses a dummy node that points to the head. The recursion works on the dummy's `next`, allowing us to treat the empty list uniformly and avoid special cases in the recursion itself.

### Approach
- Create a dummy node with `next = head`.
- Define a recursive function that takes a node (starting from dummy) and deletes the tail of the list following that node.
- The recursion stops when the node's `next` is `None` (meaning we are at the last node of the original list) or when `node.next.next` is `None` (i.e., node is second‑last). Actually, we want to delete the last node, so we stop when `node.next.next` is `None`, then set `node.next = None`.
- Return `dummy.next` as the new head.

### Algorithm
1. Create `dummy = Node(None)` and set `dummy.next = head`.
2. Define `recurse(node)`:
   - If `node.next is None` or `node.next.next is None`:
     - `node.next = None`
     - Return
   - Else:
     - `recurse(node.next)`
3. Call `recurse(dummy)`.
4. Return `dummy.next`.

### Pseudocode
```
function delete_at_tail_dummy(head):
    dummy = Node(None)
    dummy.next = head
    function recurse(node):
        if node.next is None or node.next.next is None:
            node.next = None
            return
        recurse(node.next)
    recurse(dummy)
    return dummy.next
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def delete_at_tail_dummy(head):
    dummy = Node(None)                     # Sentinel node
    dummy.next = head

    def recurse(node):
        # If node.next is None (list empty) or node.next.next is None (last node)
        if node.next is None or node.next.next is None:
            node.next = None                 # Main logic: remove tail
            return
        recurse(node.next)                    # Recursive step

    recurse(dummy)                            # Start from dummy
    return dummy.next                          # New head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = Node(11)
head.next = Node(22)
head.next.next = Node(33)
print("Before:", end=" ")
print_list(head)   # 11 -> 22 -> 33
head = delete_at_tail_dummy(head)
print("After: ", end=" ")
print_list(head)   # 11 -> 22
```