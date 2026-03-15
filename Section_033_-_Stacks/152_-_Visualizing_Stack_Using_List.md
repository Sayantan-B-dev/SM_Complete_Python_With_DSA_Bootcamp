# Stack Implementation Using Python List

## Conceptual Overview

The class `StackUsingList` implements a **stack data structure** using Python’s built-in `list` container. A stack follows the **Last In First Out (LIFO)** rule, meaning the most recently inserted element is always removed first.

Python lists are internally implemented as **dynamic arrays**, which makes them suitable for stack behavior because the `append()` and `pop()` operations operate efficiently at the **end of the list**.

---

# Class Structure Explanation

## Class Definition

```python
class StackUsingList:
```

This line declares a class that encapsulates all stack operations and internal storage logic.

---

# Constructor

```python
def __init__(self):
    self.__stack =[]
```

### Purpose

The constructor initializes an empty stack container.

### Important Design Detail

`__stack` uses **double underscore prefix**, making it a **private attribute**.

### Why Private?

Encapsulation prevents external modification of the internal structure.

Example of what is prevented:

```python
myStack.__stack.append(999)
```

Such direct access would break stack discipline and violate LIFO behavior.

Instead, all modifications must occur through controlled methods such as `push()` and `pop()`.

---

# Push Operation

```python
def push(self,data):
    self.__stack.append(data)
    print(f"Pushed {data} into Stack")
```

### Functionality

The `push` method inserts a new element at the **top of the stack**.

### Internal Mechanism

Python list operation used:

```python
append()
```

This adds the element at the **end of the list**, which represents the top of the stack.

### Time Complexity

| Operation | Complexity     |
| --------- | -------------- |
| push      | O(1) amortized |

Appending to a Python list is extremely efficient due to dynamic resizing.

---

# Size Operation

```python
def size(self):
    return len(self.__stack)
```

### Functionality

Returns the number of elements currently stored in the stack.

### Internal Mechanism

Python's built-in function `len()` calculates the number of elements stored in the list.

### Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| size      | O(1)       |

Python stores list length internally, so retrieving it requires constant time.

---

# Empty Check

```python
def is_empty(self):
    return len(self.__stack) == 0
```

### Functionality

Determines whether the stack currently contains any elements.

### Logic

If the list length equals zero, the stack has no elements.

Equivalent verbose version:

```python
if(len(self.__stack)==0):
    return True
else:
    return False
```

### Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| is_empty  | O(1)       |

---

# Top Operation

```python
def top(self):
    if(self.is_empty()):
        print("Stack is Empty, No top element")
        return None
    return self.__stack[-1]
```

### Functionality

Retrieves the element currently at the **top of the stack** without removing it.

### Internal Mechanism

Python supports **negative indexing**.

```python
list[-1]
```

This accesses the **last element in the list**, representing the stack top.

### Safety Check

Before accessing the element, the function checks whether the stack is empty to avoid an **index error**.

### Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| top       | O(1)       |

---

# Pop Operation

```python
def pop(self):
    if(self.is_empty()):
        print("Stack is Empty, Cannot pop")
        return None
    return self.__stack.pop()
```

### Functionality

Removes and returns the element at the top of the stack.

### Internal Mechanism

Python list method used:

```python
pop()
```

Without an index argument, `pop()` removes the **last element of the list**, which perfectly matches stack semantics.

### Safety Validation

The function first checks whether the stack is empty to prevent runtime errors.

### Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| pop       | O(1)       |

---

# Program Execution Walkthrough

## Stack Creation

```python
myStack = StackUsingList()
```

An instance of the stack class is created.

Internal state:

```
[]
```

---

## Check if Stack is Empty

```python
print(myStack.is_empty())
```

Stack contains zero elements.

Output:

```
True
```

---

## Push Operations

```python
print(myStack.push(10))
print(myStack.push(20))
print(myStack.push(30))
print(myStack.push(40))
```

Each call inserts a new element at the top.

Internal stack progression:

```
[10]
[10, 20]
[10, 20, 30]
[10, 20, 30, 40]
```

Each `push()` prints a message but returns **None**, therefore printing the return value results in:

```
Pushed 10 into Stack
None
Pushed 20 into Stack
None
Pushed 30 into Stack
None
Pushed 40 into Stack
None
```

---

## Check Empty Again

```python
print(myStack.is_empty())
```

Stack now contains elements.

Output:

```
False
```

---

## Pop Operations

```python
print(myStack.pop())
print(myStack.pop())
```

Stack before pop:

```
[10, 20, 30, 40]
```

First pop removes `40`.

Stack becomes:

```
[10, 20, 30]
```

Second pop removes `30`.

Stack becomes:

```
[10, 20]
```

Output:

```
40
30
```

---

## Size Check

```python
print(myStack.size())
```

Current stack contents:

```
[10, 20]
```

Output:

```
2
```

---

## Top Element

```python
print(myStack.top())
```

Top element is `20`.

Stack remains unchanged.

Output:

```
20
```

---

# Final Program Output

```
True
Pushed 10 into Stack
None
Pushed 20 into Stack
None
Pushed 30 into Stack
None
Pushed 40 into Stack
None
False
40
30
2
20
```
