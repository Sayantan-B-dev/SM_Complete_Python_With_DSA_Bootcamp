```py
class Node:
    def __init__(self,data):
        self.data=data
        self.next=None

class StackUsingLinkedList:
    def __init__(self):
        self.head = None
        self.size = 0

    def push(self,data):
        newNode = Node(data)
        self.size+=1 # Very Important to maintain Size
        if(self.head==None):
            self.head=newNode
            return f"Added {data} to the stack"
        
        newNode.next = self.head
        self.head = newNode
        return f"Added {data} to the stack"

    def top(self):
        if(self.head is None or self.size == 0):
            return "Stack is Empty, no Top element"

        return self.head.data
    
    def pop(self):
        if(self.head is None or self.size == 0):
            return "Stack is Empty, Cannot Pop element"
    
        dataAtTop = self.head.data
        self.head = self.head.next
        self.size -=1
        return dataAtTop
    
    def size(self):
        return self.size
    
    def is_empty(self):
        return self.size ==0

myStack = StackUsingLinkedList()

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
# Stack Implementation Using Linked List (Head as Top)

This stack implementation uses a **singly linked list** where the **head node represents the top of the stack**.
Each new element is inserted at the beginning of the list. Removing elements also occurs at the beginning.

This design ensures that the major stack operations operate in **constant time**.

---

# Node Structure

## Concept

A stack using a linked list requires a **node structure**. Each node stores a value and a reference to the next node.

```python
class Node:
    def __init__(self,data):
        self.data=data
        self.next=None
```

## Node Components

| Field  | Description                                    |
| ------ | ---------------------------------------------- |
| `data` | Stores the element inserted into the stack     |
| `next` | Pointer referencing the next node in the stack |

---

## ASCII Representation of a Node

```
+-----------+-----------+
|   data    |   next    |
+-----------+-----------+
```

Example node:

```
+-----------+-----------+
|    30     |  address  |
+-----------+-----------+
```

---

# Stack Structure

The stack maintains two important properties.

| Variable | Purpose                                        |
| -------- | ---------------------------------------------- |
| `head`   | Points to the top element of the stack         |
| `size`   | Tracks the number of elements currently stored |

---

## Stack Visualization

After pushing several elements:

```
Top (Head)
   │
   ▼
+------+   +------+   +------+   +------+
| 40   | → | 30   | → | 20   | → | 10   |
+------+   +------+   +------+   +------+
```

---

# Constructor

```python
def __init__(self):
    self.head = None
    self.size = 0
```

## Purpose

Initializes an empty stack.

---

## Algorithm

1. Initialize the head pointer as `None`.
2. Initialize the stack size as `0`.
3. No nodes exist initially.

---

## Resulting Structure

```
Head → None
Size = 0
```

---

# Push Operation

```python
def push(self,data):
    newNode = Node(data)
    self.size+=1
    if(self.head==None):
        self.head=newNode
        return f"Added {data} to the stack"
        
    newNode.next = self.head
    self.head = newNode
    return f"Added {data} to the stack"
```

## Purpose

Adds a new element at the **top of the stack**.

---

## Algorithm

1. Create a new node containing the value.
2. Increase the stack size by one.
3. Check whether the stack is empty.
4. If the stack is empty, assign the head pointer to the new node.
5. Otherwise connect the new node to the current head.
6. Move the head pointer to the new node.
7. Return a confirmation message.

---

## Step-by-Step Execution

### Initial Stack

```
Head → None
```

### push(10)

```
Head
 │
 ▼
+------+
| 10   |
+------+
```

---

### push(20)

```
Head
 │
 ▼
+------+   +------+
| 20   | → | 10   |
+------+   +------+
```

---

### push(30)

```
Head
 │
 ▼
+------+   +------+   +------+
| 30   | → | 20   | → | 10   |
+------+   +------+   +------+
```

---

### push(40)

```
Head
 │
 ▼
+------+   +------+   +------+   +------+
| 40   | → | 30   | → | 20   | → | 10   |
+------+   +------+   +------+   +------+
```

---

## Complexity

| Operation | Complexity |
| --------- | ---------- |
| push      | O(1)       |

The operation requires only pointer reassignment.

---

# Top Operation

```python
def top(self):
    if(self.head is None or self.size == 0):
        return "Stack is Empty, no Top element"

    return self.head.data
```

## Purpose

Retrieves the element currently at the **top of the stack** without removing it.

---

## Algorithm

1. Check if the stack is empty.
2. If empty, return an error message.
3. Otherwise access the value stored in the head node.
4. Return the head node data.

---

## Visualization

```
Head
 │
 ▼
+------+   +------+   +------+
| 40   | → | 30   | → | 20   |
+------+   +------+   +------+
```

Top element returned: **40**

---

## Complexity

| Operation | Complexity |
| --------- | ---------- |
| top       | O(1)       |

Direct access to the head pointer.

---

# Pop Operation

```python
def pop(self):
    if(self.head is None or self.size == 0):
        return "Stack is Empty, Cannot Pop element"
    
    dataAtTop = self.head.data
    self.head = self.head.next
    self.size -=1
    return dataAtTop
```

## Purpose

Removes and returns the element at the top of the stack.

---

## Algorithm

1. Check if the stack is empty.
2. If empty, return an error message.
3. Store the data of the head node.
4. Move the head pointer to the next node.
5. Decrease the stack size by one.
6. Return the stored data.

---

## Step-by-Step Example

### Before Pop

```
Head
 │
 ▼
+------+   +------+   +------+   +------+
| 40   | → | 30   | → | 20   | → | 10   |
+------+   +------+   +------+   +------+
```

---

### Pop Operation

Head moves forward.

```
Head
 │
 ▼
+------+   +------+   +------+
| 30   | → | 20   | → | 10   |
+------+   +------+   +------+
```

Removed element: **40**

---

## Complexity

| Operation | Complexity |
| --------- | ---------- |
| pop       | O(1)       |

Only a pointer movement occurs.

---

# Size Operation

```python
def size(self):
    return self.size
```

## Purpose

Returns the number of elements stored in the stack.

---

## Algorithm

1. Access the stored size variable.
2. Return its value.

---

## Why This Is Efficient

The size variable is updated during **push and pop operations**, eliminating the need to traverse the list.

---

## Complexity

| Operation | Complexity |
| --------- | ---------- |
| size      | O(1)       |

---

# Is Empty Operation

```python
def is_empty(self):
    return self.size ==0
```

## Purpose

Checks whether the stack contains any elements.

---

## Algorithm

1. Compare the size variable with zero.
2. If size equals zero, return `True`.
3. Otherwise return `False`.

---

## Visualization

### Empty Stack

```
Head → None
Size = 0
```

### Non-Empty Stack

```
Head
 │
 ▼
+------+
| 10   |
+------+
Size = 1
```

---

## Complexity

| Operation | Complexity |
| --------- | ---------- |
| is_empty  | O(1)       |

Only a simple comparison operation is required.

---

# Final Stack State After Program Execution

Operations executed:

```
push(10)
push(20)
push(30)
push(40)
pop()
pop()
```

Final structure:

```
Head
 │
 ▼
+------+   +------+
| 20   | → | 10   |
+------+   +------+
```

Size:

```
2
```

Top element:

```
20
```

---

# Operation Complexity Summary

| Operation | Time Complexity |
| --------- | --------------- |
| push      | O(1)            |
| pop       | O(1)            |
| top       | O(1)            |
| is_empty  | O(1)            |
| size      | O(1)            |

This design achieves **optimal stack performance using a singly linked list** because every operation occurs at the head of the structure.
