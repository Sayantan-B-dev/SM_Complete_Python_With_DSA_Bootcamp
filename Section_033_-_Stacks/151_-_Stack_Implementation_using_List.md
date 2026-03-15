```py
'''
Stack Implementation : Using In-Built Python List

'''

class StackUsingList:
    def __init__(self):
        self.__stack =[] # Very Important to make it private so that functionality is right
    
    def push(self,data):
        self.__stack.append(data)
        print(f"Pushed {data} into Stack")

    def size(self):
        return len(self.__stack)
    
    def is_empty(self):
        '''
        if(len(self.stack)==0):
            return True
        else:
            return False
        '''
        return len(self.__stack) == 0
    
    def top(self):
        if(self.is_empty()):
            print("Stack is Empty, No top element")
            return None
        # return self.stack[len(self.stack)-1]
        return self.__stack[-1]
    
    def pop(self):
        if(self.is_empty()):
            print("Stack is Empty, Cannot pop")
            return None
        return self.__stack.pop()
    

myStack = StackUsingList()

print(myStack.is_empty())
print(myStack.push(10))
print(myStack.push(20))
print(myStack.push(30))
print(myStack.push(40))
print(myStack.is_empty())
print(myStack.pop())
print(myStack.pop())
print(myStack.size())
print(myStack.top())

```


### Stack Implementation Using Python List

The provided code implements a stack ADT using Python's built-in list as the underlying storage. The list provides dynamic array semantics, allowing efficient appends and pops from the end.

#### Class: `StackUsingList`

**Private Attribute:**  
`self.__stack` – A list that holds the stack elements. The double underscore makes it name-mangled, discouraging direct access from outside the class (a form of encapsulation).

**Methods:**

1. **`__init__(self)`**  
   Initializes an empty stack by setting `self.__stack = []`.

2. **`push(self, data)`**  
   Appends `data` to the end of the list.  
   - Time complexity: Amortized O(1) (list append is usually O(1) but may occasionally resize).  
   - Prints a confirmation message.

3. **`size(self)`**  
   Returns `len(self.__stack)`. O(1).

4. **`is_empty(self)`**  
   Returns `True` if the stack has no elements, otherwise `False`. Implemented as `return len(self.__stack) == 0`. O(1).

5. **`top(self)`**  
   Returns the last element of the list (`self.__stack[-1]`) without removing it.  
   - Checks for emptiness first; if empty, prints an error and returns `None`.  
   - Time complexity: O(1).

6. **`pop(self)`**  
   Removes and returns the last element using `self.__stack.pop()`.  
   - Checks for emptiness; if empty, prints an error and returns `None`.  
   - Time complexity: O(1) (list pop from end is O(1)).

#### Example Usage (as shown)

```python
myStack = StackUsingList()
print(myStack.is_empty())        # True
print(myStack.push(10))           # Pushed 10 into Stack, then prints None (push returns nothing)
print(myStack.push(20))           # Pushed 20...
print(myStack.push(30))
print(myStack.push(40))
print(myStack.is_empty())         # False
print(myStack.pop())               # 40
print(myStack.pop())               # 30
print(myStack.size())              # 2
print(myStack.top())               # 20
```

**Note:** The `print()` around `push` calls will output `None` because `push` has no return statement (it implicitly returns `None`). This is harmless but may clutter output. Typically `push` does not need a print inside; the example usage could simply call `myStack.push(10)` without wrapping it in `print()`.

#### Evaluation

- **Correctness:** Implements all core stack operations correctly.
- **Encapsulation:** The stack list is private, preventing external code from manipulating the stack directly (e.g., inserting in the middle). However, name mangling is not true privacy; it can still be accessed as `_StackUsingList__stack`.
- **Error Handling:** `pop` and `top` handle underflow gracefully by returning `None` and printing a message. This avoids raising exceptions, which may be desirable in some contexts but hides errors. A production stack might raise an exception (e.g., `IndexError` or custom `EmptyStackError`).
- **Print Statements:** The `push` method prints a message. This couples the stack with I/O, reducing reusability. In practice, such logging should be optional or handled externally.
- **Performance:** O(1) for all operations, which is optimal for a stack.

#### Alternative Implementations

- **Singly Linked List:** Eliminates occasional resizing overhead; each node stores data and a pointer to the next node. Push/pop at head are O(1). Uses more memory per element due to pointers.
- **Fixed-size Array:** Useful when maximum size is known; avoids dynamic resizing but may overflow. Requires tracking a top index.
- **Collections.deque:** Python's `collections.deque` provides O(1) append/pop from both ends and can be used as a stack (using `append` and `pop`). It is implemented with a doubly-linked list of blocks.

The list-based stack is the simplest and most common in Python due to the language's dynamic nature and the efficiency of list operations.