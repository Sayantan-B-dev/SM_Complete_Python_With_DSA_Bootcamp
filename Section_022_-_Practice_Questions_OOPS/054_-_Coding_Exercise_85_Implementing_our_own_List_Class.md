```python
#Quickcode
import ctypes

class CustomList:

    def __init__(self):
        initialCapacity = 1
        self.capacity = initialCapacity
        self.size = 0
        self.array = self.__create_array(self.capacity)

    def __create_array(self, capacity):
        return (capacity * ctypes.py_object)()

    def __resize(self, new_capacity):
        new_array = self.__create_array(new_capacity)
        for i in range(self.size):
            new_array[i] = self.array[i]
        self.array = new_array
        self.capacity = new_capacity

    def append(self, item):
        if self.size == self.capacity:
            self.__resize(self.capacity * 2)
        self.array[self.size] = item
        self.size += 1

    def __len__(self):
        return self.size

    def __str__(self):
        elements = []
        for i in range(self.size):
            elements.append(str(self.array[i]))
        return "[" + ", ".join(elements) + "]"

    def pop(self):
        if self.size == 0:
            return "Empty List, IndexError: pop from empty list"
        item = self.array[self.size - 1]
        self.size -= 1
        return item

    def __getitem__(self, index):
        if index < 0 or index >= self.size:
            return "Index Error: Invalid index"
        return self.array[index]

    def clear(self):
        self.size = 0

    def insert(self, position, element):
        if position < 0 or position > self.size:
            return "Index Error: Invalid position"
        if self.size == self.capacity:
            self.__resize(self.capacity * 2)
        for i in range(self.size, position, -1):
            self.array[i] = self.array[i - 1]
        self.array[position] = element
        self.size += 1

    def remove(self, element):
        index = -1
        for i in range(self.size):
            if self.array[i] == element:
                index = i
                break
        if index == -1:
            return "Element not found"
        for i in range(index, self.size - 1):
            self.array[i] = self.array[i + 1]
        self.size -= 1
```

# Custom Dynamic Array Implementation — `CustomList`

## Concept of Dynamic Arrays

A **dynamic array** is a data structure that behaves like a normal array but automatically increases its storage capacity when it becomes full. Traditional arrays have a fixed size determined during memory allocation. Dynamic arrays overcome this limitation by allocating a larger memory block and copying existing elements when needed.

Python’s built-in `list` type internally uses this concept. The goal of this implementation is to recreate similar behavior manually using the `ctypes` module.

The implementation manually manages memory allocation and resizing while providing operations commonly available in Python lists.

## Core Attributes of `CustomList`

| Attribute  | Description                                                     |
| ---------- | --------------------------------------------------------------- |
| `capacity` | Total number of elements the internal array can currently store |
| `size`     | Number of elements actually stored in the list                  |
| `array`    | Low-level memory storage holding object references              |

The list begins with:

```
capacity = 1
size = 0
```

As new elements are appended and the array becomes full, the capacity is doubled.

## Memory Allocation Using `ctypes`

The `ctypes` module allows creation of a low-level array that stores Python object references.

```
(capacity * ctypes.py_object)()
```

This allocates a block capable of storing Python objects.

Example internal structure:

```
capacity = 4

┌────────┬────────┬────────┬────────┐
│ None   │ None   │ None   │ None   │
└────────┴────────┴────────┴────────┘
```

## Complete Implementation

