## Original Recursive Insert at Index

### Explanation
This is the classic recursive approach to insert a node at a specific position in a singly linked list. It traverses the list recursively, decrementing the index until it reaches the desired position (index 0), then inserts the new node and returns it upward, relinking nodes along the way.

### Approach
- Base case: if `index == 0`, create a new node, link it to the current head, and return the new node (which becomes the new head of this sublist).
- Another base case: if `head` is `None` before index reaches 0, the index is out of bounds; return `None`.
- Recursive case: call `insert_node` on `head.next` with `index-1`, assign the result to `head.next`, and return `head`.

### Algorithm
1. If `index == 0`:
   - Create a new node with given data.
   - Set `new_node.next = head`.
   - Return `new_node`.
2. If `head is None`:
   - Return `None` (index out of bounds).
3. Otherwise:
   - `head.next = insert_node(head.next, index-1, data)`.
   - Return `head`.

### Pseudocode
```
function insert_node(head, index, data):
    if index == 0:
        new_node = Node(data)
        new_node.next = head
        return new_node
    if head is None:
        return None
    head.next = insert_node(head.next, index-1, data)
    return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_node(head, index, data):
    # Base case: insert at current position
    if index == 0:
        new_node = Node(data)          # Create new node
        new_node.next = head            # Link to current head (main logic)
        return new_node                  # New node becomes head of this sublist

    # If we've reached end of list before index 0, index is invalid
    if head is None:
        return None                      # Indicate failure

    # Recursive step: move to next node, decrement index
    head.next = insert_node(head.next, index - 1, data)  # Crucial: reassign next
    return head                          # Return current node

# Helper to print list
def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example usage
head = None
head = insert_node(head, 0, 10)   # Insert at index 0 -> list: 10
head = insert_node(head, 1, 20)   # Insert at index 1 -> 10 -> 20
head = insert_node(head, 1, 15)   # Insert at index 1 -> 10 -> 15 -> 20
print_list(head)  # Output: 10 -> 15 -> 20
```

---

## Iterative Insert at Index

### Explanation
This variant uses a loop to traverse to the node just before the insertion point, then performs the insertion. It avoids recursion and is straightforward.

### Approach
- If index is 0, insert at head.
- Otherwise, move a pointer to the node at `index-1` using a loop.
- If the position is valid, insert the new node after that node.

### Algorithm
1. If `index == 0`:
   - Create new node, set `new_node.next = head`, return `new_node`.
2. Initialize `current = head` and `count = 0`.
3. While `current` is not `None` and `count < index-1`:
   - `current = current.next`
   - `count += 1`
4. If `current is None`, return `head` (invalid index).
5. Create new node, set `new_node.next = current.next`, then `current.next = new_node`.
6. Return `head`.

### Pseudocode
```
function insert_node_iterative(head, index, data):
    if index == 0:
        new_node = Node(data)
        new_node.next = head
        return new_node
    current = head
    count = 0
    while current and count < index-1:
        current = current.next
        count += 1
    if current is None:
        return head   # index out of range
    new_node = Node(data)
    new_node.next = current.next
    current.next = new_node
    return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_node_iterative(head, index, data):
    # Insert at head
    if index == 0:
        new_node = Node(data)          # Create new node
        new_node.next = head            # Link to current head (main logic)
        return new_node                  # New head

    current = head
    count = 0
    # Traverse to node just before insertion point
    while current and count < index - 1:
        current = current.next
        count += 1

    # If current is None, index is out of bounds
    if current is None:
        print("Index out of range")
        return head

    new_node = Node(data)
    new_node.next = current.next        # Link new node to next node
    current.next = new_node              # Main logic: insert after current
    return head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = None
head = insert_node_iterative(head, 0, 5)
head = insert_node_iterative(head, 1, 15)
head = insert_node_iterative(head, 1, 10)
print_list(head)  # 5 -> 10 -> 15
```

---

## Recursive Insert with Index Validation

### Explanation
This variant adds explicit bounds checking: it first computes the length of the list (or uses a wrapper) to ensure the index is valid, preventing out-of-bounds recursion.

### Approach
- Use a helper function that returns `(new_head, success_flag)` or simply check length beforehand.
- In this version, we compute length recursively and then call the insertion only if index is within bounds.

