```python
# Node Definition for Singly Linked List
# Each node stores an integer value and a pointer to the next node
class Node:
    def __init__(self, data):
        # Store the value inside the node
        self.data = data
        # Pointer to the next node; initially None because the node is not yet connected
        self.next = None


# Utility Function: Print Linked List
# Traverses the list from the head and prints all node values in order
def print_linked_list(head):
    current_node = head
    while current_node is not None:
        print(current_node.data, end=" -> ")
        current_node = current_node.next
    print("None")


# Step 1: Find the Middle of the Linked List
# Uses the slow and fast pointer technique (also known as tortoise and hare)
# - slow_pointer moves one step at a time
# - fast_pointer moves two steps at a time
# When fast_pointer reaches the end, slow_pointer points to the middle node.
# For an even number of nodes, this returns the first of the two middle nodes.
def find_middle(head):
    slow_pointer = head
    fast_pointer = head.next  # start fast one step ahead to get the correct middle when splitting

    while fast_pointer is not None and fast_pointer.next is not None:
        slow_pointer = slow_pointer.next
        fast_pointer = fast_pointer.next.next

    return slow_pointer


# Step 2: Merge Two Sorted Linked Lists
# Recursively merges two sorted linked lists into one sorted list.
# Returns the head of the merged list.
def merge_sorted_lists(head1, head2):
    # If one list is empty, return the other list
    if head1 is None:
        return head2
    if head2 is None:
        return head1

    # Compare the data of the current nodes of both lists
    if head1.data <= head2.data:
        # The smaller node (head1) becomes the new head of the merged list
        merged_head = head1
        # Recursively merge the rest of head1's list with head2
        merged_head.next = merge_sorted_lists(head1.next, head2)
    else:
        # head2 is smaller
        merged_head = head2
        # Recursively merge head1 with the rest of head2's list
        merged_head.next = merge_sorted_lists(head1, head2.next)

    return merged_head


# Step 3: Merge Sort Function for Linked List
# Implements the merge sort algorithm on a singly linked list.
# Algorithm steps:
#   1. Find the middle node of the list.
#   2. Split the list into two halves by setting the next of the middle node to None.
#   3. Recursively sort both halves.
#   4. Merge the two sorted halves.
def merge_sort_linked_list(head):
    # Base case: if the list is empty or has only one node, it is already sorted
    if head is None or head.next is None:
        return head

    # Find the middle node
    middle_node = find_middle(head)

    # Split the list into two halves:
    #   first half starts at head and ends at middle_node
    #   second half starts at middle_node.next
    second_half_head = middle_node.next
    middle_node.next = None  # terminate the first half

    # Recursively sort both halves
    left_sorted_list = merge_sort_linked_list(head)
    right_sorted_list = merge_sort_linked_list(second_half_head)

    # Merge the two sorted halves
    sorted_head = merge_sorted_lists(left_sorted_list, right_sorted_list)

    return sorted_head


# Helper Function: Insert Node at Tail (for building the example list)
def insert_at_tail(head, data):
    new_node = Node(data)
    if head is None:
        return new_node
    current_node = head
    while current_node.next is not None:
        current_node = current_node.next
    current_node.next = new_node
    return head


# Example Execution
if __name__ == "__main__":
    head = None

    # Construct an unsorted linked list: 4 -> 2 -> 1 -> 3
    head = insert_at_tail(head, 4)
    head = insert_at_tail(head, 2)
    head = insert_at_tail(head, 1)
    head = insert_at_tail(head, 3)

    print("Original Linked List:")
    print_linked_list(head)

    # Perform merge sort on the linked list
    head = merge_sort_linked_list(head)

    print("Sorted Linked List:")
    print_linked_list(head)

# Expected Output:
# Original Linked List:
# 4 -> 2 -> 1 -> 3 -> None
# Sorted Linked List:
# 1 -> 2 -> 3 -> 4 -> None
```

---

## Step‑by‑Step Explanation of Merge Sort on a Linked List

This code implements the **merge sort algorithm** for a singly linked list. Merge sort is a divide‑and‑conquer algorithm that works well on linked lists because it does not require random access and can be implemented with only sequential access.

### 1. **Node Class**
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
```
- Each node holds an integer `data` and a reference `next` to the subsequent node.  
- When a node is created, its `next` is set to `None` – it is an isolated node.

### 2. **Print Linked List**
```python
def print_linked_list(head):
    ...
```
- Traverses from the `head` and prints each node’s data, ending with `"None"` to indicate the end.  
- Used for debugging and verifying the result.

### 3. **Find Middle (`find_middle`)**
- **Purpose:** Locate the middle node of the list to split it into two halves.  
- **Technique:** Slow and fast pointers (tortoise and hare).  
  - `slow_pointer` moves one step per iteration.  
  - `fast_pointer` starts one step ahead (`head.next`) and moves two steps.  
- **Loop condition:** `fast_pointer` and `fast_pointer.next` are not `None`.  
  - This ensures that when the loop ends, `slow_pointer` is at the node just before the midpoint in a way that splitting at `slow_pointer.next` gives two balanced halves.  
  - For an odd number of nodes (e.g., 5), `slow_pointer` ends at the 3rd node (exact middle). For an even number (e.g., 6), it ends at the 3rd node (first of the two middle nodes), so the split yields a left half of size 3 and a right half of size 3.  
- **Return:** The middle node.

### 4. **Merge Two Sorted Lists (`merge_sorted_lists`)**
- **Purpose:** Combine two already sorted linked lists into one sorted list.  
- **Base cases:** If one list is empty, return the other.  
- **Recursive merging:**  
  - Compare the data of the current heads of both lists.  
  - The smaller node becomes the head of the merged list.  
  - Recursively merge the remaining part of that list with the other list, and attach the result to the chosen node’s `next`.  
- This function returns the head of the merged, sorted list.

### 5. **Merge Sort on Linked List (`merge_sort_linked_list`)**
- **Base case:** If the list is empty or has one node, it is already sorted – return it.  
- **Divide:**  
  - Find the middle node using `find_middle`.  
  - Split the list into two halves:  
    - `second_half_head = middle_node.next` (the start of the right half).  
    - `middle_node.next = None` (terminate the left half).  
- **Conquer:**  
  - Recursively sort the left half (`merge_sort_linked_list(head)`).  
  - Recursively sort the right half (`merge_sort_linked_list(second_half_head)`).  
- **Combine:**  
  - Merge the two sorted halves using `merge_sorted_lists`.  
- **Return:** The head of the fully sorted list.

### 6. **Helper Function (`insert_at_tail`)**
- Used only in the example to build the initial unsorted list. It appends a new node at the end.

### 7. **Example Execution**
- Constructs the list `4 → 2 → 1 → 3`.  
- Prints the original list.  
- Calls `merge_sort_linked_list` to sort it.  
- Prints the sorted list `1 → 2 → 3 → 4`.

---

## Complexity Analysis

- **Time Complexity:** **O(n log n)**  
  - The list is divided in half at each recursion level (log n levels).  
  - Merging at each level processes all n nodes, costing O(n) per level.  
- **Space Complexity:** **O(log n)** due to the recursion stack (each recursive call adds a frame). This is better than the O(n) space required for arrays because we are not creating new lists – we only rearrange pointers. The merging is done in place.

---

## Key Points

- **No random access needed** – only sequential traversal, making merge sort ideal for linked lists.  
- **In‑place merging** – we only change `next` pointers, no extra nodes are created.  
- The middle‑finding logic ensures a balanced split, which guarantees the O(n log n) performance.  
- The recursive implementation is clean and mirrors the divide‑and‑conquer nature of merge sort.