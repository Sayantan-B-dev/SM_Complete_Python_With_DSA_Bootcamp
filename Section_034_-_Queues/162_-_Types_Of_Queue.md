## Types of Queues – Complete Overview

This document covers four fundamental queue types: Simple Queue, Circular Queue, Priority Queue, and Double-Ended Queue (Deque). Each section includes definition, real examples, operations with algorithmic steps, ASCII representation, advantages, and disadvantages.

---

## 1. Simple Queue (Linear Queue)

### Definition
A linear queue follows the First-In, First-Out (FIFO) principle. Elements are inserted at the rear and removed from the front. It is the most basic queue structure.

### Real Examples & Use Cases
- **Ticket counter line** – First person gets ticket first.
- **Printer spooler** – Print jobs processed in order received.
- **Call center systems** – Calls handled in arrival order.
- **Breadth-First Search (BFS)** in graphs/trees.

### Operations & Algorithmic Overview

**Node structure:** `[data|next]`

**Queue state example (3 elements):**
```
Front → [10|*]→[20|*]→[30|/] ← Rear
```

#### Enqueue (Add at rear)
```
1. Create new node with given data, next = NULL
2. If queue is empty (front == NULL):
     front = rear = new node
3. Else:
     rear.next = new node
     rear = new node
4. Increment size
```

#### Dequeue (Remove from front)
```
1. If queue is empty (front == NULL):
     return error (underflow)
2. Save data = front.data
3. front = front.next
4. If front becomes NULL:
     rear = NULL    // queue now empty
5. Decrement size
6. Return saved data
```

#### Front (Peek)
```
1. If queue empty: return error
2. Return front.data
```

#### Rear (Peek at last)
```
1. If queue empty: return error
2. Return rear.data
```

#### isEmpty
```
return front == NULL
```

#### Size
```
return size_counter   // maintained during enqueue/dequeue
```

### Advantages
- Simple to implement and understand.
- Fair scheduling (FIFO order).
- O(1) operations with linked list or circular array.

### Disadvantages
- Fixed-size array implementation wastes space (cannot reuse empty slots at front).
- Not suitable for priority-based processing.
- Linear array implementation requires shifting for dequeue (O(n)).

---

## 2. Circular Queue (Ring Buffer)

### Definition
A circular queue connects the last position back to the first, forming a circle. It reuses empty slots created by dequeue operations, preventing wasted space in array-based implementations. Uses modulo arithmetic to wrap indices.

### Real Examples & Use Cases
- **CPU scheduling** – Round-robin scheduler.
- **Audio/video streaming buffer** – Circular buffer stores incoming packets.
- **Keyboard buffer** – Stores keystrokes until processed.
- **Network packet buffers** in routers.

### Operations & Algorithmic Overview

**Array representation with front/rear pointers:**
```
Indices:  0     1     2     3     4
Values: [10]  [20]  [  ]  [40]  [30]
         ↑           ↑
        rear        front
```
(Queue contains: 30,40,10,20 – front at 3, rear at 0)

#### Enqueue (Add at rear)
```
1. If queue is full ((rear+1) % capacity == front):
     return error (overflow)
2. If empty (front == -1):
     front = rear = 0
3. Else:
     rear = (rear + 1) % capacity
4. array[rear] = data
5. Increment size
```

#### Dequeue (Remove from front)
```
1. If empty (front == -1):
     return error (underflow)
2. Save data = array[front]
3. If front == rear (only one element):
     front = rear = -1
4. Else:
     front = (front + 1) % capacity
5. Decrement size
6. Return saved data
```

#### Front
```
1. If empty: return error
2. Return array[front]
```

#### Rear
```
1. If empty: return error
2. Return array[rear]
```

#### isEmpty
```
return front == -1
```

#### isFull
```
return (rear + 1) % capacity == front
```

#### Size
```
if rear >= front:
    size = rear - front + 1
else:
    size = capacity - front + rear + 1
```

### Advantages
- Efficient memory usage – reuses empty slots.
- No shifting required – O(1) enqueue and dequeue.
- Ideal for fixed-size buffers.

### Disadvantages
- Fixed capacity – may overflow if not sized appropriately.
- Slightly complex implementation due to modulo arithmetic.
- One slot is often sacrificed to distinguish full vs empty.

---

## 3. Priority Queue

### Definition
A priority queue is a queue where each element has an associated priority. Elements with higher priority are dequeued before elements with lower priority, regardless of their arrival order. If two elements have same priority, FIFO order may be followed (depending on implementation).

### Real Examples & Use Cases
- **Emergency room triage** – Critical patients treated first.
- **Operating systems** – Process scheduling (highest priority process runs first).
- **Dijkstra's algorithm** – Uses priority queue to find shortest paths.
- **Event-driven simulations** – Events processed by time priority.
- **Bandwidth management** – Prioritized packet handling.