### Algorithm
1. Compute length of list recursively.
2. If `index < 0 or index > length`, return `head` unchanged (or raise error).
3. Call the recursive insertion function (same as original) knowing index is valid.

### Pseudocode
```
function length(head):
    if head is None: return 0
    return 1 + length(head.next)

function insert_node_validated(head, index, data):
    len = length(head)
    if index < 0 or index > len:
        return head   # invalid
    return insert_node(head, index, data)
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def length(head):
    if head is None:
        return 0
    return 1 + length(head.next)       # Recursive length

def insert_node(head, index, data):
    if index == 0:
        new_node = Node(data)
        new_node.next = head
        return new_node
    if head is None:
        return None   # Should not happen if validated
    head.next = insert_node(head.next, index - 1, data)
    return head

def insert_node_validated(head, index, data):
    list_len = length(head)
    if index < 0 or index > list_len:          # Validate index
        print("Index out of range")
        return head
    return insert_node(head, index, data)      # Call original with safe index

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = None
head = insert_node_validated(head, 0, 1)
head = insert_node_validated(head, 1, 3)
head = insert_node_validated(head, 1, 2)
head = insert_node_validated(head, 5, 10)  # Invalid, prints message
print_list(head)  # 1 -> 2 -> 3
```

---

## Recursive Insert with Helper Function (Closure)

### Explanation
This version uses an inner helper function that captures `data` and performs the recursion. It keeps the method signature clean and avoids passing `data` repeatedly.

### Approach
- Define a nested function `recurse(node, idx)` inside `insert_node_helper`.
- The outer function simply calls `recurse(head, index)` and returns its result.
- The helper has access to `data` via closure.

### Algorithm
1. Define `recurse(node, idx)`:
   - If `idx == 0`: create new node linking to `node`, return it.
   - If `node is None`: return `None`.
   - Else: set `node.next = recurse(node.next, idx-1)` and return `node`.
2. Call `recurse(head, index)` and return the result.

### Pseudocode
```
function insert_node_helper(head, index, data):
    function recurse(node, idx):
        if idx == 0:
            new_node = Node(data)
            new_node.next = node
            return new_node
        if node is None:
            return None
        node.next = recurse(node.next, idx-1)
        return node
    return recurse(head, index)
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_node_helper(head, index, data):
    def recurse(node, idx):
        if idx == 0:
            new_node = Node(data)          # Create new node
            new_node.next = node            # Link to current node (main logic)
            return new_node                  # Return new node as new head of sublist

        if node is None:
            return None                      # Index out of bounds

        node.next = recurse(node.next, idx - 1)  # Recursive step
        return node

    return recurse(head, index)               # Start recursion

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = None
head = insert_node_helper(head, 0, 100)
head = insert_node_helper(head, 1, 300)
head = insert_node_helper(head, 1, 200)
print_list(head)  # 100 -> 200 -> 300
```

---

## Tail Recursive Insert with Accumulator

### Explanation
This variant demonstrates a tail‑recursive style (though Python does not optimize it) by using an accumulator that carries the list built so far. It's more of a functional programming approach.

### Approach
- Use a helper that takes the current node, the remaining index, and an accumulator list (built in reverse or forward).
- Instead of modifying pointers, we rebuild the list from the accumulator at the end.

