# Python Tuples

## 1. Definition and Concept Overview
A tuple is an ordered, immutable collection of objects. Tuples are sequence types, similar to lists, but cannot be modified after creation (immutable). They can contain elements of heterogeneous types. Tuples are defined by parentheses `()` and commas, or the `tuple()` constructor. They are hashable if all elements are hashable, allowing them to be used as dictionary keys or set elements.

## 2. Core Principles and Internal Mechanics
- **Immutable sequence**: Once created, elements cannot be added, removed, or replaced. The tuple object’s memory layout is fixed.
- **Storage**: Implemented as a contiguous array of `PyObject*` pointers. No over-allocation; size is exactly the number of elements.
- **Hashability**: A tuple is hashable only if all its elements are hashable. Hash is computed by combining element hashes (order-dependent).
- **Packing and unpacking**: Tuple packing (implicit tuple creation) and unpacking (iterable unpacking) are syntactic conveniences with no runtime overhead beyond the operations themselves.
- **Cache efficiency**: Small tuples (especially empty tuple) are singletons in CPython (e.g., `()` is a single object reused). Some small tuples may be interned.

## 3. Step-by-Step Logical Breakdown
- **Creation**: Allocate memory for the tuple and the pointer array. Copy references from the source iterable.
- **Indexing**: Direct pointer lookup using base address + offset. O(1).
- **Concatenation**: Create a new tuple with size len(a) + len(b); copy pointers from both into new storage.
- **Repetition**: Multiply tuple by integer n: allocate new tuple of size len(original)*n, copy pointers repeatedly.
- **Slicing**: Create new tuple copying pointers for the slice range.
- **Hash computation**: If tuple is hashable (all elements hashable), compute hash by combining element hashes; result is cached on the tuple for subsequent calls.

## 4. Implementation Examples

### 4.1 Creating Tuples
```python
# Empty tuple – singleton in CPython
empty = ()

# Tuple with one element – note trailing comma
single = (42,)                 # Without comma, (42) is just int 42

# Multiple elements, parentheses optional but recommended
colors = ("red", "green", "blue")

# Using tuple() constructor from any iterable
from_list = tuple([1, 2, 3])        # (1, 2, 3)
from_string = tuple("hello")         # ('h', 'e', 'l', 'l', 'o')

# Without parentheses – comma-separated values (tuple packing)
packed = 1, 2, 3                     # (1, 2, 3)
```

### 4.2 Accessing Tuple Elements
```python
point = (10, 20, 30)

# Indexing – 0-based
x = point[0]                         # 10
z = point[-1]                         # 30

# IndexError if out of range
# point[5]                            # IndexError

# Slicing returns new tuple
middle = point[1:3]                   # (20, 30)
reversed_slice = point[::-1]           # (30, 20, 10)
```

### 4.3 Tuple Operations: Concatenation, Repetition, Slicing
```python
a = (1, 2)
b = (3, 4)

# Concatenation (+)
c = a + b                              # (1, 2, 3, 4)

# Repetition (*)
repeated = a * 3                        # (1, 2, 1, 2, 1, 2)

# Membership test
exists = 2 in a                         # True
not_exists = 5 not in a                  # True

# Length
length = len(a)                          # 2
```

### 4.4 Immutable Nature of Tuples – Changing Requires Conversion
```python
# Tuples cannot be modified in-place
t = (1, 2, 3)
# t[0] = 99                             # TypeError: 'tuple' object does not support item assignment

# Workaround: convert to list, modify, convert back
temp = list(t)
temp[0] = 99
t = tuple(temp)                          # (99, 2, 3)

# However, if tuple contains mutable objects, those objects can be mutated
t_mut = ([1, 2], 3)
t_mut[0].append(99)                       # t_mut becomes ([1, 2, 99], 3) – tuple itself unchanged, but content changed
```

### 4.5 Tuple Methods: count, index
```python
data = (1, 2, 3, 2, 4, 2)

# count() – number of occurrences
twos = data.count(2)                      # 3

# index() – first index of value
first_two = data.index(2)                  # 1

# index with start and end bounds
next_two = data.index(2, 2)                # 3 (search starting at index 2)
last_two = data.index(2, 4)                # ValueError if not found
```

### 4.6 Packing and Unpacking Tuples
```python
# Packing – implicit tuple creation
packed = 10, 20, 30                        # (10, 20, 30)

# Unpacking – assign tuple elements to variables
x, y, z = packed                           # x=10, y=20, z=30

# Unpacking with *_ to collect remaining items
first, *rest = (1, 2, 3, 4)                # first=1, rest=[2, 3, 4]
*beginning, last = (1, 2, 3, 4)            # beginning=[1, 2, 3], last=4

# Unpacking in function calls
def func(a, b, c):
    return a + b + c

args = (10, 20, 30)
result = func(*args)                        # 60 – unpacks tuple as arguments
```

### 4.7 Unpacking with `*` in Tuple Contexts
```python
# Extended unpacking in tuple assignments
a, *b, c = (1, 2, 3, 4, 5)                 # a=1, b=[2,3,4], c=5

# Using * to merge tuples
t1 = (1, 2)
t2 = (3, 4)
merged = (*t1, *t2)                         # (1, 2, 3, 4) – unpacking inside tuple literal

# In list literals
lst = [*t1, 99, *t2]                         # [1, 2, 99, 3, 4]
```

### 4.8 Nested Tuples – Creation and Access
```python
# Tuple containing other tuples
nested = ((1, 2), (3, 4), (5, 6))

# Access element
value = nested[1][0]                         # 3

# Create nested tuple from existing structures
matrix = tuple(tuple(range(i, i+3)) for i in range(0, 9, 3))
# ((0, 1, 2), (3, 4, 5), (6, 7, 8))

# Mixed nesting
mixed = (1, (2, 3), (4, (5, 6)))
```

