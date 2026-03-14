```python
# Helper functions assumed to be imported from common module:
# createLLFromList(lst) -> returns head of linked list built from list
# print_LL(head) -> prints the linked list

def merge_two_sorted_LL(head1, head2):
    """
    Merges two sorted singly linked lists into one sorted linked list.
    Returns the head of the merged list.
    """
    # If either list is empty, return the other list
    if head1 is None:
        return head2
    if head2 is None:
        return head1

    # Pointers to build the result list
    finalHead = None   # head of the merged list
    finalTail = None   # tail of the merged list (last node added)

    # Traverse both lists while both have nodes
    while head1 is not None and head2 is not None:
        # Compare the data of the current nodes
        if head1.data < head2.data:
            # Node from head1 should come next
            if finalHead is None:
                # First node being added
                finalHead = head1
                finalTail = head1
            else:
                # Append head1 to the result list
                finalTail.next = head1
                finalTail = head1
            # Move head1 forward
            head1 = head1.next
        else:
            # Node from head2 should come next (if equal, head2 is chosen; any order works)
            if finalHead is None:
                finalHead = head2
                finalTail = head2
            else:
                finalTail.next = head2
                finalTail = head2
            # Move head2 forward
            head2 = head2.next

    # After one list is exhausted, attach the remaining nodes of the other list
    if head1 is not None:
        finalTail.next = head1
    if head2 is not None:
        finalTail.next = head2

    # Return the head of the merged list
    return finalHead

# Example usage (assuming helper functions are imported)
head1 = createLLFromList([2, 5, 9, 10, 17])
head2 = createLLFromList([3, 6, 7])

print("List 1:")
print_LL(head1)
print("List 2:")
print_LL(head2)

# Merge the two sorted lists
merged_head = merge_two_sorted_LL(head1, head2)

print("Merged list:")
print_LL(merged_head)

# Edge case test: merging two empty lists
empty_merged = merge_two_sorted_LL(None, None)
print("Merged empty lists:", end=" ")
print_LL(empty_merged)   # Should print nothing (or "None" if implemented that way)
```

---

### Step‑by‑Step Explanation of `merge_two_sorted_LL`

#### 1. **Purpose**
The function merges two sorted singly linked lists into a single sorted linked list. It preserves the sorted order (non‑decreasing) by comparing node values and appending the smaller node to the result.

#### 2. **Edge Cases – Empty Lists**
```python
if head1 is None:
    return head2
if head2 is None:
    return head1
```
- If one of the input lists is empty (`head1 is None`), the result is simply the other list.  
- If both are `None`, the function returns `None` (since `head2` is `None`). This correctly handles the empty‑input scenario.

#### 3. **Initialization of Result Pointers**
```python
finalHead = None
finalTail = None
```
- `finalHead` will eventually point to the first node of the merged list.  
- `finalTail` always points to the last node added, making it easy to append new nodes without traversing the whole result each time.

#### 4. **Main Merging Loop**
```python
while head1 is not None and head2 is not None:
```
The loop continues as long as **both** lists still have nodes to process. Inside the loop:

- **Compare** the data values of the current nodes: `head1.data` and `head2.data`.  
- If `head1.data < head2.data`, we choose the node from `head1`; otherwise, we choose the node from `head2`. (If values are equal, we arbitrarily choose `head2` – both choices are valid for stability.)

**Adding the chosen node to the result list**:
- If `finalHead` is `None`, this is the very first node of the merged list. Set both `finalHead` and `finalTail` to that node.  
- Otherwise, append the node to the end:  
  ```python
  finalTail.next = chosen_node
  finalTail = chosen_node
  ```
  This attaches the node and updates the tail.

**Advance the pointer** of the list from which the node was taken:
```python
head1 = head1.next   # or head2 = head2.next
```

#### 5. **Attaching the Remaining Nodes**
After the loop, one of the lists may still have nodes (because the other list was exhausted).  
```python
if head1 is not None:
    finalTail.next = head1
if head2 is not None:
    finalTail.next = head2
```
- Since the remaining nodes are already sorted and larger than all nodes in the merged list, we simply attach the whole sublist to the tail.

#### 6. **Return the Merged List**
```python
return finalHead
```
- `finalHead` points to the first node of the newly merged list.

---

### Complexity Analysis
- **Time Complexity:** **O(m + n)** – each node from both lists is visited exactly once.  
- **Space Complexity:** **O(1)** – only a few pointer variables are used; no extra nodes are created.

---

### Important Notes
- The function **does not create new nodes**; it rearranges the existing nodes by changing `next` pointers. This is memory‑efficient.  
- The original lists (`head1` and `head2`) are effectively consumed – after merging, they no longer exist as separate lists.  
- The code assumes that the helper functions `createLLFromList` and `print_LL` are correctly implemented in the `common` module (as per the `from common import *` statement).  
- In the example, we merge `head1` and `head2` and print the result. The final call with `None,None` demonstrates the empty‑list edge case.