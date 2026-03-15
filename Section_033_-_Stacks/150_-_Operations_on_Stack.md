**Push** – Adds an element to the top of the stack.  
- If the stack is implemented with a dynamic array or linked list, push is typically O(1).  
- In a fixed-size array implementation, push may cause overflow if the stack is full.

**Pop** – Removes and returns the top element of the stack.  
- If the stack is empty, pop is undefined (underflow).  
- O(1) operation.

**Top / Peek** – Returns the top element without removing it.  
- Does not modify the stack.  
- O(1) operation.  
- If the stack is empty, peek may return a sentinel value or cause an error.

**Size** – Returns the number of elements currently in the stack.  
- Often implemented by maintaining a counter that is incremented/decremented on push/pop.  
- O(1) operation.

**Empty** – Checks whether the stack contains no elements.  
- Returns true if size == 0, false otherwise.  
- O(1) operation.