```python
import ctypes

class CustomList:

    def __init__(self):
        initialCapacity = 1
        self.capacity = initialCapacity
        self.size = 0
        self.array = self.__create_array(self.capacity)

    def __create_array(self, capacity):
        return (capacity * ctypes.py_object)()

    def __resize(self, new_capacity):
        new_array = self.__create_array(new_capacity)

        for i in range(self.size):
            new_array[i] = self.array[i]

        self.array = new_array
        self.capacity = new_capacity

    def append(self, item):
        if self.size == self.capacity:
            self.__resize(self.capacity * 2)

        self.array[self.size] = item
        self.size += 1

    def __len__(self):
        return self.size

    def __str__(self):
        elements = []

        for i in range(self.size):
            elements.append(str(self.array[i]))

        return "[" + ", ".join(elements) + "]"

    def pop(self):
        if self.size == 0:
            return "Empty List, IndexError: pop from empty list"

        item = self.array[self.size - 1]
        self.size -= 1
        return item

    def __getitem__(self, index):
        if index < 0 or index >= self.size:
            return "Index Error: Invalid index"

        return self.array[index]

    def clear(self):
        self.size = 0

    def insert(self, position, element):
        if position < 0 or position > self.size:
            return "Index Error: Invalid position"

        if self.size == self.capacity:
            self.__resize(self.capacity * 2)

        for i in range(self.size, position, -1):
            self.array[i] = self.array[i - 1]

        self.array[position] = element
        self.size += 1

    def remove(self, element):
        index = -1

        for i in range(self.size):
            if self.array[i] == element:
                index = i
                break

        if index == -1:
            return "Element not found"

        for i in range(index, self.size - 1):
            self.array[i] = self.array[i + 1]

        self.size -= 1
```

## Example Usage

```python
myList = CustomList()

myList.append(1)
myList.append(2)
myList.append(3)
myList.append(4)

print(myList)

myList.insert(1, 100)
print(myList)

print(len(myList))

print(myList.pop())

print(myList)

print(myList[2])

myList.remove(2)
print(myList)

myList.clear()
print(myList)
```

## Expected Output

```
[1, 2, 3, 4]
[1, 100, 2, 3, 4]
5
4
[1, 100, 2, 3]
2
[1, 100, 3]
[]
```

## Step-by-Step Internal Behavior

### Initial State

```
capacity = 1
size = 0
array = [None]
```

### After `append(1)`

```
capacity = 1
size = 1

[1]
```

### After `append(2)`

Capacity becomes full so resizing occurs.

```
new capacity = 2
size = 2

[1, 2]
```

### After `append(3)`

Resizing occurs again.

```
new capacity = 4
size = 3

[1, 2, 3, None]
```

### After `append(4)`

```
capacity = 4
size = 4

[1, 2, 3, 4]
```

### After `insert(1, 100)`

Elements shift right to make space.

```
[1, 100, 2, 3, 4]
```

## Algorithmic Complexity

| Operation | Complexity     |
| --------- | -------------- |
| append    | O(1) amortized |
| pop       | O(1)           |
| get item  | O(1)           |
| insert    | O(n)           |
| remove    | O(n)           |
| clear     | O(1)           |

## Why Dynamic Arrays Double Capacity

Doubling capacity reduces the frequency of expensive resize operations.

Example growth pattern:

```
1 → 2 → 4 → 8 → 16 → 32
```

This strategy ensures **amortized constant time append operations**.

## Comparison With Python Built-in List

| Feature           | CustomList          | Python List                |
| ----------------- | ------------------- | -------------------------- |
| Memory allocation | Manual using ctypes | Implemented in optimized C |
| Dynamic resizing  | Yes                 | Yes                        |
| Speed             | Slower              | Extremely optimized        |
| Educational value | High                | Not visible internally     |

## Internal Memory Visualization

Example after inserting five elements:

```
capacity = 8
size = 5

┌─────┬─────┬─────┬─────┬─────┬──────┬──────┬──────┐
│ 1   │100  │ 2   │ 3   │ 4   │None  │None  │None  │
└─────┴─────┴─────┴─────┴─────┴──────┴──────┴──────┘
```

Only the first `size` elements are valid list elements.

## Key Learning Concepts Demonstrated

This implementation demonstrates several fundamental computer science principles:

| Concept                        | Explanation                                                       |
| ------------------------------ | ----------------------------------------------------------------- |
| Dynamic memory management      | Expanding storage during runtime                                  |
| Amortized analysis             | Efficient average runtime despite occasional expensive operations |
| Low-level array operations     | Manual element shifting during insert and remove                  |
| Abstract data structure design | Creating a high-level list interface over low-level storage       |

Understanding this implementation reveals how most programming languages internally implement expandable lists such as:

* Python `list`
* Java `ArrayList`
* C++ `vector`