### Implementation Approaches
- Using binary heap (most common) – O(log n) enqueue/dequeue.
- Using unsorted list – O(1) enqueue, O(n) dequeue.
- Using sorted list – O(n) enqueue, O(1) dequeue.

### Operations & Algorithmic Overview (Heap-based)

**Heap structure (not linear like list):**
```
        [P3]      (P = priority, lower number = higher priority)
       /    \
    [P5]    [P8]
   /   \
[P9]  [P10]
```

#### Enqueue (Insert with priority)
```
1. Add new element at the end of heap (bottom-rightmost position)
2. Increment size
3. While element's priority is higher than its parent:
       Swap element with parent
4. (This is "heapify up" or "bubble up")
```

#### Dequeue (Remove highest priority element)
```
1. If empty: return error
2. Save root element data (highest priority)
3. Replace root with last element in heap
4. Decrement size
5. While new root's priority is lower than either child:
       Swap with highest priority child
6. (This is "heapify down" or "bubble down")
7. Return saved data
```

#### Peek (Front – highest priority)
```
1. If empty: return error
2. Return root element data
```

#### isEmpty
```
return size == 0
```

#### Size
```
return size_counter
```

### Advantages
- Prioritized processing – critical tasks handled first.
- Efficient O(log n) operations with heap.
- Flexible – can be implemented in multiple ways.

### Disadvantages
- More complex implementation than simple queue.
- Not strictly FIFO (may cause starvation for low-priority items).
- Heap implementation requires extra memory for tree structure.

---

## 4. Double-Ended Queue (Deque – pronounced "deck")

### Definition
A deque allows insertion and deletion at both ends (front and rear). It combines properties of stacks and queues. Can be used as both FIFO and LIFO.

### Types
- **Input-restricted deque** – Insertions allowed at one end only, deletions at both ends.
- **Output-restricted deque** – Deletions allowed at one end only, insertions at both ends.

### Real Examples & Use Cases
- **Undo/Redo operations** – Can add/remove from both ends.
- **Sliding window maximum** – Maintaining max in a sliding window.
- **Task scheduling** where tasks can be added to front or back.
- **Palindrome checker** – Compare characters from both ends.
- **Browser history** (back/forward with ability to add at both ends).

### Operations & Algorithmic Overview

**Deque representation (doubly linked list):**
```
NULL ← [10|*|*] ⇄ [20|*|*] ⇄ [30|/|*] → NULL
       front                    rear
```
(Each node: [data|prev|next])

#### Enqueue Front (Add at front)
```
1. Create new node with given data, prev = NULL, next = NULL
2. If empty (front == NULL):
     front = rear = new node
3. Else:
     new node.next = front
     front.prev = new node
     front = new node
4. Increment size
```

#### Enqueue Rear (Add at back)
```
1. Create new node
2. If empty:
     front = rear = new node
3. Else:
     rear.next = new node
     new node.prev = rear
     rear = new node
4. Increment size
```

#### Dequeue Front (Remove from front)
```
1. If empty: return error
2. Save data = front.data
3. front = front.next
4. If front becomes NULL (queue empty):
     rear = NULL
5. Else:
     front.prev = NULL
6. Decrement size
7. Return saved data
```

#### Dequeue Rear (Remove from back)
```
1. If empty: return error
2. Save data = rear.data
3. rear = rear.prev
4. If rear becomes NULL:
     front = NULL
5. Else:
     rear.next = NULL
6. Decrement size
7. Return saved data
```

#### Front (Peek front)
```
1. If empty: return error
2. Return front.data
```

#### Rear (Peek rear)
```
1. If empty: return error
2. Return rear.data
```

#### isEmpty
```
return front == NULL
```

#### Size
```
return size_counter
```

### Advantages
- Highly flexible – supports both stack and queue operations.
- All operations O(1) with doubly linked list.
- Useful for algorithms requiring access at both ends.

### Disadvantages
- More complex implementation (needs prev pointers).
- Slightly higher memory overhead per node (extra pointer).
- Not suitable if only simple FIFO is needed (overkill).

---

## Summary Comparison Table

| Feature | Simple Queue | Circular Queue | Priority Queue | Deque |
|---------|--------------|----------------|----------------|-------|
| **Ordering** | FIFO | FIFO | By priority | None (user-defined) |
| **Insertion** | Rear only | Rear only | Any (heap) | Front & Rear |
| **Deletion** | Front only | Front only | Highest priority | Front & Rear |
| **Peek** | Front/Rear | Front/Rear | Highest priority | Front/Rear |
| **Common Implementation** | Linked List, Array | Circular Array | Binary Heap | Doubly Linked List |
| **Time Complexity (all ops)** | O(1) | O(1) | O(log n) | O(1) |
| **Memory Efficiency** | Good | Excellent (reuses space) | Moderate | Moderate |
| **Use Case** | Basic queuing | Buffers, scheduling | Priority processing | Multi-purpose |