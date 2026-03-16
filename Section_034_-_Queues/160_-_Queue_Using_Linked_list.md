### Queue Using Singly Linked List – Algorithmic Overview

A queue implemented with a singly linked list uses two pointers: `head` (front) and `tail` (rear). Nodes contain data and a `next` pointer. All operations should be O(1) with proper maintenance.

---

#### Node Structure
- `data`: the stored value.
- `next`: reference to the next node (or `null` if last).

---

#### Queue Structure
- `head`: points to the first node (front of queue).
- `tail`: points to the last node (rear of queue).
- `size`: integer counter tracking number of elements (optional but recommended for O(1) size query).

---

### Operations (Corrected Plan)

#### 1. **Enqueue** – Add element at rear
- Create a new node with the given data, `next = null`.
- **If queue is empty** (`head == null`):
  - Set `head = tail = new node`.
- **Else**:
  - Set `tail.next = new node` (link current tail to new node).
  - Update `tail = new node`.
- Increment `size` by 1.

**Time Complexity:** O(1)  
**Edge Cases:**
- Adding to an empty queue correctly sets both head and tail.
- Adding to a non‑empty queue only touches tail; head unchanged.

---

#### 2. **Dequeue** – Remove element from front
- **If queue is empty** (`head == null`):
  - Underflow: return error or `null` (depending on design).
- Save the data from `head` node to return later.
- Set `head = head.next` (move head to next node).
- **If head becomes `null`** (queue now empty):
  - Set `tail = null` as well (important to keep tail consistent).
- (Optional) In languages without garbage collection, free the removed node.
- Decrement `size` by 1.
- Return saved data.

**Time Complexity:** O(1)  
**Edge Cases:**
- Dequeue from a single‑element queue: after `head = head.next`, head becomes null; we must also set tail to null.
- Dequeue from empty queue must be detected and handled gracefully.

---

#### 3. **Front** – Peek at front element
- If queue empty, return error / `null`.
- Else return `head.data`.

**Time Complexity:** O(1)

---

#### 4. **Rear** – Peek at rear element
- If queue empty, return error / `null`.
- Else return `tail.data`.

**Time Complexity:** O(1)

---

#### 5. **isEmpty**
- Return `head == null` (or `size == 0`).

**Time Complexity:** O(1)

---

#### 6. **Size**
- If a `size` variable is maintained, return its value.  
- Without it, you would need to traverse the list (O(n)), which is inefficient for a queue.

**Time Complexity with counter:** O(1)

---

### Complexity Summary (with counter)

| Operation | Time Complexity | Notes |
|-----------|-----------------|-------|
| Enqueue   | O(1)            | Only tail pointer updated. |
| Dequeue   | O(1)            | Only head pointer updated; special case when last element removed. |
| Front     | O(1)            | Direct access via head. |
| Rear      | O(1)            | Direct access via tail. |
| isEmpty   | O(1)            | Check head pointer. |
| Size      | O(1)            | Using a counter. |

---

### Memory Management Considerations

- **Languages with garbage collection (Java, Python, C#, JavaScript):**  
  No explicit deallocation needed. When a node is dequeued and no references remain, it becomes eligible for garbage collection. However, be careful not to keep unintended references (e.g., if you store the removed node elsewhere). The algorithm above drops the reference by moving head, which is sufficient.

- **Languages without garbage collection (C, C++):**  
  You **must** explicitly free the memory of the dequeued node. After saving its data and before reassigning head, call `free(removedNode)` (or `delete` in C++). Failure to do so causes a memory leak.

- **Tail pointer after last dequeue:**  
  Setting `tail = null` when head becomes null ensures tail does not point to a freed node (in manual memory management) or to a node that is no longer logically part of the queue.

---

### Correcting the Original Plan

The user wrote:  
> enqueue: add in head by maintaiain a tail at the end if empty tail=head=newnode tail.next=newnode tail=newnode

This phrasing is ambiguous: "add in head" suggests adding at the head, but the subsequent steps correctly describe adding at the tail. **Enqueue must add at the tail** to maintain FIFO order. Adding at the head would make it a LIFO structure (stack). So the corrected wording is:  
- **Enqueue (add at rear):** Create new node. If empty, set head = tail = new node. Else, set tail.next = new node, then tail = new node.

The dequeue steps are correct:  
> dequeue: save head.data and return later, head=head.next, if head=none then tail=none too

One subtle point: after setting head = head.next, if the new head is null, we set tail = null. This is correct. However, if you maintain a size counter, you should decrement it before returning (or after, but consistently).

---

### Edge Cases Checklist

- [x] Enqueue into empty queue – head and tail both point to new node.
- [x] Enqueue into non‑empty queue – tail updated correctly.
- [x] Dequeue from empty queue – handled (return error/null).
- [x] Dequeue from queue with one element – head becomes null, tail set to null.
- [x] Dequeue from queue with multiple elements – head moves, tail unchanged.
- [x] Front/Rear on empty queue – handled.
- [x] Size maintained correctly on every enqueue/dequeue.
- [x] No memory leaks (if applicable) – dequeued node freed in non‑GC languages.

---

This linked list implementation gives true O(1) queue operations and is suitable for both learning and production use in languages with proper memory management.