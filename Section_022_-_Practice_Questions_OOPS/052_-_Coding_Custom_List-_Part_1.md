## CustomList: A Dynamic Array Implementation

The `CustomList` class is a manual implementation of a dynamic array, similar to Python's built-in list. It uses the `ctypes` library to create a low-level array of `py_object` elements, allowing it to store references to any Python object. The list automatically grows when more elements are added than its current capacity. Below is a detailed explanation of how each method works, including algorithms, pseudocode, and step‑by‑step logic.

---

### 1. Overview of the Dynamic Array Concept

A dynamic array is a data structure that provides the ability to store elements in contiguous memory locations while automatically resizing itself when it runs out of space. It maintains three key attributes:

- **Capacity** – The total number of slots currently available in the underlying raw array.
- **Size** – The number of elements actually stored (size ≤ capacity).
- **Array** – A pointer/reference to the raw array that holds the elements.

When the size reaches the capacity, a new, larger array is allocated, all existing elements are copied to it, and the old array is discarded. This operation is called **resizing**.

---

### 2. Initialization (`__init__`)

**Purpose:** Create a new empty list with an initial capacity of 1.

**Algorithm Steps:**

1. Set the initial capacity to 1.
2. Set the current size (number of elements) to 0.
3. Create the underlying raw array with the given capacity using a helper method `__create_array`.
4. Store the array reference in `self.array`.

**Pseudocode:**
```
INITIALIZE capacity = 1
SET size = 0
CALL create_array(capacity) AND STORE result IN array
```

**Flowchart (text description):**

```
[Start] → Set capacity = 1, size = 0 → Call create_array(capacity) → Assign returned array to self.array → [End]
```

**Detailed Steps:**

- **Step 1:** The `__init__` method is called automatically when a new `CustomList` object is created.
- **Step 2:** It defines a local variable `initialCapacity` with the value 1. This is the starting capacity.
- **Step 3:** The instance variables `capacity` and `size` are set to this initial capacity and 0 respectively.
- **Step 4:** The helper method `__create_array` is invoked with `self.capacity` as an argument. This method returns a raw array of length `capacity` that can hold references to Python objects.
- **Step 5:** The returned array is stored in `self.array`. The list is now ready to accept elements.

---

### 3. Helper Method: `__create_array`

**Purpose:** Allocate a raw array of a given capacity using `ctypes.py_object`. Each element of this array can hold a reference to any Python object.

**Algorithm Steps:**

1. Accept an integer `capacity` as input.
2. Use `ctypes.py_object` multiplied by `capacity` to create a new array of that length.
3. Return the newly created array.

**Pseudocode:**
```
FUNCTION create_array(capacity):
    RETURN new array of ctypes.py_object with length capacity
```

**Flowchart (text description):**

```
[Start] → Receive capacity → Create array = (capacity * ctypes.py_object)() → Return array → [End]
```

**Detailed Steps:**

- **Step 1:** The method takes one parameter, `capacity`, which is the desired number of slots.
- **Step 2:** It constructs a new array by multiplying `ctypes.py_object` by `capacity`. This is a C‑style array that can store Python object references.
- **Step 3:** The array is returned to the caller. At this point, each slot contains a null reference.

---

### 4. Resizing: `__resize`

**Purpose:** Increase (or decrease) the capacity of the underlying array. This method is called internally when the list runs out of space.

**Algorithm Steps:**

1. Accept a new capacity value `new_capacity`.
2. Create a new raw array of that size using `__create_array`.
3. Copy all existing elements from the old array to the new array.
   - Loop from index 0 to `self.size - 1`.
   - For each index `i`, assign `old_array[i]` to `new_array[i]`.
4. Replace the old array with the new array.
5. Update the instance variable `capacity` to `new_capacity`.

**Pseudocode:**
```
FUNCTION resize(new_capacity):
    new_array = create_array(new_capacity)
    FOR i FROM 0 TO size-1:
        new_array[i] = array[i]
    array = new_array
    capacity = new_capacity
```

**Flowchart (text description):**

```
[Start] → Receive new_capacity → Call create_array(new_capacity) → [Loop: i=0 to size-1] → Copy array[i] to new_array[i] → After loop, assign new_array to array → Update capacity → [End]
```

**Detailed Steps:**

- **Step 1:** The method receives `new_capacity`, which is typically double the current capacity (for growth) but could be any value.
- **Step 2:** It creates a new raw array of length `new_capacity` using `__create_array`.
- **Step 3:** A loop runs from `0` to `self.size - 1`. For each index, it copies the reference stored in the old array into the corresponding position of the new array. (Because the old array may have extra slots beyond `size`, those are ignored.)
- **Step 4:** After the copy, the instance variable `self.array` is set to point to the new array. The old array is no longer referenced and will be garbage‑collected.
- **Step 5:** Finally, `self.capacity` is updated to the new value.

