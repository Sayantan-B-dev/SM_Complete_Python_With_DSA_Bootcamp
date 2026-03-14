```python
# Helper functions assumed to be imported from common module:
# createLLFromList(lst) -> returns head of linked list built from list
# print_LL(head) -> prints the linked list
# ListNode class with attributes .data (or .val) and .next

def reverse_linked_list_iteration(head):
    """
    Iteratively reverses a singly linked list in place.
    Returns the new head of the reversed list.
    """
    # Edge case: empty list or single node is already reversed
    if head is None or head.next is None:
        return head

    # Initialize three pointers:
    # 'prev' will eventually become the new head; it starts as None
    # 'current' starts at the original head
    prev = None
    current = head

    # Traverse the list until we reach the end
    while current is not None:
        # 1. Save the next node before breaking the link
        next_node = current.next

        # 2. Reverse the current node's pointer: make it point to prev
        current.next = prev

        # 3. Move 'prev' one step forward (to the current node)
        prev = current

        # 4. Move 'current' one step forward (to the saved next_node)
        current = next_node

    # At the end, 'prev' points to the last node we processed,
    # which is the new head of the reversed list.
    return prev

# Example usage:
head = reverse_linked_list_iteration(createLLFromList([1, 2, 3, 4, 5]))
print_LL(head)   # Should print the reversed list: 5 4 3 2 1
```

---

## Step‑by‑Step Explanation of `reverse_linked_list_iteration`

### 1. **Purpose**
This function reverses a singly linked list **iteratively** by changing the `next` pointers of each node so that they point to the previous node instead of the next one. It modifies the list **in place** (no new nodes are created) and returns the new head of the reversed list.

### 2. **Edge‑Case Handling**
```python
if head is None or head.next is None:
    return head
```
- If the list is empty (`head is None`) or contains only one node (`head.next is None`), it is already reversed. Simply return the original head.

### 3. **Pointer Initialization**
```python
prev = None
current = head
```
- `prev` will always point to the node that comes **before** the current node in the original order. Initially, there is no previous node, so it is `None`.
- `current` starts at the original head and will move through the list.

### 4. **Iterative Reversal Loop**
```python
while current is not None:
    next_node = current.next   # (a) store the next node
    current.next = prev         # (b) reverse the pointer
    prev = current              # (c) move prev forward
    current = next_node         # (d) move current forward
```
- **Step (a):** Save the next node  
  Before we change `current.next` to point to `prev`, we need to remember where `current` originally pointed, so we can continue traversing. We store it in `next_node`.
- **Step (b):** Reverse the pointer  
  Make `current.next` point to the previous node (`prev`). This is the core reversal action.
- **Step (c):** Move `prev` forward  
  Now that `current` has been processed, it becomes the "previous" node for the next iteration. So we set `prev = current`.
- **Step (d):** Move `current` forward  
  Advance `current` to the next node that we saved earlier (`next_node`).

The loop continues until `current` becomes `None`, meaning we have processed all nodes.

### 5. **Return the New Head**
```python
return prev
```
- After the loop, `prev` points to the last node we processed – which, after all reversals, is the first node of the reversed list. This becomes the new head.

---

## Visual Example

Original list: `1 → 2 → 3 → 4 → 5 → None`

**Initial state:**
- `prev = None`, `current = 1`

**Iteration 1 (current = 1):**
- Save `next_node = 2`
- Set `1.next = None` (now `1` points to `None`)
- `prev = 1`
- `current = 2`  
  List becomes: `1 → None` (detached), rest `2 → 3 → 4 → 5 → None`

**Iteration 2 (current = 2):**
- Save `next_node = 3`
- Set `2.next = 1`
- `prev = 2`
- `current = 3`  
  Now `2 → 1 → None`, rest `3 → 4 → 5 → None`

**Iteration 3 (current = 3):**
- Save `next_node = 4`
- Set `3.next = 2`
- `prev = 3`
- `current = 4`  
  Now `3 → 2 → 1 → None`, rest `4 → 5 → None`

**Iteration 4 (current = 4):**
- Save `next_node = 5`
- Set `4.next = 3`
- `prev = 4`
- `current = 5`  
  Now `4 → 3 → 2 → 1 → None`, rest `5 → None`

**Iteration 5 (current = 5):**
- Save `next_node = None`
- Set `5.next = 4`
- `prev = 5`
- `current = None`  
  Now `5 → 4 → 3 → 2 → 1 → None`

Loop ends. `prev = 5` is returned as the new head.

---

## Complexity Analysis

- **Time Complexity:** **O(n)** – each node is visited exactly once.
- **Space Complexity:** **O(1)** – only a few pointer variables are used; no extra space proportional to input size.

This iterative approach is often preferred over recursion because it uses constant extra space, avoiding potential stack overflow for very long lists.