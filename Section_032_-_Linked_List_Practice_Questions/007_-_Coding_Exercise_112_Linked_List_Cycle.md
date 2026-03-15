## Table of Contents
1. [Full Commented Code](#full-commented-code)
2. [Introduction to Linked Lists and Cycle Detection](#introduction-to-linked-lists-and-cycle-detection)
3. [Algorithm Overview](#algorithm-overview)
4. [Step-by-Step Explanation with ASCII Diagrams](#step-by-step-explanation-with-ascii-diagrams)
   - 4.1 Example List with a Cycle
   - 4.2 Example List without a Cycle
5. [Complexity Analysis](#complexity-analysis)
6. [Recursive Approach for Cycle Detection](#recursive-approach-for-cycle-detection)
   - 6.1 Algorithm Description
   - 6.2 Recursion Tree Example
7. [Conclusion](#conclusion)

---

## 1. Full Commented Code

```python
class ListNode:
    def __init__(self, val=0, next=None):
        # Store the integer value contained in this node.
        self.val = val
        
        # Reference to the next node in the singly linked list.
        # If this is the last node, next is None.
        self.next = next


def has_cycle(head):
    """
    Determines whether a singly linked list contains a cycle.

    Algorithm Description:
    The algorithm uses Floyd's Cycle Detection technique, commonly
    called the tortoise and hare algorithm. Two traversal pointers
    move through the list at different speeds. The slow pointer moves
    one node per step while the fast pointer moves two nodes per step.
    If a cycle exists inside the linked list structure, both pointers
    will eventually meet at the same node due to the circular traversal.
    If the fast pointer reaches the end of the list, the structure does
    not contain any cycle.

    Parameters:
    head (ListNode): Head node of the singly linked list

    Returns:
    bool: True if a cycle exists inside the linked list structure,
          otherwise False if the list terminates normally.
    """

    # Initialize both pointers at the head of the list.
    slow_pointer = head
    fast_pointer = head

    # Traverse the list while fast pointer and its next reference exist.
    # This ensures we don't attempt to access None.next.
    while fast_pointer is not None and fast_pointer.next is not None:

        # Move slow pointer forward by one node.
        slow_pointer = slow_pointer.next

        # Move fast pointer forward by two nodes.
        fast_pointer = fast_pointer.next.next

        # If both pointers meet at the same node, a cycle exists.
        if slow_pointer == fast_pointer:
            return True

    # If the loop exits, fast pointer reached the end of the list (null),
    # meaning no cycle is present.
    return False
```

---

## 2. Introduction to Linked Lists and Cycle Detection

A **singly linked list** is a linear data structure where each node contains a value and a pointer to the next node. The list ends when a node's `next` pointer is `None`.

A **cycle** (or loop) occurs when a node's `next` pointer points back to a previous node, creating a circular path. Traversing such a list will never reach `None`; instead, it will loop indefinitely.

Detecting a cycle is important because algorithms that assume a linear list may run forever or cause errors. The classic solution is **Floyd's Cycle Detection Algorithm** (also known as the tortoise and hare algorithm), which uses two pointers moving at different speeds.

---

## 3. Algorithm Overview

Floyd's algorithm works as follows:

1. Initialize two pointers, `slow` and `fast`, both pointing to the head.
2. Move `slow` one step forward and `fast` two steps forward in each iteration.
3. If there is a cycle, `fast` will eventually catch up to `slow` from behind (they will meet inside the cycle).
4. If `fast` reaches `None` (end of list), then no cycle exists.

The reasoning: In a cycle, the relative speed of `fast` with respect to `slow` is one node per step, so they are guaranteed to meet after at most the length of the cycle steps.

---

## 4. Step-by-Step Explanation with ASCII Diagrams

We will illustrate the algorithm with two examples:

- A list that contains a cycle.
- A list that does not contain a cycle.

### 4.1 Example List with a Cycle

Consider the following linked list with a cycle:

```
1 -> 2 -> 3 -> 4 -> 5
          ^         |
          |_________|
```

Node values: 1, 2, 3, 4, 5. Node 5's `next` points back to node 3, forming a cycle.

**Initial state:**

```
slow = head (node 1)
fast = head (node 1)

List: [1] -> [2] -> [3] -> [4] -> [5]
       ^      ^      ^      ^      ^
      slow   fast  (others not pointed yet)
```

**Step 1:**  
Move `slow` one step → node 2.  
Move `fast` two steps → node 3.  

```
[1] -> [2] -> [3] -> [4] -> [5]
        ^              ^
       slow           fast
```

**Step 2:**  
Move `slow` one step → node 3.  
Move `fast` two steps: from node 3, next is node 4, then next.next is node 5. So `fast` → node 5.  

```
[1] -> [2] -> [3] -> [4] -> [5]
               ^              ^
              slow           fast
```

**Step 3:**  
Move `slow` one step → node 4.  
Move `fast` two steps: from node 5, next is node 3 (due to cycle), then next.next is node 4. So `fast` → node 4.  

```
[1] -> [2] -> [3] -> [4] -> [5]
                       ^
                  slow, fast (they meet)
```

At this point, `slow` and `fast` both point to node 4 → they meet, so the function returns `True`.

**Meeting proof:** In a cycle, the distance between `slow` and `fast` reduces by one each step (since `fast` gains one step relative to `slow` per iteration). Eventually they coincide.

### 4.2 Example List without a Cycle

Consider a linear list: `1 -> 2 -> 3 -> 4 -> 5 -> None`

**Initial state:**

```
slow = head (1)
fast = head (1)

[1] -> [2] -> [3] -> [4] -> [5] -> None
 ^
 slow, fast
```

**Step 1:**  
`slow` → 2, `fast` → 3  

```
[1] -> [2] -> [3] -> [4] -> [5] -> None
        ^       ^
       slow    fast
```

**Step 2:**  
`slow` → 3, `fast` → 5  

```
[1] -> [2] -> [3] -> [4] -> [5] -> None
                ^              ^
               slow           fast
```

**Step 3:**  
`slow` → 4, `fast` would move to `fast.next.next`. But `fast.next` is `5`, and `5.next` is `None`. So `fast` becomes `None`. The loop condition `fast is not None` fails, and we exit the while loop. Return `False`.

Thus, no cycle detected.

---

## 5. Complexity Analysis

- **Time Complexity:** O(n)  
  In the worst case (no cycle), `fast` traverses the entire list once, visiting about n/2 nodes, which is linear. In the presence of a cycle, the pointers meet after at most a linear number of steps (the distance before entering the cycle plus the cycle length). So overall O(n).

- **Space Complexity:** O(1)  
  Only two pointers are used, regardless of list size. No additional data structures.

---

## 6. Recursive Approach for Cycle Detection

Although Floyd's algorithm is iterative and optimal, a recursive method can also detect cycles by using a hash set to remember visited nodes. This approach, however, uses O(n) extra space due to the set and recursion stack.

### 6.1 Algorithm Description

1. Define a recursive function that takes a node and a set of visited nodes.
2. If the node is `None`, return `False` (reached end, no cycle).
3. If the node is already in the set, a cycle is detected → return `True`.
4. Otherwise, add the node to the set and recursively call the function on `node.next`.

This is essentially a depth‑first traversal that stops when a node is revisited.

**Pseudo‑code:**

```python
def has_cycle_recursive(node, visited=None):
    if visited is None:
        visited = set()
    if node is None:
        return False
    if node in visited:
        return True
    visited.add(node)
    return has_cycle_recursive(node.next, visited)
```

### 6.2 Recursion Tree Example

Take the same cyclic list: `1 -> 2 -> 3 -> 4 -> 5 -> (back to 3)`. Let `visited` be passed down.

**Recursive calls:**

```
has_cycle_recursive(1, {})
  -> node 1 not in visited → add 1, call on node 2
      has_cycle_recursive(2, {1})
        -> node 2 not in visited → add 2, call on node 3
            has_cycle_recursive(3, {1,2})
              -> node 3 not in visited → add 3, call on node 4
                  has_cycle_recursive(4, {1,2,3})
                    -> node 4 not in visited → add 4, call on node 5
                        has_cycle_recursive(5, {1,2,3,4})
                          -> node 5 not in visited → add 5, call on node 3
                              has_cycle_recursive(3, {1,2,3,4,5})
                                -> node 3 is in visited! → return True
                          <- receives True from child, propagates True
                    <- receives True, propagates True
              <- receives True, propagates True
        <- receives True, propagates True
  <- receives True, returns True
```

**Recursion tree (simplified):**

```
has_cycle_recursive(1)
  └─ has_cycle_recursive(2)
       └─ has_cycle_recursive(3)
            └─ has_cycle_recursive(4)
                 └─ has_cycle_recursive(5)
                      └─ has_cycle_recursive(3) → returns True
                 ← returns True
            ← returns True
       ← returns True
  ← returns True
```

At the point when node 3 is revisited, the recursion unwinds, passing `True` back up. The final result is `True`.

If the list had no cycle, the recursion would eventually hit `None` and return `False` all the way up.

---

## 7. Conclusion

The iterative Floyd's cycle detection algorithm is the standard solution for detecting cycles in a singly linked list. It runs in linear time and constant extra space, making it both time‑ and memory‑efficient. The recursive approach, while conceptually simple, uses O(n) space and is not recommended for large lists due to potential stack overflow. The step‑by‑step ASCII diagrams illustrate clearly how the pointers move and how a cycle causes them to meet.