---

### 5. Appending an Element (`append`)

**Purpose:** Add a new element to the end of the list.

**Algorithm Steps:**

1. Check if the current size equals the capacity.
   - If yes, call `__resize` with double the current capacity.
2. Insert the new element at the index equal to the current size.
3. Increment the size by 1.

**Pseudocode:**
```
FUNCTION append(item):
    IF size == capacity:
        resize(2 * capacity)
    array[size] = item
    size = size + 1
```

**Flowchart (text description):**

```
[Start] → [Check if size == capacity?] 
   |-- Yes → Call resize(2*capacity) → proceed
   |-- No → proceed
→ Place item at array[size] → Increment size → [End]
```

**Detailed Steps:**

- **Step 1:** The method receives the item to be appended.
- **Step 2:** It compares `self.size` with `self.capacity`. If they are equal, the underlying array is full, so a resize is triggered. The new capacity is set to twice the current capacity (a common growth factor to amortize the cost of resizing).
- **Step 3:** The new item is stored at the index `self.size` in the array. Because array indices start at 0, this is the first free slot.
- **Step 4:** `self.size` is incremented by 1 to reflect the addition.

---

### 6. Getting the Length (`__len__`)

**Purpose:** Return the number of elements currently stored in the list.

**Algorithm Steps:**

1. Return the value of `self.size`.

**Pseudocode:**
```
FUNCTION __len__():
    RETURN size
```

**Flowchart (text description):**

```
[Start] → Return size → [End]
```

**Detailed Steps:**

- This method is called when the built‑in `len()` function is used on a `CustomList` object. It simply returns the current size, which is always the count of elements stored.

---

### 7. String Representation (`__str__`)

**Purpose:** Create a human‑readable string representation of the list, e.g., `[1,2,3]`.

**Algorithm Steps:**

1. Initialize an empty string `output`.
2. Loop from index 0 to `self.size - 1`:
   - Convert the element at that index to a string.
   - Append that string and a comma to `output`.
3. After the loop, remove the trailing comma (if any elements exist).
4. Enclose the result in square brackets and return.

**Pseudocode:**
```
FUNCTION __str__():
    output = ""
    FOR i FROM 0 TO size-1:
        output = output + str(array[i]) + ","
    IF size > 0:
        REMOVE last comma from output
    RETURN "[" + output + "]"
```

**Flowchart (text description):**

```
[Start] → Set output = "" → [Loop i=0 to size-1] → Append str(array[i]) + "," to output → End loop → If size>0, remove last character (the extra comma) → Return "[" + output + "]" → [End]
```

**Detailed Steps:**

- **Step 1:** An empty string variable `output` is created.
- **Step 2:** A loop runs from 0 to `self.size - 1`. For each element, it converts the element to a string using `str()` and appends it followed by a comma to `output`. This produces something like `"1,2,3,"`.
- **Step 3:** After the loop, the trailing comma is removed by slicing off the last character (`output[:-1]` conceptually). This step is omitted if the list is empty.
- **Step 4:** The resulting string is placed inside square brackets and returned.

---

### 8. Removing the Last Element (`pop`)

**Purpose:** Remove and return the last element of the list. If the list is empty, return an error message.

**Algorithm Steps:**

1. Check if the size is 0.
   - If yes, return an error message (or raise an exception; here a string is returned).
2. Retrieve the element at index `size - 1` and store it.
3. Decrease the size by 1 (the element is effectively removed).
4. Return the stored element.

**Pseudocode:**
```
FUNCTION pop():
    IF size == 0:
        RETURN "Empty List , IndexError: pop from empty list"
    popped = array[size-1]
    size = size - 1
    RETURN popped
```

**Flowchart (text description):**

```
[Start] → [Check if size == 0?] 
   |-- Yes → Return error message
   |-- No → Save array[size-1] → Decrement size → Return saved item
```

**Detailed Steps:**

- **Step 1:** The method first checks whether the list is empty (`self.size == 0`). If it is, it returns a descriptive error string. (Note: In a production implementation, raising an exception would be more appropriate, but this code returns a string.)
- **Step 2:** If the list is not empty, it accesses the last element at index `self.size - 1` and stores it in a temporary variable.
- **Step 3:** It then reduces `self.size` by one. The underlying array still holds the reference at that index, but it is now beyond the logical end of the list and will be overwritten when a new element is added.
- **Step 4:** The stored element is returned to the caller.

