## Queue Operations – Detailed Explanation

A queue is a linear data structure following First-In, First-Out (FIFO). Operations are restricted to the ends: additions at the rear, removals from the front.

### 1. Enqueue
- **Purpose:** Add an element to the back (rear) of the queue.
- **Process:** The new element becomes the new rear. If the queue was empty, it also becomes the front.
- **Time Complexity:** O(1) for both array-based (if space is available) and linked-list implementations.
- **Edge Cases:** In a fixed-size array, enqueue may fail if the queue is full (overflow). In dynamic implementations, it always succeeds.

### 2. Dequeue
- **Purpose:** Remove and return the element at the front of the queue.
- **Process:** The front element is removed; the next element becomes the new front. If the queue becomes empty, front and rear become `null` or -1.
- **Time Complexity:** O(1).
- **Edge Cases:** Attempting to dequeue from an empty queue causes underflow – should be handled (return error or `null`).

### 3. Front (or Peek)
- **Purpose:** Retrieve the element at the front without removing it.
- **Process:** Returns the value of the front element; queue remains unchanged.
- **Time Complexity:** O(1).
- **Edge Cases:** If queue is empty, front operation may return a sentinel or raise an exception.

### 4. Rear
- **Purpose:** Retrieve the element at the back without removing it.
- **Process:** Returns the value of the rear element.
- **Time Complexity:** O(1) in most implementations (requires direct access to rear).
- **Edge Cases:** Empty queue – same as front.

### 5. Size
- **Purpose:** Return the number of elements currently in the queue.
- **Process:** Usually maintained as a counter that increments on enqueue and decrements on dequeue.
- **Time Complexity:** O(1).

### 6. isEmpty
- **Purpose:** Check whether the queue has no elements.
- **Process:** Returns `true` if size == 0, else `false`.
- **Time Complexity:** O(1).

---

## Implementation Plan (No Code)

### Using an Array (List)
- **Idea:** Use a fixed-size or dynamic array with two indices: `front` and `rear`. Initially `front = rear = -1` (empty).
- **Enqueue:**  
  - If empty, set `front = rear = 0` and place element.  
  - Else increment `rear` (modulo capacity for circular) and place element.  
- **Dequeue:**  
  - If empty, underflow.  
  - Else retrieve element at `front`. If only one element, set `front = rear = -1`. Else increment `front` modulo capacity.  
- **Size:** Can be computed as `(rear - front + capacity) % capacity + 1` or kept as a separate counter.
- **Pros:** Cache-friendly, simple.  
- **Cons:** Fixed size may lead to overflow; dynamic resizing is costly (O(n) copy). Wasted space if not circular.

### Using a Singly Linked List
- **Idea:** Maintain pointers to `head` (front) and `tail` (rear). Each node contains data and a `next` pointer.
- **Enqueue:** Create new node; if queue empty, set head = tail = new node; else set tail.next = new node, then tail = new node.
- **Dequeue:** If empty, underflow. Else store head data; move head to head.next; if head becomes null, set tail = null.
- **Size:** Maintain a counter or traverse (O(n) if not maintained).
- **Pros:** Dynamic size, no wasted space, no shifting.  
- **Cons:** Extra memory for pointers, not cache-friendly.

---

## Types of Queues

| Type | Description | Typical Use |
|------|-------------|-------------|
| **Simple Queue** | Basic FIFO queue; insertion at rear, deletion at front. | Job scheduling, printer spooling. |
| **Circular Queue** | Array-based where the last position connects back to the first, reusing empty slots. | Fixed-size buffers, audio/video streaming. |
| **Priority Queue** | Elements have priorities; the element with highest priority is dequeued first (not necessarily FIFO). | Dijkstra's algorithm, task scheduling with priorities. |
| **Double-Ended Queue (Deque)** | Insertion and deletion allowed at both ends. | Sliding window algorithms, undo-redo. |
| **Blocking Queue** | Thread-safe queue that blocks when attempting to dequeue from an empty queue or enqueue to a full queue. | Producer-consumer problems. |

---

## ASCII Representation of a Queue

A queue can be visualized as a horizontal line with front on the left and rear on the right.

```
Front → [10] [20] [30] [40] ← Rear
```

- Elements: 10, 20, 30, 40  
- Front: 10 (next to be removed)  
- Rear: 40 (last added)

Enqueue 50:
```
Front → [10] [20] [30] [40] [50] ← Rear
```

Dequeue (removes 10):
```
Front → [20] [30] [40] [50] ← Rear
```

For a **circular queue** with array of size 5, indices might wrap:

```
Indices: 0   1   2   3   4
Values: [30] [40] [ ] [10] [20]
         ↑             ↑
        rear          front
```
Here front is at index 3 (value 10), rear at index 0 (value 30). The queue contains 10,20,30,40 in order.