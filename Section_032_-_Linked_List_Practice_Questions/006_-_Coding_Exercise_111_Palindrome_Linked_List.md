## Table of Contents
1. [Full Commented Code](#full-commented-code)
2. [Introduction to Linked Lists and Palindromes](#introduction-to-linked-lists-and-palindromes)
3. [Algorithm Overview](#algorithm-overview)
4. [Step-by-Step Explanation with ASCII Diagrams](#step-by-step-explanation-with-ascii-diagrams)
   - 4.1 Finding the Middle Node (Slow & Fast Pointers)
   - 4.2 Reversing the Second Half
   - 4.3 Comparing the Two Halves
5. [Complexity Analysis](#complexity-analysis)
6. [Recursive Approach for Palindrome Check](#recursive-approach-for-palindrome-check)
   - 6.1 Algorithm Description
   - 6.2 Recursion Tree Example
7. [Conclusion](#conclusion)

---

## 1. Full Commented Code

```python
# Definition for a singly linked list node.
class ListNode:
    def __init__(self, val=0, next=None):
        # The integer value stored in this node.
        self.val = val
        # Reference to the next node in the list (or None if this is the last node).
        self.next = next


def is_palindrome(head):
    """
    Checks whether the linked list starting at 'head' is a palindrome.

    A palindrome reads the same forwards and backwards. For a singly linked list,
    we cannot traverse backwards directly, so we modify the list by reversing
    the second half in-place and then compare it with the first half.

    Parameters:
    head (ListNode): The head node of the singly linked list.

    Returns:
    bool: True if the list is a palindrome, False otherwise.
    """

    # Edge case: an empty list or a list with a single node is trivially a palindrome.
    if head is None or head.next is None:
        return True

    # ---------- Step 1: Find the middle of the list ----------
    # Use the slow and fast pointer technique.
    # slow_pointer moves one step at a time, fast_pointer moves two steps.
    # When fast_pointer reaches the end, slow_pointer will be at the middle.
    slow_pointer = head
    fast_pointer = head

    while fast_pointer is not None and fast_pointer.next is not None:
        slow_pointer = slow_pointer.next          # move one step
        fast_pointer = fast_pointer.next.next    # move two steps

    # At this point, slow_pointer points to the middle node.
    # For an even-length list, it points to the first node of the second half.
    # For an odd-length list, it points to the exact middle node.

    # ---------- Step 2: Reverse the second half of the list ----------
    # We reverse the list starting from slow_pointer.
    previous_node = None
    current_node = slow_pointer

    while current_node is not None:
        # Save the next node before breaking the link.
        next_node_reference = current_node.next
        # Reverse the link: point current node to the previous node.
        current_node.next = previous_node
        # Move previous_node and current_node forward.
        previous_node = current_node
        current_node = next_node_reference

    # After reversal, previous_node becomes the head of the reversed second half.

    # ---------- Step 3: Compare the first half with the reversed second half ----------
    first_half_pointer = head
    second_half_pointer = previous_node

    # Traverse the reversed second half (which may be shorter if the list had odd length)
    while second_half_pointer is not None:
        if first_half_pointer.val != second_half_pointer.val:
            return False          # mismatch found, not a palindrome

        first_half_pointer = first_half_pointer.next
        second_half_pointer = second_half_pointer.next

    # All compared values matched, therefore it is a palindrome.
    return True
```

---

## 2. Introduction to Linked Lists and Palindromes

A **singly linked list** is a linear data structure where each node contains a value and a pointer to the next node. Traversal is only possible in one direction: from head to tail.

A **palindrome** is a sequence that reads the same forward and backward. For example, the list `1 -> 2 -> 3 -> 2 -> 1` is a palindrome because the values in reverse order are identical.

Because we cannot traverse backwards in a singly linked list, checking for a palindrome requires either:
- Using extra memory (like a stack or array) to store and compare values.
- Reversing part of the list in-place to simulate backward traversal.

The implemented algorithm uses the in-place reversal approach to achieve **O(1) extra space** (ignoring recursion stack if used) and **O(n) time**.

---

## 3. Algorithm Overview

The algorithm consists of three high‑level steps:

1. **Find the middle node** of the linked list using the slow/fast pointer technique.
2. **Reverse the second half** of the list starting from that middle node.
3. **Compare the first half** with the reversed second half node by node.

If all comparisons succeed, the list is a palindrome.

---

## 4. Step-by-Step Explanation with ASCII Diagrams

We will use the example list:  
`1 -> 2 -> 3 -> 2 -> 1`

### 4.1 Finding the Middle Node (Slow & Fast Pointers)

We initialize two pointers at the head:

```
slow_pointer = head
fast_pointer = head

List: 1 -> 2 -> 3 -> 2 -> 1
      ^
      slow, fast
```

**Loop condition:** `while fast is not None and fast.next is not None`

- **Iteration 1:**  
  Move `slow` one step: `slow = slow.next` → points to node `2`.  
  Move `fast` two steps: `fast = fast.next.next` → points to node `3`.

```
List: 1 -> 2 -> 3 -> 2 -> 1
           ^         ^
          slow      fast
```

- **Iteration 2:**  
  Move `slow` one step: now points to node `3`.  
  Move `fast` two steps: `fast.next` is node `2`, so `fast.next.next` is node `1`. Now `fast` points to node `1`.

```
List: 1 -> 2 -> 3 -> 2 -> 1
                ^              ^
               slow           fast
```

- **Iteration 3:**  
  Check condition: `fast.next` is `None` (since node `1` has no next). Loop stops.

**Result:** `slow_pointer` is at node `3`. For an odd-length list, this is the exact middle.

If the list were even, e.g., `1 -> 2 -> 2 -> 1`, the pointers would end with `slow` at the first node of the second half (node `2`).

### 4.2 Reversing the Second Half

We now reverse the sublist starting from `slow_pointer` (node `3`). We maintain three pointers: `prev`, `curr`, and `next_temp`.

Initial state:

```
prev = None
curr = slow_pointer   (node 3)
next_temp = None

List first half: 1 -> 2
List second half to reverse: 3 -> 2 -> 1
```

**Step A:** Save `curr.next` into `next_temp`, then set `curr.next = prev`.

```
next_temp = curr.next  (node 2)
curr.next = prev       (None)

Now: 3 -> None   (original next is lost temporarily)
prev = curr      (prev becomes node 3)
curr = next_temp (curr becomes node 2)
```

**Step B:** Repeat.

```
next_temp = curr.next  (node 1)
curr.next = prev       (node 3)
Now: 2 -> 3 -> None
prev = curr            (prev becomes node 2)
curr = next_temp       (curr becomes node 1)
```

**Step C:**

```
next_temp = curr.next  (None)
curr.next = prev       (node 2)
Now: 1 -> 2 -> 3 -> None
prev = curr            (prev becomes node 1)
curr = next_temp       (curr becomes None)
```

Loop ends. `prev` now points to the head of the reversed second half: `1 -> 2 -> 3 -> None`.

After reversal, the list structure becomes:

```
First half: 1 -> 2
Reversed second half: 1 -> 2 -> 3 -> None
```

But note that the original link between node `3` (now at the end of the reversed half) and node `2` (originally after it) is broken, and node `3` now points to `None`.

### 4.3 Comparing the Two Halves

We set two pointers:

```
p1 = head               (node 1)
p2 = previous_node      (node 1, head of reversed half)
```

**Compare:**

- `p1.val` (1) == `p2.val` (1) → continue.
- `p1 = p1.next` → node 2
- `p2 = p2.next` → node 2

- `p1.val` (2) == `p2.val` (2) → continue.
- `p1 = p1.next` → node 3
- `p2 = p2.next` → node 3

- `p1.val` (3) == `p2.val` (3) → continue.
- `p1 = p1.next` → node 2 (first half after middle)
- `p2 = p2.next` → None

Loop stops because `p2` is `None`. All values matched, return `True`.

**ASCII workflow summary:**

```
Initial: 1 -> 2 -> 3 -> 2 -> 1
Step1 (middle): slow at 3
Step2 (reverse second half):
  Original second half: 3 -> 2 -> 1
  After reversal:       1 -> 2 -> 3 -> None
Step3 (compare):
  p1: 1 -> 2 -> 3
  p2: 1 -> 2 -> 3 -> None
  Compare (1==1, 2==2, 3==3) -> True
```

---

## 5. Complexity Analysis

- **Time Complexity:** O(n)  
  - Finding the middle: O(n/2) ≈ O(n)  
  - Reversing the second half: O(n/2) ≈ O(n)  
  - Comparing: O(n/2) ≈ O(n)  
  All linear steps, so total linear.

- **Space Complexity:** O(1)  
  Only a few pointer variables are used, regardless of list size. No additional data structures.

---

## 6. Recursive Approach for Palindrome Check

Although the iterative in‑place method is efficient, a recursive solution can also check for a palindrome by using the call stack to simulate backward traversal. This approach uses O(n) extra space due to recursion stack.

### 6.1 Algorithm Description

1. Use a recursive function that traverses to the end of the list.
2. On the way back from recursion, compare the node values with a global pointer that moves forward from the head.
3. If any mismatch occurs, return `False`; otherwise, return `True`.

**Pseudo‑code:**

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def is_palindrome_recursive(head):
    # Global pointer that moves forward during unwinding
    front_pointer = head

    def recursive_check(current_node):
        nonlocal front_pointer
        if current_node is None:
            return True
        # Recursively go to the end
        if not recursive_check(current_node.next):
            return False
        # On return, compare with front pointer
        if current_node.val != front_pointer.val:
            return False
        front_pointer = front_pointer.next
        return True

    return recursive_check(head)
```

### 6.2 Recursion Tree Example

Consider the list `1 -> 2 -> 3 -> 2 -> 1`. We start with `recursive_check(head)`.

Let `fp` denote the `front_pointer` (initially at node 1).

**Recursive calls (going down):**

```
recursive_check(1)
  |-> recursive_check(2)
        |-> recursive_check(3)
              |-> recursive_check(2)
                    |-> recursive_check(1)
                          |-> recursive_check(None) -> returns True
```

**Unwinding (coming back up) with comparisons:**

- At `recursive_check(1)` (last node):  
  `current_node.val = 1`, `fp.val = 1` → equal.  
  Move `fp` to next (now at node 2). Return True.

- At `recursive_check(2)` (second last):  
  `current_node.val = 2`, `fp.val = 2` → equal.  
  Move `fp` to next (now at node 3). Return True.

- At `recursive_check(3)` (middle):  
  `current_node.val = 3`, `fp.val = 3` → equal.  
  Move `fp` to next (now at node 2). Return True.

- At `recursive_check(2)` (second node):  
  `current_node.val = 2`, `fp.val = 2` → equal.  
  Move `fp` to next (now at node 1). Return True.

- At `recursive_check(1)` (first node):  
  `current_node.val = 1`, `fp.val = 1` → equal.  
  Move `fp` to next (now at None). Return True.

All comparisons succeeded, final result `True`.

**Recursion tree (simplified):**

```
recursive_check(1) [waiting for child]
  └─ recursive_check(2) [waiting]
       └─ recursive_check(3) [waiting]
            └─ recursive_check(2) [waiting]
                 └─ recursive_check(1) [waiting]
                      └─ recursive_check(None) -> returns True
                 (1) compares 1==1, fp->2, returns True
            (2) compares 2==2, fp->3, returns True
       (3) compares 3==3, fp->2, returns True
  (2) compares 2==2, fp->1, returns True
(1) compares 1==1, fp->None, returns True
```

This approach is straightforward but uses O(n) stack space, which may be prohibitive for very long lists.

---

## 7. Conclusion

The iterative algorithm efficiently checks if a singly linked list is a palindrome by:

- Finding the middle (slow/fast pointers)
- Reversing the second half in‑place
- Comparing the halves

It runs in O(n) time and O(1) extra space, making it optimal for this problem. The recursive alternative, while elegant, uses O(n) stack space and is more suited for educational understanding or when recursion depth is not a concern.

Both methods rely on the same core idea: compare the first half with the reverse of the second half to verify palindrome property.