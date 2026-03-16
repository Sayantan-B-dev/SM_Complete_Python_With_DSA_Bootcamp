```py
class Node:
    def __init__(self,value):
        self.data = value
        self.next = None

class QueueUsingLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None
        self.len = 0

    def size(self):
        return self.len
    
    def isEmpty(self):
        return self.size() == 0
    
    def enqueue(self,data):
        newNode = Node(data)
        self.len +=1
        if(self.head is None): # My Queue is Empty
            self.head = newNode
            self.tail = newNode
        else:
            self.tail.next = newNode
            self.tail = newNode
        
        return f"Added {data} to the Queue"

    def front(self):
        if(self.isEmpty()):
            print ("Queue is Empty, cannot show Front")
            return

        return self.head.data

    def dequeue(self):
        if(self.isEmpty()):
            print ("Queue is Empty, cannot Dequeue")
            return
        self.len-=1
        dataToBeReturned = self.head.data
        self.head = self.head.next
        if(self.head == None): # Very Very Important to handle the Right Form of Queue 
            self.tail = None
        
        return dataToBeReturned


Q = QueueUsingLinkedList()

Q.enqueue(10)
Q.enqueue(20)
Q.enqueue(30)
print(Q.size())
print(Q.isEmpty())
print(Q.front())
print(Q.dequeue())
print(Q.dequeue())
print(Q.front())
print(Q.size())

# Visualization for the process
# https://www.cs.usfca.edu/~galles/visualization/QueueLL.html
```
## Queue Using Linked List – Implementation Documentation

This document provides a detailed analysis of the `QueueUsingLinkedList` class implemented in Python. The queue follows the First-In, First-Out (FIFO) principle and uses a singly linked list as the underlying data structure. Each node contains `data` and a `next` pointer.

### Class: `Node`
A simple helper class representing a single node in the linked list.
- **`__init__(self, value)`**: Initializes a node with `data = value` and `next = None`.

### Class: `QueueUsingLinkedList`
Maintains pointers to the `head` (front) and `tail` (rear) of the queue, along with a `len` counter for O(1) size queries.

---

## Method Analysis

Each method is examined in four parts: **incoming data**, **edge cases**, **working**, and **returning**.

---

### `__init__(self)`
- **Incoming data:** `self` (implicit).
- **Edge cases:** None.
- **Working:** Initializes instance variables:
  - `self.head = None` (front of queue).
  - `self.tail = None` (rear of queue).
  - `self.len = 0` (number of elements).
- **Returning:** Nothing (constructor).

---

### `size(self)`
- **Incoming data:** `self`.
- **Edge cases:** None.
- **Working:** Returns the current value of `self.len`, which is maintained during enqueue/dequeue.
- **Returning:** Integer representing the number of elements in the queue. Time complexity: O(1).

---

### `isEmpty(self)`
- **Incoming data:** `self`.
- **Edge cases:** None.
- **Working:** Compares `self.size()` (i.e., `self.len`) with 0.
- **Returning:** `True` if queue has no elements, otherwise `False`. Time complexity: O(1).

---

### `enqueue(self, data)`
- **Incoming data:** `self`, `data` (the value to be added).
- **Edge cases:**
  - Queue is empty (`self.head is None`).
  - Queue has one or more elements.
- **Working:**
  1. Creates a new `Node` with the given `data`.
  2. Increments `self.len` by 1.
  3. If queue is empty (`head is None`): sets both `head` and `tail` to the new node.
  4. Else: appends the new node after the current `tail` (`tail.next = newNode`), then updates `tail` to the new node.
- **Returning:** A confirmation string: `f"Added {data} to the Queue"`. In typical production use, a method like this would return `None` or nothing; the string here is for demonstration. Time complexity: O(1).

---

### `front(self)`
- **Incoming data:** `self`.
- **Edge cases:** Queue is empty (`self.isEmpty()` is `True`).
- **Working:**
  1. Checks if queue is empty. If so, prints an error message and returns `None` (early exit).
  2. Otherwise, accesses `self.head.data`.
- **Returning:** The data value at the front of the queue, or `None` (with printed error) if empty. Time complexity: O(1).

---

### `dequeue(self)`
- **Incoming data:** `self`.
- **Edge cases:**
  - Queue is empty.
  - Queue has exactly one element (after removal, both `head` and `tail` must become `None`).
  - Queue has multiple elements.
- **Working:**
  1. Checks if queue is empty. If so, prints an error and returns `None` (early exit).
  2. Decrements `self.len` by 1.
  3. Saves the data from the current `head` node (`self.head.data`).
  4. Moves `head` to the next node (`self.head = self.head.next`).
  5. **Critical edge case:** If `head` becomes `None` (meaning the queue is now empty), sets `tail` to `None` as well. This maintains consistency.
  6. Returns the saved data.
- **Returning:** The data value of the removed front element, or `None` (with error) if queue was empty. Time complexity: O(1).

---

### Example Usage and Output
The provided test code demonstrates basic operations:

```python
Q = QueueUsingLinkedList()
Q.enqueue(10)          # Returns: "Added 10 to the Queue"
Q.enqueue(20)          # "Added 20 to the Queue"
Q.enqueue(30)          # "Added 30 to the Queue"

print(Q.size())        # 3
print(Q.isEmpty())     # False
print(Q.front())       # 10
print(Q.dequeue())     # 10
print(Q.dequeue())     # 20
print(Q.front())       # 30
print(Q.size())        # 1
```

Output:
```
3
False
10
10
20
30
1
```

---

## Key Observations

1. **Tail Pointer Management:** The implementation correctly handles the tail pointer when the last element is dequeued (`self.head == None` leading to `self.tail = None`). This is essential to prevent a dangling reference.

2. **Size Counter:** Maintaining `self.len` ensures O(1) size queries. Without it, determining size would require O(n) traversal.

3. **Encapsulation:** The class does not use private attributes (no double underscore), so external code could potentially modify `head`, `tail`, or `len` directly, breaking the queue's integrity. For robust design, consider making them private.

4. **Error Handling:** `front()` and `dequeue()` print error messages and return `None` on underflow. This is acceptable for simple scripts but may hide issues. Raising a custom exception (e.g., `EmptyQueueError`) would be more explicit.

5. **Memory Management (Python-specific):** Python's garbage collector handles node deallocation automatically. When `head` is moved, the old head node becomes unreachable (if no other references exist) and is eventually collected. No explicit `free` is needed.

6. **Return Values:** `enqueue` returns a string, which is unusual for a queue method. Typical implementations return `None` or the new size. This design choice is fine for learning but not for general use.

---

## Visualization Resource

The linked implementation can be visualized step by step at:
**https://www.cs.usfca.edu/~galles/visualization/QueueLL.html**

This interactive tool allows you to see how `head` and `tail` pointers move during enqueue and dequeue operations, reinforcing the concepts described above. The animation speed can be adjusted, and each step is clearly shown.