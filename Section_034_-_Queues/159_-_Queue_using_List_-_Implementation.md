```py
class QueueUsingList:
    def __init__(self):
        self.__queue = []

    def size(self):
        return len(self.__queue)
    
    def isEmpty(self):
        '''if(self.size()==0):
            return True
        else:
            return False
        '''
        return self.size() == 0
    
    def enqueue(self,data):
        self.__queue.append(data)
        return f"We have added {data} to the Queue"
    
    def front(self):
        if(self.size()==0):
            print("Queue is Empty, cannot show Front")
            return None
        return self.__queue[0]
    
    def dequeue(self):
        if(self.size()==0):
            print("Queue is Empty, cannot dequeue")
            return None
        
        '''
        elementToBeDequeue=self.front()
        self.__queue.pop(0)
        return elementToBeDequeue
        '''

        return self.__queue.pop(0)
    
Q = QueueUsingList()

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

```

## QueueUsingList – Explanation and Documentation

This class implements a simple queue using Python's built-in `list` as the underlying storage. It follows the First-In, First-Out (FIFO) principle: elements are added at the rear and removed from the front.

### Class: `QueueUsingList`

#### `__init__(self)`
Initializes an empty queue by creating a private list `self.__queue`.  
- The double underscore makes the attribute name-mangled, discouraging direct access from outside the class (a form of encapsulation).

#### `size(self)`
Returns the number of elements currently in the queue.  
- Implementation: `len(self.__queue)`  
- Time complexity: O(1)

#### `isEmpty(self)`
Checks whether the queue has no elements.  
- Returns `True` if size is 0, else `False`.  
- Implementation: `return self.size() == 0`  
- Time complexity: O(1)

#### `enqueue(self, data)`
Adds a new element to the rear (end) of the queue.  
- Uses `self.__queue.append(data)` to insert at the end.  
- Returns a confirmation string: `f"We have added {data} to the Queue"`.  
- Time complexity: Amortized O(1) – Python lists are dynamic arrays; occasional resizing happens but is rare.

#### `front(self)`
Returns the element at the front of the queue without removing it.  
- If the queue is empty, prints an error message and returns `None`.  
- Otherwise returns `self.__queue[0]`.  
- Time complexity: O(1)

#### `dequeue(self)`
Removes and returns the element at the front of the queue.  
- If the queue is empty, prints an error and returns `None`.  
- Otherwise removes and returns the first element using `self.__queue.pop(0)`.  
- **Important:** `pop(0)` is O(n) because all remaining elements must shift left one position. This makes the operation inefficient for large queues.  
- Returns the dequeued element.

---

### Example Usage

```python
Q = QueueUsingList()

Q.enqueue(10)        # Returns: "We have added 10 to the Queue"
Q.enqueue(20)        # Returns: "We have added 20 to the Queue"
Q.enqueue(30)        # Returns: "We have added 30 to the Queue"

print(Q.size())      # 3
print(Q.isEmpty())   # False
print(Q.front())     # 10
print(Q.dequeue())   # 10
print(Q.dequeue())   # 20
print(Q.front())     # 30
print(Q.size())      # 1
```

---

### Performance Considerations

- **Enqueue:** Fast (amortized O(1)).  
- **Dequeue:** Slow (O(n)) due to shifting elements. For a true queue, we expect O(1) dequeue.  
- **Memory:** The list never shrinks automatically; after many dequeues, the underlying list may still hold references to popped items until garbage collection, but they are inaccessible.

---

### When to Use This Implementation

- For learning purposes or very small queues where performance is not critical.  
- When you need a quick FIFO structure and dequeue operations are rare.  
- **Not recommended** for production code with many dequeues.

---

### Better Alternatives in Python

- **`collections.deque`** – Provides O(1) `append()` (enqueue) and `popleft()` (dequeue).  
- **Custom circular buffer** – Useful for fixed-size queues with O(1) operations.

---

### Notes on Code Style

- The `enqueue` method returns a string; typically queue methods return nothing (or `None`) to avoid coupling with I/O. This is acceptable for demonstration but unusual in practice.  
- Error handling uses print statements and returns `None`; raising exceptions (e.g., `IndexError` or custom `EmptyQueueError`) would be more robust.  
- The class uses a private list to hide implementation details, but name mangling is not true security; it can still be accessed as `_QueueUsingList__queue`.