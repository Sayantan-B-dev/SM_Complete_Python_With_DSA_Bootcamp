# LeetCode 237 — Delete Node in a Linked List

## Problem Statement

A node inside a singly linked list is provided instead of the head pointer. The given node is **not the tail node**. The task requires deleting this node from the linked list while maintaining correct list connectivity.

**Function Signature**

```python
def deleteNode(node):
```

**Key Constraint**

The head pointer is unavailable. Only the reference to the node requiring deletion is provided.

**Example**

| Input Linked List | Node to Delete | Resulting List |
| ----------------- | -------------- | -------------- |
| 4 → 5 → 1 → 9     | 5              | 4 → 1 → 9      |
| 4 → 5 → 1 → 9     | 1              | 4 → 5 → 9      |

---

# Solution 1 — Data Copy Technique (Optimal)

## Concept

The current node cannot be removed directly because the previous node reference is unknown. Therefore, the next node’s data is copied into the current node, and the next node is bypassed.

**Core Idea**

```
node.data = node.next.data
node.next = node.next.next
```

The next node effectively disappears from the list, simulating deletion of the given node.

---

## Algorithm

1. Access the next node of the given node.
2. Copy the next node's data into the current node.
3. Update the current node’s `next` pointer to skip the next node.
4. The next node becomes unreachable and effectively removed.

---

## Implementation

```python
class ListNode:
    def __init__(self, val=0, next_node=None):
        # Store value inside the node
        self.val = val
        
        # Pointer to next node in the list
        self.next = next_node


def deleteNode(node):
    """
    Deletes the given node from a singly linked list
    without access to the head pointer.

    Strategy:
    Copy the next node's data into the current node,
    then bypass the next node.
    """

    # Copy value from next node into current node
    node.val = node.next.val
    
    # Bypass the next node to remove it from the list
    node.next = node.next.next
```

---

### Expected Output

```
Input List
4 → 5 → 1 → 9

Delete Node = 5

Output List
4 → 1 → 9
```

---

# Solution 2 — Value Shifting Technique

## Concept

Instead of copying only the immediate next node, values are shifted leftwards until reaching the tail. The final node is then removed.

This method repeatedly moves values forward until the last node is isolated.

---

## Algorithm

1. Start from the node to delete.
2. Copy the value of the next node.
3. Move to the next node.
4. Continue until reaching the node before the tail.
5. Set its `next` pointer to `None`.

---

## Implementation

```python
class ListNode:
    def __init__(self, val=0, next_node=None):
        # Node value initialization
        self.val = val
        
        # Next pointer initialization
        self.next = next_node


def deleteNode(node):
    """
    Deletes the node by shifting values
    from subsequent nodes forward.
    """

    current_node = node

    # Shift values until reaching second last node
    while current_node.next.next is not None:
        
        # Copy value from next node
        current_node.val = current_node.next.val
        
        # Move forward in the list
        current_node = current_node.next

    # Copy value from last node
    current_node.val = current_node.next.val

    # Remove the final node
    current_node.next = None
```

---

### Expected Output

```
Input
4 → 5 → 1 → 9

Delete Node = 5

Output
4 → 1 → 9
```

---

# Solution 3 — Recursive Value Shift

## Concept

Recursively shift values forward until reaching the tail node. The final recursive call removes the last node.

---

## Algorithm

1. If next node is the tail, copy its value and remove it.
2. Otherwise copy next node’s value.
3. Recursively call function for the next node.

---

## Implementation

```python
class ListNode:
    def __init__(self, val=0, next_node=None):
        # Store node value
        self.val = val
        
        # Store next pointer
        self.next = next_node


def deleteNode(node):
    """
    Recursive solution shifting values forward
    until the tail node is removed.
    """

    # Base condition when next node is the last node
    if node.next.next is None:
        
        # Copy value from last node
        node.val = node.next.val
        
        # Remove last node
        node.next = None
        
        return

    # Copy value from next node
    node.val = node.next.val

    # Recursive call to shift remaining values
    deleteNode(node.next)
```

---

### Expected Output

```
Input
4 → 5 → 1 → 9

Delete Node = 1

Output
4 → 5 → 9
```

---

# Complexity Analysis

| Solution        | Time Complexity | Space Complexity | Notes                         |
| --------------- | --------------- | ---------------- | ----------------------------- |
| Data Copy       | **O(1)**        | **O(1)**         | Optimal and intended solution |
| Value Shifting  | O(n)            | O(1)             | Multiple data shifts          |
| Recursive Shift | O(n)            | O(n)             | Recursion stack overhead      |

---

# Key Insight

The classical deletion method requires access to the **previous node**, which is unavailable in this problem. Therefore, the deletion is simulated by overwriting the current node with its successor and bypassing that successor node. This trick allows constant-time deletion without head access.