### 4.9 Iterating Over Nested Tuples
```python
nested = ((1, 2), (3, 4), (5, 6))

# Simple iteration
for inner in nested:
    for elem in inner:
        print(elem, end=" ")                 # 1 2 3 4 5 6

# Using enumerate for indices
for i, inner in enumerate(nested):
    for j, val in enumerate(inner):
        print(f"({i},{j}) = {val}")

# Flatten nested tuple using comprehension
flat = [elem for inner in nested for elem in inner]   # [1, 2, 3, 4, 5, 6]
```

### 4.10 Practical Example: Returning Multiple Values from a Function
```python
def min_max(data):
    """Return minimum and maximum of a sequence."""
    if not data:
        raise ValueError("empty sequence")
    return min(data), max(data)               # returns tuple

values = [5, 2, 8, 1, 9]
min_val, max_val = min_max(values)            # unpacking
print(f"Min: {min_val}, Max: {max_val}")      # Min: 1, Max: 9
```

### 4.11 Practical Example: Using Tuples as Dictionary Keys
```python
# Tuples are hashable, so can be used as keys in dict or elements in set
locations = {
    (40.7128, -74.0060): "New York",
    (34.0522, -118.2437): "Los Angeles",
    (41.8781, -87.6298): "Chicago"
}

coordinate = (40.7128, -74.0060)
city = locations.get(coordinate)               # "New York"

# Useful for composite keys
student_grades = {
    ("Alice", "Math"): 90,
    ("Alice", "Physics"): 85,
    ("Bob", "Math"): 88
}
```

### 4.12 Common Errors and Pitfalls
```python
# 1. Single-element tuple missing comma
not_a_tuple = (5)                     # int, not tuple
actual_tuple = (5,)                    # correct

# 2. Modifying tuple elements
t = (1, 2, 3)
# t[0] = 99                            # TypeError

# 3. Unpacking mismatch – number of variables doesn't match tuple length
# x, y = (1, 2, 3)                     # ValueError: too many values to unpack

# 4. Using mutable objects inside tuple breaks hashability if the tuple is used as key
# hash((1, [2, 3]))                     # TypeError: unhashable type: 'list'

# 5. Assuming tuple is completely immutable: the tuple's references are immutable, but referenced objects may be mutable
t_mut = ([1], 2)
t_mut[0].append(99)                    # allowed, changes the list inside tuple

# 6. Confusing tuple concatenation with repetition
# a = (1, 2) + 3                        # TypeError: can only concatenate tuple (not "int") to tuple

# 7. Using parentheses for grouping, not tuple creation
# (x * 2 for x in range(5)) is a generator expression, not a tuple. Use tuple(gen) or literal.
```

## 5. Time and Space Complexity Analysis
| Operation               | Time Complexity | Notes                                      |
|-------------------------|-----------------|--------------------------------------------|
| Indexing (`t[i]`)       | O(1)            | Direct pointer access                       |
| Length (`len(t)`)       | O(1)            | Stored in tuple header                      |
| Concatenation (`+`)     | O(n + m)        | Creates new tuple copying elements          |
| Repetition (`*`)        | O(n * k)        | Creates new tuple; k copies                 |
| Slicing `t[i:j]`        | O(k)            | k = slice length; copies references         |
| Membership (`in`)       | O(n)            | Linear scan; no hash lookup                 |
| Count / Index           | O(n)            | Linear scan                                 |
| Hash computation        | O(n)            | Cached after first call                      |

Space complexity: Tuple consumes memory for its header and an array of n pointers. No extra capacity overhead. The tuple object itself is fixed-size.

## 6. Edge Cases and Failure Scenarios
- **Empty tuple**: `()` – indexing raises `IndexError`; `len` returns 0; hashable (hash is constant).
- **Single-element tuple**: Must include trailing comma; otherwise it's not a tuple.
- **Tuple with mutable elements**: The tuple itself is immutable, but mutable elements can be changed; this can break invariants if tuple is used as dict key (the object's hash changes).
- **Large tuples**: Concatenation and repetition create new tuples; memory usage can spike.
- **Recursive tuples**: A tuple can contain itself (via mutable intermediary), but printing may cause recursion depth errors.
- **Unhashable elements**: If any element is unhashable, the tuple cannot be used as dict key or set element, but the tuple itself can exist.
- **Unpacking with `*` in assignment**: The starred target always receives a list, not a tuple; this is by design.

## 7. Practical Use Cases
- **Fixed collections of heterogeneous data**: e.g., coordinates (x, y), RGB values, function return values.
- **Dictionary keys**: When composite keys are needed (immutable, order matters).
- **Protecting data against accidental modification**: Passing tuple instead of list to indicate read-only intent.
- **Efficient storage for small immutable sequences**: Less overhead than list.
- **Argument packing/unpacking**: `*args` collects extra positional arguments as a tuple.
- **Named tuples (collections.namedtuple)**: Lightweight immutable objects with named fields.

## 8. Limitations and Trade-offs
- **Immutability**: Cannot change elements; if modification is needed frequently, use list.
- **No methods like `append`, `remove`** – must create new tuple.
- **Linear membership test**: No hash lookup; use set if frequent membership checks are needed.
- **Hashable only if all elements hashable** – cannot use as key if any element mutable.
- **Performance**: Creation of new tuples for operations like concatenation or repetition can be slower than in-place list operations.
- **Memory**: While tuples have no overhead capacity, the requirement to allocate new tuples for any "change" can lead to temporary allocations.