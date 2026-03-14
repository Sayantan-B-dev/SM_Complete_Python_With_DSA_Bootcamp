# Recursive Search by Index in Singly Linked List

## 1. Data Structure Definition

A **singly linked list node** contains two components:

| Component | Purpose                                |
| --------- | -------------------------------------- |
| `data`    | Stores the value contained in the node |
| `next`    | Reference pointing to the next node    |

```
class Node:
    def __init__(self, data):
        # Stores actual value inside the node
        self.data = data
        
        # Pointer to the next node in the list
        self.next = None
```

### Example Linked List Representation

```
Head
 ↓
[10] → [20] → [30] → [40] → None
 0      1      2      3
```

Each node position corresponds to an **index** starting from `0`.

---

# 2. Problem Definition

**Goal**

Return the value stored at a specific index in a linked list using **recursion**.

If the requested index does not exist, return **None**.

Example:

| Index Requested | Returned Value |
| --------------- | -------------- |
| 0               | 10             |
| 2               | 30             |
| 5               | None           |

---

# 3. Recursive Algorithm Logic

Recursion works by **breaking a problem into smaller identical subproblems**.

Searching index `k` in a linked list can be converted into:

> Searching index `k-1` in the remaining list.

### Core Idea

Every recursive call:

* Moves **one node forward**
* Reduces **index by one**

Eventually index becomes `0`.

---

# 4. Recursive Implementation

```
def search_by_index(head, index):

    # Base Case 1:
    # If head becomes None before reaching index,
    # the list ended early therefore index does not exist.
    if head is None:
        return None

    # Base Case 2:
    # When index becomes zero we reached the desired node.
    if index == 0:
        return head.data

    # Recursive Step:
    # Move to next node and decrease index.
    return search_by_index(head.next, index - 1)
```

---

# 5. Step-By-Step Execution Example

Linked List

```
10 → 20 → 30 → 40
```

Search index **2**

```
search_by_index(head,2)
```

### Call 1

```
head = 10
index = 2
```

Index not zero.

Move forward.

```
search_by_index(head.next,1)
```

---

### Call 2

```
head = 20
index = 1
```

Index not zero.

Move forward.

```
search_by_index(head.next,0)
```

---

### Call 3

```
head = 30
index = 0
```

Base case reached.

Return

```
30
```

---

### Return Flow

```
Call3 → returns 30
Call2 → returns 30
Call1 → returns 30
```

Final Output

```
30
```

---

# 6. Recursion Visualization

```
search(10,2)
   ↓
search(20,1)
   ↓
search(30,0)
   ↓
return 30
```

---

# 7. Time and Space Complexity

| Metric           | Value                |
| ---------------- | -------------------- |
| Time Complexity  | O(n)                 |
| Space Complexity | O(n) recursion stack |

Explanation:

* Worst case traverses the entire list.
* Each recursive call consumes stack memory.

---

# 8. Alternative Method 1 — Iterative Approach

Recursion is not mandatory. A **loop based approach** is more memory efficient.

### Algorithm

1. Start from head
2. Move node by node
3. Maintain a counter
4. Stop when counter equals index

### Implementation

```
def search_by_index_iterative(head, index):

    # Counter to track current node index
    current_index = 0

    # Temporary pointer used for traversal
    current_node = head

    # Traverse until list ends
    while current_node is not None:

        # If the desired index is reached return value
        if current_index == index:
            return current_node.data

        # Move to next node
        current_node = current_node.next

        # Increment index counter
        current_index += 1

    # If loop finishes index does not exist
    return None
```

### Expected Output

```
search_by_index_iterative(head,2)

Output:
30
```

---

# 9. Alternative Method 2 — Using Length Validation

Another safe method:

1. Compute list length
2. Check if index valid
3. Traverse to position

### Implementation

```
def linked_list_length(head):

    length = 0
    temp = head

    # Count nodes until list ends
    while temp is not None:
        length += 1
        temp = temp.next

    return length


def search_by_index_with_validation(head, index):

    # First compute length of linked list
    length = linked_list_length(head)

    # If index exceeds length it is invalid
    if index >= length or index < 0:
        return None

    # Traverse until index position
    temp = head
    current_position = 0

    while current_position < index:
        temp = temp.next
        current_position += 1

    return temp.data
```

### Expected Output

```
search_by_index_with_validation(head,2)

Output:
30
```

---

# 10. Alternative Method 3 — Tail Recursive Style

A cleaner recursive approach using **helper parameters**.

### Idea

Carry the current index as recursion parameter.

### Implementation

```
def search_by_index_tail(head, target_index, current_index = 0):

    # If list ends before reaching index
    if head is None:
        return None

    # When current index equals target index
    if current_index == target_index:
        return head.data

    # Move forward in list and increase index
    return search_by_index_tail(
        head.next,
        target_index,
        current_index + 1
    )
```

### Expected Output

```
search_by_index_tail(head,2)

Output:
30
```

---

# 11. Comparison of Approaches

| Method            | Time        | Space | Notes                                           |
| ----------------- | ----------- | ----- | ----------------------------------------------- |
| Recursive         | O(n)        | O(n)  | Clean logic but stack usage                     |
| Iterative         | O(n)        | O(1)  | Most efficient in practice                      |
| Length Validation | O(n) + O(n) | O(1)  | Extra traversal                                 |
| Tail Recursion    | O(n)        | O(n)  | Similar to recursion but structured differently |

---

# 12. Key Concept Summary

* Linked lists require **sequential traversal**.
* Direct indexing like arrays is impossible.
* Recursion solves the problem by **reducing index each step**.
* Iterative solution is usually **preferred in production code** due to lower memory consumption.