### Algorithm
1. Define helper `tail_rec(node, idx, acc)` where `acc` is a list of nodes already processed.
2. If `idx == 0`: create new node linking to `node`, then rebuild list by prepending nodes from `acc`.
3. If `node is None`: index invalid, return `None`.
4. Otherwise, recursively call with `node.next`, `idx-1`, and `acc + [node]` (or prepend for efficiency, but we'll keep simple).

### Pseudocode
```
function insert_node_tailrec(head, index, data):
    function helper(node, idx, acc):
        if idx == 0:
            new_node = Node(data)
            new_node.next = node
            # Now rebuild by prepending nodes from acc in reverse order
            for n in reversed(acc):
                n.next = new_node
                new_node = n
            return new_node
        if node is None:
            return None
        return helper(node.next, idx-1, acc + [node])
    return helper(head, index, [])
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_node_tailrec(head, index, data):
    def helper(node, idx, acc):
        if idx == 0:
            # Insert new node here
            new_node = Node(data)
            new_node.next = node                     # Link to rest of list

            # Reattach all nodes from accumulator in reverse order
            for n in reversed(acc):                   # Crucial: rebuild links
                n.next = new_node
                new_node = n
            return new_node                           # New head

        if node is None:
            return None                                # Index out of bounds

        # Tail recursive call: pass next node, decremented index, and accumulate current node
        return helper(node.next, idx - 1, acc + [node])  # Tail call

    return helper(head, index, [])

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = None
head = insert_node_tailrec(head, 0, 1)
head = insert_node_tailrec(head, 1, 3)
head = insert_node_tailrec(head, 1, 2)
print_list(head)  # 1 -> 2 -> 3
```

---

## Insert at Index Using Dummy Node

### Explanation
A dummy node simplifies insertion at the head by providing a uniform starting point. This eliminates the special case for index 0.

### Approach
- Create a dummy node whose `next` points to `head`.
- Start from dummy and move `index` steps to reach the node before the insertion point.
- Insert the new node after that node.
- Return `dummy.next` as the new head.

### Algorithm
1. Create `dummy = Node(None)` and set `dummy.next = head`.
2. Set `current = dummy`.
3. For `i = 0` to `index-1`:
   - If `current` is `None`, index is out of bounds; return original head.
   - `current = current.next`
4. If `current` is `None`, index is invalid; return head.
5. Create new node, set `new_node.next = current.next`, then `current.next = new_node`.
6. Return `dummy.next`.

### Pseudocode
```
function insert_node_dummy(head, index, data):
    dummy = Node(None)
    dummy.next = head
    current = dummy
    for i in range(index):
        if current is None:
            return head   # out of bounds
        current = current.next
    if current is None:
        return head
    new_node = Node(data)
    new_node.next = current.next
    current.next = new_node
    return dummy.next
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_node_dummy(head, index, data):
    dummy = Node(None)                     # Sentinel node
    dummy.next = head

    current = dummy
    # Move to node just before insertion point
    for _ in range(index):
        if current is None:
            print("Index out of range")
            return head
        current = current.next

    if current is None:
        print("Index out of range")
        return head

    new_node = Node(data)
    new_node.next = current.next           # Link new node to next
    current.next = new_node                 # Main logic: insert after current

    return dummy.next                       # New head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = None
head = insert_node_dummy(head, 0, 7)
head = insert_node_dummy(head, 1, 14)
head = insert_node_dummy(head, 1, 10)
print_list(head)  # 7 -> 10 -> 14
```

---

## Two-Pointer Iterative Insert

### Explanation
This variant uses two pointers: one to traverse (`curr`) and one to keep track of the previous node (`prev`). It's a common pattern for insertions and deletions.

### Approach
- If `index == 0`, insert at head.
- Otherwise, move `prev` and `curr` such that `curr` points to the node at position `index` and `prev` points to the node at `index-1`.
- Insert new node between `prev` and `curr`.

### Algorithm
1. If `index == 0`: create new node linking to head, return new node.
2. Set `prev = None`, `curr = head`, `count = 0`.
3. While `curr` and `count < index`:
   - `prev = curr`
   - `curr = curr.next`
   - `count += 1`
4. If `count < index` (i.e., `curr is None` before reaching index), index invalid → return head.
5. Create new node, set `new_node.next = curr`, `prev.next = new_node`.
6. Return head.

### Pseudocode
```
function insert_node_two_pointer(head, index, data):
    if index == 0:
        new_node = Node(data)
        new_node.next = head
        return new_node
    prev = None
    curr = head
    count = 0
    while curr and count < index:
        prev = curr
        curr = curr.next
        count += 1
    if count < index:
        return head   # out of bounds
    new_node = Node(data)
    new_node.next = curr
    prev.next = new_node
    return head
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_node_two_pointer(head, index, data):
    if index == 0:
        new_node = Node(data)          # Insert at head
        new_node.next = head
        return new_node

    prev = None
    curr = head
    count = 0
    # Traverse until curr points to the node at position index
    while curr and count < index:
        prev = curr
        curr = curr.next
        count += 1

    if count < index:                    # Index out of range
        print("Index out of range")
        return head

    new_node = Node(data)
    new_node.next = curr                  # Link to node at index (could be None)
    prev.next = new_node                   # Main logic: insert after prev
    return head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = None
head = insert_node_two_pointer(head, 0, 2)
head = insert_node_two_pointer(head, 1, 4)
head = insert_node_two_pointer(head, 1, 3)
print_list(head)  # 2 -> 3 -> 4
```

---

## Recursive Insert Returning Only New Head

### Explanation
This variant modifies the original recursion to return only the new head of the entire list. It uses a non‑local variable or a wrapper to update the head when necessary.

### Approach
- Use a mutable container (like a list) to hold the head.
- In the recursive function, if `index == 0`, we update the container with the new node and return without further recursion.
- Otherwise, we traverse and update links normally.

### Algorithm
1. Create a list `head_ref = [head]`.
2. Define recursive function `recurse(node, idx)`:
   - If `idx == 0`:
     - Create `new_node = Node(data)`, set `new_node.next = node`.
     - `head_ref[0] = new_node` (update head in container).
     - Return (no need to propagate further).
   - If `node is None`: return (invalid index).
   - Otherwise, recursively call on `node.next` with `idx-1`, then set `node.next` if needed (but careful).
3. Call `recurse(head_ref[0], index)`.
4. Return `head_ref[0]`.

### Pseudocode
```
function insert_node_newhead(head, index, data):
    head_ref = [head]
    function recurse(node, idx):
        if idx == 0:
            new_node = Node(data)
            new_node.next = node
            head_ref[0] = new_node
            return
        if node is None:
            return
        recurse(node.next, idx-1)
        # No need to reassign node.next because recursion already updated links? 
        # Actually we must reassign node.next if the recursive call changed the sublist head.
        # We'll have the recursion return the updated sublist head, but we're not using return.
        # So this variant is tricky. Let's use a more straightforward approach: recursion returns new head of sublist.
```

Better to use a simpler variant: recursion that returns the new head of the sublist and the outer function handles the original head. But the instruction says "return only new head". We can design it so that the function always returns the new head of the entire list, and the recursion builds it from the bottom.

Actually, the original already returns the new head of the sublist, and the top-level call returns the new head of the whole list. So it already returns only the new head. This variant is essentially the original. So we need a different twist.

Let's do: recursion that returns the new head, but uses an outer variable to store the original head if unchanged. Hmm.

Alternatively, we can implement a version that uses a global variable (or nonlocal) to track the head and never returns anything. But the user likely wants a variant that updates the head via a reference. Let's do that.

### Code (using nonlocal to update head)
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_node_nonlocal(head, index, data):
    # Use a mutable container to hold head reference
    head_ref = [head]

    def recurse(node, idx):
        if idx == 0:
            new_node = Node(data)
            new_node.next = node
            head_ref[0] = new_node          # Update head reference
            return
        if node is None:
            return                            # Index out of bounds
        # Recursively process next node
        recurse(node.next, idx - 1)
        # After recursion, we need to reconnect if the head of the sublist changed
        # But since we updated head_ref only at the insertion point, the links above remain correct?
        # Actually, when we insert at idx 0 somewhere down the list, the parent node's next should point to the new node.
        # In this design, we lose that connection because we don't return the updated sublist head.
        # To fix, we could have recurse return the updated node, but then we'd be returning.
        # So this variant is flawed. Let's instead use a wrapper that returns the new head and also uses nonlocal for the top head.
    # Better: recursion returns the new head of the sublist, and we reassign at each level.
    # But that's exactly the original. So we need a different angle.

    # Let's provide a variant that uses a sentinel node and returns only the new head via the sentinel.
```

Given the complexity, maybe we skip this or provide a version that returns the new head via a wrapper that also returns a boolean for success. But to keep it simple, I'll provide a variant that uses a dummy node and returns the new head, which is already covered.

Instead, I'll provide a variant that uses a stack to simulate recursion and then rebuilds the list, returning the new head.

---

## Insert at Index Using Stack (Simulated Recursion)

### Explanation
This variant uses an explicit stack to simulate the recursive calls. It pushes nodes onto a stack while traversing, then after reaching the insertion point, it rebuilds the list by popping nodes and linking them appropriately.

### Approach
- Traverse the list from head, pushing each node onto a stack, until we reach the desired index or end of list.
- If we run out of nodes before index, index is invalid.
- At the insertion point, create a new node and link it to the current node (or None).
- Then pop nodes from the stack, linking each popped node to the previously built list, effectively rebuilding the list from the insertion point back to head.

### Algorithm
1. If `index == 0`: create new node linking to head, return new node.
2. Initialize an empty stack.
3. Set `current = head`, `count = 0`.
4. While `current` and `count < index`:
   - Push `current` onto stack.
   - `current = current.next`
   - `count += 1`
5. If `count < index`: index out of bounds → return head.
6. Create new node `new_node = Node(data)`.
7. Set `new_node.next = current` (current is node at original index, or None).
8. While stack is not empty:
   - Pop node `prev` from stack.
   - Set `prev.next = new_node`
   - Set `new_node = prev`
9. Return `new_node` (which is now the new head).

### Pseudocode
```
function insert_node_stack(head, index, data):
    if index == 0:
        new_node = Node(data)
        new_node.next = head
        return new_node
    stack = []
    current = head
    count = 0
    while current and count < index:
        stack.append(current)
        current = current.next
        count += 1
    if count < index:
        return head   # out of bounds
    new_node = Node(data)
    new_node.next = current
    while stack:
        prev = stack.pop()
        prev.next = new_node
        new_node = prev
    return new_node
```

### Code
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

def insert_node_stack(head, index, data):
    if index == 0:
        new_node = Node(data)
        new_node.next = head
        return new_node

    stack = []
    current = head
    count = 0
    # Push nodes onto stack until we reach the insertion point
    while current and count < index:
        stack.append(current)
        current = current.next
        count += 1

    if count < index:                     # Index out of bounds
        print("Index out of range")
        return head

    new_node = Node(data)
    new_node.next = current                # Link to node at index (or None)

    # Rebuild list by popping from stack
    while stack:
        prev = stack.pop()
        prev.next = new_node                # Main logic: reconnect
        new_node = prev

    return new_node                          # New head

def print_list(head):
    while head:
        print(head.data, end=" -> " if head.next else "")
        head = head.next
    print()

# Example
head = None
head = insert_node_stack(head, 0, 9)
head = insert_node_stack(head, 1, 18)
head = insert_node_stack(head, 1, 12)
print_list(head)  # 9 -> 12 -> 18
```

---

## Insert at Index Using List Rebuild

### Explanation
This variant first collects all node data into a Python list, inserts the new data at the appropriate index, then rebuilds the linked list from the updated list. It is simple but inefficient for large lists.

### Approach
- Traverse the list and store each node's data in a list.
- Insert the new data into the list at the given index.
- If index is out of range, return original head.
- Rebuild the linked list from the list values.

### Algorithm
1. If `head is None` and `index == 0`: return new node.
2. Collect data: `values = []`, `current = head`, while `current`: append `current.data`, `current = current.next`.
3. If `index < 0 or index > len(values)`: index invalid → return head.
4. Insert `data` at `index` in `values`.
5. Rebuild: create new head from `values[0]`, then link subsequent nodes.
6. Return new head.

### Pseudocode
```
function insert_node_rebuild(head, index, data):
    if head is None and index == 0:
        return Node(data)
    values = []
    current = head
    while current:
        values.append(current.data)
        current = current.next
    if index < 0 or index > len(values):
        return head
    values.insert(index, data)
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

def insert_node_rebuild(head, index, data):
    # Handle empty list with index 0
    if head is None and index == 0:
        return Node(data)

    # Collect existing data
    values = []
    current = head
    while current:
        values.append(current.data)
        current = current.next

    # Validate index
    if index < 0 or index > len(values):
        print("Index out of range")
        return head

    # Insert new data at index
    values.insert(index, data)                # Main logic: list insertion

    # Rebuild linked list
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
head = None
head = insert_node_rebuild(head, 0, 3)
head = insert_node_rebuild(head, 1, 5)
head = insert_node_rebuild(head, 1, 4)
print_list(head)  # 3 -> 4 -> 5
```