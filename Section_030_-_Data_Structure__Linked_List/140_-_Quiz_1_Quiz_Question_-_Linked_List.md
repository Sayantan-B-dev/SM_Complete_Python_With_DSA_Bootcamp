### 1. What is a linked list?
A) A linear data structure where elements are stored in contiguous memory locations.  
B) A linear data structure where each element is a separate object with a pointer to the next element.  
C) A hierarchical data structure where each node points to two children.  
D) A collection of elements that can be accessed only in LIFO order.

**Answer:** B  
**Explanation:** A linked list is a linear collection of nodes. Each node contains data and a reference (pointer) to the next node in the sequence. Unlike arrays, nodes are not stored in contiguous memory.

---

### 2. In a singly linked list, the time complexity to insert a new node at the beginning is:
A) O(1)  
B) O(n)  
C) O(log n)  
D) O(n²)

**Answer:** A  
**Explanation:** Insertion at the head only requires creating a new node, setting its `next` to the current head, and updating the head pointer. These are constant‑time operations, regardless of list size.

---

### 3. To delete the last node of a singly linked list, you must:
A) Directly free the last node using a pointer to it.  
B) Traverse to the second‑last node and set its `next` to `null`.  
C) Use a special `tail` pointer and delete the node it points to.  
D) Shift all elements one position left.

**Answer:** B  
**Explanation:** In a singly linked list, you have no direct link from the last node to the previous one. Therefore, you must traverse from the head until you reach the node whose `next` is the last node. Then you set that node’s `next` to `null`. This takes O(n) time.

---

### 4. What will be the output of the following code snippet (assuming a proper `Node` class and `printList` function)?
```python
head = Node(10)
head.next = Node(20)
head.next.next = Node(30)
head = head.next
printList(head)
```
A) 10 → 20 → 30  
B) 20 → 30  
C) 30  
D) Error

**Answer:** B  
**Explanation:** Initially, the list is 10 → 20 → 30. After `head = head.next`, `head` now points to the node containing 20. Printing from that node gives 20 → 30.

---

### 5. Which of the following is an advantage of a doubly linked list over a singly linked list?
A) Less memory usage per node.  
B) Easier insertion and deletion at the beginning.  
C) Ability to traverse in both directions.  
D) Faster search for a given value.

**Answer:** C  
**Explanation:** A doubly linked list node contains a `prev` pointer in addition to `next`, allowing traversal backward as well as forward. This is its main advantage. It does, however, use more memory.

---

### 6. In a circular linked list, how can you detect the end of the list while traversing?
A) A node with `next == NULL`.  
B) The last node points back to the head.  
C) You cannot; you must keep a counter.  
D) The traversal never ends.

**Answer:** B  
**Explanation:** In a circular linked list, there is no `null`. Instead, the last node’s `next` points to the first node (or the head). To stop traversal, you typically start at the head and stop when you return to it.

---

### 7. What is the time complexity of searching for an element in a singly linked list of size n?
A) O(1)  
B) O(log n)  
C) O(n)  
D) O(n log n)

**Answer:** C  
**Explanation:** Linked lists do not support random access. To find an element, you must traverse from the head, comparing each node’s data until you find it or reach the end. In the worst case, you visit all n nodes.

---

### 8. Compared to an array, a linked list is better for:
A) Random access of elements.  
B) Memory locality and cache performance.  
C) Frequent insertions and deletions in the middle.  
D) Storing a fixed number of elements.

**Answer:** C  
**Explanation:** Insertions and deletions in the middle of an array require shifting many elements. In a linked list, once you have the position, you can insert or delete by adjusting a few pointers, which is O(1) (assuming you already have a reference to the node). Random access is slower (O(n) vs O(1) for arrays).

---

### 9. Which of the following applications commonly uses a circular linked list?
A) Undo functionality in software.  
B) Implementation of a stack.  
C) Round‑robin scheduling in operating systems.  
D) Storing a sorted list of items.

**Answer:** C  
**Explanation:** Round‑robin scheduling cycles through processes in a circular order. A circular linked list naturally represents such a cyclic sequence. Undo/redo often uses a doubly linked list, stacks use arrays or singly linked lists.

---

### 10. To reverse a singly linked list iteratively, how many pointers do you typically need?
A) 1  
B) 2  
C) 3  
D) 4

**Answer:** C  
**Explanation:** The standard iterative reversal uses three pointers: `prev` (initially `NULL`), `curr` (head), and `next` (to temporarily store the next node). You loop through the list, reversing each link.

---

### 11. What does the following function do?
```python
def fun(head):
    if head is None or head.next is None:
        return head
    rest = fun(head.next)
    head.next.next = head
    head.next = None
    return rest
```
A) Counts the number of nodes.  
B) Deletes the last node.  
C) Reverses the linked list recursively.  
D) Finds the middle node.

**Answer:** C  
**Explanation:** This is a classic recursive reversal. It reverses the list after the current node, then makes the next node point back to the current node, and finally sets current’s `next` to `None`. The base case handles empty or single‑node lists.

---

### 12. Floyd’s cycle‑detection algorithm (tortoise and hare) is used to:
A) Find the middle of a linked list.  
B) Detect if a linked list has a loop.  
C) Reverse a linked list in place.  
D) Merge two sorted linked lists.

**Answer:** B  
**Explanation:** Floyd’s algorithm uses two pointers moving at different speeds. If there is a cycle, the fast pointer will eventually lap the slow pointer. This is a standard way to detect cycles without extra space.

---

### 13. You are given a pointer to a node (not the tail) in a singly linked list and you want to delete that node. Which of the following is correct?
A) Set the previous node’s `next` to the node’s `next`.  
B) Copy the data from the next node into the current node and then delete the next node.  
C) Shift all subsequent nodes one position left.  
D) It is impossible without the head pointer.

**Answer:** B  
**Explanation:** Since you don’t have access to the previous node, you cannot adjust its pointer. The common trick is to copy the data and `next` pointer from the next node into the current node, then skip over the next node (effectively deleting it). This works only if the given node is not the last node.

---

### 14. The worst‑case time complexity of inserting a node at a given position (if you have to first find that position) in a singly linked list is:
A) O(1)  
B) O(n)  
C) O(n²)  
D) O(log n)

**Answer:** B  
**Explanation:** You must traverse from the head to reach the desired position, which in the worst case (e.g., inserting near the end) requires visiting all n nodes. The actual insertion (pointer updates) is O(1) once the location is reached.

---

### 15. Which statement about memory usage in a linked list is true?
A) Each node uses exactly the same memory as an array element.  
B) Linked lists have no memory overhead compared to arrays.  
C) Each node requires extra memory for pointer(s).  
D) Linked lists always use less memory than arrays.

**Answer:** C  
**Explanation:** Every node in a linked list stores its data plus one or more pointers (next, and possibly prev). This is additional overhead compared to an array, which stores only the data in contiguous memory.

--- 