---

### 9. Indexing (`__getitem__`)

**Purpose:** Allow access to an element by its index using square brackets (e.g., `mylist[i]`). Performs bounds checking.

**Algorithm Steps:**

1. Receive an integer `index`.
2. Check if the index is within the valid range: `0 ≤ index < size`.
   - If yes, return the element at that index from the array.
   - If no, return an error message (or raise an exception).

**Pseudocode:**
```
FUNCTION __getitem__(index):
    IF index >= 0 AND index < size:
        RETURN array[index]
    ELSE:
        RETURN "Index Error : Invalid index"
```

**Flowchart (text description):**

```
[Start] → Receive index → [Check if 0 ≤ index < size?] 
   |-- Yes → Return array[index]
   |-- No → Return error message
```

**Detailed Steps:**

- **Step 1:** The method is called with an index. It verifies whether the index is non‑negative and less than the current size.
- **Step 2:** If the index is valid, it retrieves the value stored at `self.array[index]` and returns it.
- **Step 3:** If the index is out of bounds, it returns an error string. (Again, raising an exception would be typical, but this implementation returns a string.)

---

### 10. Clearing the List (`clear`)

**Purpose:** Remove all elements from the list, resetting it to an empty state. The underlying array remains the same, but the logical size becomes zero.

**Algorithm Steps:**

1. Set `self.size` to 0.

**Pseudocode:**
```
FUNCTION clear():
    size = 0
```

**Flowchart (text description):**

```
[Start] → Set size = 0 → [End]
```

**Detailed Steps:**

- **Step 1:** This method simply sets the `size` attribute to 0. The underlying array still holds references to the old elements, but they are now considered inaccessible. The next append will overwrite them. (This is a fast operation because no actual deletion occurs.)

---

### 11. Inserting an Element at a Specific Position (`insert`)

**Purpose:** Insert a new element at a given index, shifting subsequent elements to the right.

**Algorithm Steps:**

1. Check if the list is full (`size == capacity`). If yes, resize to double the capacity.
2. Shift elements from the insertion position to the end one step to the right:
   - Loop from `size` down to `position+1` (or `position` depending on implementation).
   - Move each element one index forward.
3. Place the new element at the specified position.
4. Increment size by 1.

**Pseudocode:**
```
FUNCTION insert(position, element):
    IF size == capacity:
        resize(2 * capacity)
    FOR index FROM size DOWN TO position+1:
        array[index] = array[index-1]
    array[position] = element
    size = size + 1
```

**Flowchart (text description):**

```
[Start] → [Check if size == capacity?] 
   |-- Yes → resize(2*capacity) → proceed
   |-- No → proceed
→ [Loop index = size down to position+1] → array[index] = array[index-1] → End loop
→ Place element at array[position] → Increment size → [End]
```

**Detailed Steps:**

- **Step 1:** The method receives the desired `position` (an integer) and the `element` to insert. It assumes the position is valid (the original code does not check bounds, so an invalid position would cause an error or unexpected behavior).
- **Step 2:** It first checks whether the array is full. If `self.size` equals `self.capacity`, it calls `__resize` with double the capacity to make room.
- **Step 3:** A loop runs from the current `size` (which is the first free slot after the last element) down to `position + 1`. In each iteration, it copies the element from index `index-1` to index `index`, effectively shifting everything right by one.
- **Step 4:** After the shift, the slot at `position` is empty, and the new element is placed there.
- **Step 5:** Finally, `self.size` is incremented by one to include the new element.

---

### 12. Removing an Element by Value (`remove` – placeholder)

The `remove` method in the provided code is only a placeholder (the body contains `pass`). Therefore, no functionality is implemented. In a typical implementation, it would search for the first occurrence of the given element and remove it by shifting subsequent elements left.

---

### Summary of Internal Workings

- The underlying array is created with `ctypes.py_object`, which provides a low‑level array of Python object references.
- The list distinguishes between **capacity** (total slots) and **size** (used slots).
- When appending, if the list is full, it resizes to double the capacity, copying all elements to a new array. This amortizes the cost of resizing over many appends.
- Methods like `pop`, `__getitem__`, and `__str__` rely on the logical size to access only the valid portion of the array.
- `clear` simply resets the size to zero, making the list appear empty without deallocating the array.

This design mimics the behaviour of Python’s built‑in list, which is also a dynamic array, though Python’s actual implementation is more optimized and uses a different growth pattern. The `CustomList` class serves as an educational example of how dynamic arrays work under the hood.