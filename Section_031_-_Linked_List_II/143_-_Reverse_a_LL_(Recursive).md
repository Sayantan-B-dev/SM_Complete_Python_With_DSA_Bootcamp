```python
# Helper functions assumed to be imported from common module:
# ListNode class with attributes 'data' and 'next'
# createLLFromList(lst) -> returns head of linked list
# print_LL(head) -> prints the linked list

# -------------------- First Version (Inefficient) --------------------
def reverse_LL(head):
    """
    Recursively reverses a singly linked list.
    This version traverses the reversed sublist each time to find its tail,
    leading to O(n²) time complexity.
    """
    # Debug: print the current state of the list (from this node onward)
    print_LL(head)

    # Base Case: empty list or single node -> already reversed
    if head is None or head.next is None:
        return head

    # Step 1: Recursively reverse the rest of the list (head.next onwards)
    smallLinkedListHead = reverse_LL(head.next)

    # Step 2: Find the tail of the reversed smaller list
    temp = smallLinkedListHead
    while temp.next is not None:
        temp = temp.next

    # Step 3: Attach the current head at the end of the reversed smaller list
    temp.next = head

    # Step 4: Break the original forward link (head.next used to point to the old next)
    head.next = None

    # Step 5: Return the head of the fully reversed list
    return smallLinkedListHead


# -------------------- Second Version (Efficient) --------------------
def reverse_LL_with_tail_returned(head):
    """
    Recursively reverses a singly linked list and returns both the new head and the new tail.
    By returning the tail, we avoid traversing the reversed sublist, achieving O(n) time.
    """
    # Debug: print the current state
    print_LL(head)

    # Base Case: empty list or single node -> head is also tail
    if head is None or head.next is None:
        tail = head
        return head, tail

    # Step 1: Recursively reverse the rest of the list, obtaining its head and tail
    smallLinkedListHead, tail = reverse_LL_with_tail_returned(head.next)

    # Step 2: Attach the current head after the tail of the reversed smaller list
    tail.next = head

    # Step 3: Break the original forward link
    head.next = None

    # Step 4: Update the tail to be the current head (now the new last node)
    tail = head

    # Step 5: Return the new head and the updated tail
    return smallLinkedListHead, tail
```

---

## Step‑by‑Step Explanation

### `reverse_LL(head)` – Inefficient Recursive Reversal

#### 1. **Base Case**
```python
if head is None or head.next is None:
    return head
```
- If the list is empty (`head is None`) or has only one node (`head.next is None`), it is already reversed. Return the node itself.

#### 2. **Recursive Step**
```python
smallLinkedListHead = reverse_LL(head.next)
```
- Assume the function correctly reverses the sublist starting at `head.next`. After this call, `smallLinkedListHead` points to the **head of the reversed sublist**.

#### 3. **Find the Tail of the Reversed Sublist**
```python
temp = smallLinkedListHead
while temp.next is not None:
    temp = temp.next
```
- A temporary pointer traverses the reversed sublist until it reaches the last node (where `temp.next` is `None`). This is the **tail** of the reversed part.

#### 4. **Attach the Current Head at the End**
```python
temp.next = head
```
- The tail of the reversed sublist now points to the original `head` node, effectively appending it at the end.

#### 5. **Break the Original Forward Link**
```python
head.next = None
```
- The original `head` node used to point to the rest of the list. After reversal, it becomes the new tail, so its `next` must be set to `None`.

#### 6. **Return the New Head**
```python
return smallLinkedListHead
```
- The head of the fully reversed list is the same as the head of the reversed sublist (which was originally `head.next`).

**Drawback:** In each recursive call, we traverse the entire reversed sublist to find its tail. For a list of `n` nodes, the total time is the sum of lengths of the sublists traversed: approximately **O(n²)**.

---

### `reverse_LL_with_tail_returned(head)` – Efficient Recursive Reversal

#### 1. **Base Case**
```python
if head is None or head.next is None:
    tail = head
    return head, tail
```
- Same base case, but now we also return the tail. For an empty or single‑node list, the tail is the node itself (or `None` for empty).

#### 2. **Recursive Step (returns both head and tail)**
```python
smallLinkedListHead, tail = reverse_LL_with_tail_returned(head.next)
```
- The recursive call reverses the sublist `head.next` and **returns two values**: the new head (`smallLinkedListHead`) and the new tail (`tail`) of that reversed sublist.  
- Because we have the tail directly, we **do not need to traverse** the reversed part again.

#### 3. **Attach the Current Head After the Tail**
```python
tail.next = head
```
- The tail of the reversed sublist (which is the last node of that part) now points to the current `head`. This correctly places `head` at the end of the reversed list.

#### 4. **Break the Old Forward Link**
```python
head.next = None
```
- The current node becomes the new tail, so its `next` must be `None`.

#### 5. **Update the Tail**
```python
tail = head
```
- After attaching `head`, the new tail of the entire reversed list is `head`. Update the `tail` variable accordingly.

#### 6. **Return the New Head and the New Tail**
```python
return smallLinkedListHead, tail
```
- The head remains the head of the reversed sublist; the tail is now the current node.

**Advantage:** No extra traversal for finding the tail. Each recursive call does constant work (aside from the recursion itself). Total time becomes **O(n)**, with O(n) recursion stack space.

---

## Complexity Comparison

| Version                     | Time Complexity | Space Complexity (stack) | Notes                                    |
|-----------------------------|-----------------|--------------------------|------------------------------------------|
| `reverse_LL`                | O(n²)           | O(n)                     | Inefficient due to tail traversal        |
| `reverse_LL_with_tail_returned` | O(n)       | O(n)                     | Efficient, returns tail to avoid traversal |

The second version is the recommended recursive approach for reversing a linked list. Both modify the list in place and do not create new nodes.