```python
# Definition for singly-linked list node
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val      # value stored in the node
        self.next = next    # reference to the next node (None if tail)

# Helper function to compute the length of a linked list
def lengthOfLL(head):
    """
    Returns the number of nodes in the linked list starting at 'head'.
    """
    count = 0
    current = head
    while current:          # traverse until the end (None)
        count += 1
        current = current.next
    return count

# Function to find the middle node of a linked list
def middleOfLL(head):
    """
    Returns the middle node of the linked list.
    If the list has an even number of nodes, returns the first of the two middle nodes
    (based on integer division of length by 2).
    """
    # Edge case: empty list or single node -> return head as is
    if head is None or head.next is None:
        return head

    # Step 1: get the total length of the list
    length = lengthOfLL(head)

    # Step 2: compute the index of the middle node (0-based)
    # For odd length e.g. 5, middle = 2 (nodes at indices 0,1,2 -> 3rd node)
    # For even length e.g. 6, middle = 3 (nodes 0,1,2,3 -> 4th node; first of two middles)
    middle = length // 2

    # Step 3: traverse from head to the middle node
    temp = head
    count = 0
    while count < middle:
        temp = temp.next
        count += 1

    # Step 4: return the node found
    return temp
```

---

### Step‑by‑Step Explanation of `middleOfLL`

#### 1. **Purpose**
The function `middleOfLL` returns the middle node of a singly linked list.  
- If the list has an **odd** number of nodes (e.g., 5), it returns the exact middle node (the 3rd node).  
- If the list has an **even** number of nodes (e.g., 6), it returns the **first** of the two middle nodes (the 4th node).

#### 2. **Edge‑case handling**
```python
if head is None or head.next is None:
    return head
```
- If the list is empty (`head is None`), there is no node to return → `None` is returned.  
- If the list has only one node (`head.next is None`), that node itself is the middle → return `head`.

#### 3. **Compute the total length**
```python
length = lengthOfLL(head)
```
- The helper function `lengthOfLL` traverses the entire list, counting nodes, and returns the total number of nodes.  
- This step costs **O(n)** time, where `n` is the number of nodes.

#### 4. **Determine the middle index**
```python
middle = length // 2
```
- Integer division by 2 gives the **0‑based index** of the desired node.  
  - Example: length = 5 → `5 // 2 = 2` → node at index 2 (the 3rd node).  
  - length = 6 → `6 // 2 = 3` → node at index 3 (the 4th node, first of the two middles).

#### 5. **Traverse to the middle node**
```python
temp = head
count = 0
while count < middle:
    temp = temp.next
    count += 1
```
- Start at the `head` and move forward `middle` steps using a `count` variable.  
- After the loop, `temp` points exactly to the middle node.

#### 6. **Return the node**
```python
return temp
```
- The node (with its `val` and `next` attributes) is returned.

---

### Complexity Analysis
- **Time Complexity:** **O(n)** – we traverse the list twice (once for length, once to reach the middle). In practice this is `O(n) + O(n/2) = O(n)`.  
- **Space Complexity:** **O(1)** – only a few variables are used, no additional data structures.

---

### Alternative Approach (Fast & Slow Pointers)
A more efficient one‑pass solution uses two pointers:
- `slow` moves one step at a time, `fast` moves two steps.
- When `fast` reaches the end, `slow` is at the middle.
- That method also runs in **O(n)** time but only one traversal, and handles even/odd cases naturally. However, the provided code uses the length‑based method, which is simpler to understand conceptually.