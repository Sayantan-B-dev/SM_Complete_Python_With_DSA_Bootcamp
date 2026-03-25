## Table of Contents
1. Full Commented Code  
2. Overview of Hashmap and Open Addressing  
3. Lower Layer: Core Components  
   - 3.1 Constructor (`__init__`)  
   - 3.2 Hash Function (`hash_function`)  
   - 3.3 Rehash Function (`rehash`) – Linear Probing  
4. Insertion Algorithm (`insert`) – Step by Step with ASCII Workflow  
5. Retrieval Algorithm (`get`) – Step by Step with ASCII Workflow  
6. Special Methods (`__setitem__`, `__getitem__`, `__str__`)  
7. Linear Probing vs Quadratic Probing vs Double Hashing  
8. Complexity and Performance Considerations  
9. Recursion? – Why This Implementation is Iterative  
10. Example Walkthrough with a Small Capacity  
11. Conclusion  

---

## 1. Full Commented Code

```python
class Hashmaps:
    """
    A simple hash map implementation using open addressing with linear probing.
    It stores keys and values in two parallel arrays: 'slots' and 'values'.
    """

    def __init__(self, capacity):
        """
        Initialize the hash map with a fixed capacity.

        :param capacity: The size of the internal arrays.
        """
        self.capacity = capacity          # Total number of slots in the table
        self.slots = [None] * self.capacity   # Array to hold keys
        self.values = [None] * self.capacity  # Array to hold corresponding values
        self.size = 0                     # Number of key-value pairs currently stored

    def hash_function(self, key):
        """
        Compute the initial index for a given key.

        :param key: The key to hash.
        :return: An integer index between 0 and capacity-1.
        """
        # Use Python's built-in hash() for any hashable type, then take absolute value
        # and reduce modulo capacity.
        return abs(hash(key)) % self.capacity

    def rehash(self, old_hash_value):
        """
        Compute the next index when a collision occurs (linear probing).

        :param old_hash_value: The previous index that caused a collision.
        :return: The next index to try.
        """
        # Increment by one and wrap around using modulo capacity
        return (old_hash_value + 1) % self.capacity

    def insert(self, key, value):
        """
        Insert a key-value pair into the hash map.
        If the key already exists, its value is updated.
        """
        hash_value = self.hash_function(key)      # Step 1: compute initial index
        initial_index = hash_value

        # Case 1: the computed slot is empty – store key and value directly
        if self.slots[hash_value] is None:
            self.slots[hash_value] = key
            self.values[hash_value] = value
        else:
            # Case 2: slot occupied – check if it's the same key
            if self.slots[hash_value] == key:
                self.values[hash_value] = value   # update existing key
                return
            else:
                # Case 3: collision – linear probe to find an empty slot or the key
                new_hash_value = self.rehash(hash_value)

                # Continue probing until we either find an empty slot or find the key
                while (self.slots[new_hash_value] is not None and
                       self.slots[new_hash_value] != key):
                    new_hash_value = self.rehash(new_hash_value)

                # After exiting loop, either we found an empty slot or the key
                if self.slots[new_hash_value] is None:
                    self.slots[new_hash_value] = key
                    self.values[new_hash_value] = value
                else:  # key found, update its value
                    self.values[new_hash_value] = value

    def get(self, key):
        """
        Retrieve the value associated with a given key.
        Returns "Not Found" if the key does not exist.
        """
        initial_index = self.hash_function(key)
        current_position = initial_index

        # Probe until we hit an empty slot (key definitely not present) or return to start
        while self.slots[current_position] is not None:
            if self.slots[current_position] == key:
                return self.values[current_position]   # key found

            current_position = self.rehash(current_position)

            # If we have traversed the whole table and came back to start, stop
            if current_position == initial_index:
                break

        return "Not Found"

    # Optional: Delete method (commented out in original, but provided for completeness)
    # def delete(self, key):
    #     initial_index = self.hash_function(key)
    #     current_position = initial_index
    #     while self.slots[current_position] is not None:
    #         if self.slots[current_position] == key:
    #             self.slots[current_position] = None
    #             self.values[current_position] = None
    #             print(f"{key} has been deleted")
    #             return
    #         current_position = self.rehash(current_position)
    #         if current_position == initial_index:
    #             break
    #     print(f"{key} is not found")

    def __setitem__(self, key, value):
        """Allow dictionary-like assignment: h[key] = value"""
        self.insert(key, value)

    def __getitem__(self, key):
        """Allow dictionary-like retrieval: h[key]"""
        return self.get(key)

    def __str__(self):
        """Return a string representation of the hash map."""
        result = []
        for i in range(self.capacity):
            if self.slots[i] is not None:
                result.append(f" {self.slots[i]} : {self.values[i]} ")
        return "\n".join(result)


# Example usage
if __name__ == "__main__":
    h1 = Hashmaps(3)
    h1.insert("Apple", 10)
    h1['orange'] = 45
    print(h1['orange'])   # 45
    # print(h1.get("Apple"))
    # h1.delete("Apple")
    # print(h1.get("Apple"))
    # print(h1)
```

---

## 2. Overview of Hashmap and Open Addressing

A hash map (or dictionary) is a data structure that stores key-value pairs and provides average O(1) time complexity for insert, delete, and lookup operations. It works by using a hash function to map each key to an index in an array (the hash table). When two different keys hash to the same index, a **collision** occurs.

**Open addressing** is one method to handle collisions. Instead of storing multiple items in a linked list (separate chaining), we find another empty slot within the same array by **probing** – examining subsequent indices until an empty slot is found. The probing sequence is deterministic and based on the original hash.

In this implementation:
- Two parallel arrays `slots` and `values` store keys and their associated values.
- The capacity is fixed at creation time (no resizing is implemented).
- Linear probing is used: when a collision happens, we move to the next slot (index + 1) and wrap around using modulo capacity.

---

## 3. Lower Layer: Core Components

### 3.1 Constructor (`__init__`)

```
__init__(self, capacity)
    └── self.capacity = capacity
    └── self.slots = [None] * capacity   (holds keys)
    └── self.values = [None] * capacity  (holds values)
    └── self.size = 0
```

- `capacity`: The total number of slots. This determines the size of the arrays.
- `slots`: An array that stores keys (or `None` if empty).
- `values`: An array that stores values corresponding to the keys in `slots`.
- `size`: Keeps track of how many key-value pairs are currently stored (not used in the code but good for future resizing).

**Why separate arrays?**  
This allows us to store keys and values in parallel positions. If `slots[i]` is a key, then `values[i]` is its associated value. If `slots[i]` is `None`, the slot is empty.

### 3.2 Hash Function (`hash_function`)

```
hash_function(key)
    └── return abs(hash(key)) % self.capacity
```

- `hash(key)`: Python’s built-in function that returns an integer hash for any hashable object. For strings, it’s based on the characters; for integers, it’s the integer itself.
- `abs(...)`: Takes the absolute value because Python’s `hash()` can return negative numbers (especially for strings).
- `% self.capacity`: Reduces the hash value to an index between `0` and `capacity-1`.

**Example:**  
If `capacity = 5` and `hash("apple") = 123456789`, then  
`abs(123456789) % 5 = 4`. So "apple" would initially be placed at index 4.

### 3.3 Rehash Function (`rehash`) – Linear Probing

```
rehash(old_hash_value)
    └── return (old_hash_value + 1) % self.capacity
```

- This defines the probing sequence: the next index is simply the previous index plus one, modulo capacity.
- For example, if `capacity = 5` and we are at index 3, the next index is `(3+1) % 5 = 4`, then `(4+1)%5 = 0`, then `1`, `2`, etc., wrapping around.

This is the **linear probing** strategy. The name “rehash” might be slightly misleading; it’s actually the probe step.

---

## 4. Insertion Algorithm (`insert`) – Step by Step with ASCII Workflow

### Steps in `insert(key, value)`:

1. **Compute initial hash**: `hash_value = hash_function(key)`.
2. **If the slot at `hash_value` is `None`**: Place the key and value there. Done.
3. **Else, if the slot contains the same key**: Update the existing value. Done.
4. **Else (collision)**: Start probing:
   - Set `new_hash_value = rehash(hash_value)`.
   - While the slot at `new_hash_value` is not `None` **and** does not contain the key, keep probing: `new_hash_value = rehash(new_hash_value)`.
   - After the loop, either we found an empty slot or we found the key.
   - If empty, insert the key-value pair; if key found, update its value.

### ASCII Workflow for Insert (with capacity = 5)

Let’s simulate inserting keys with linear probing. We’ll use brackets to represent slots, `[ ]` for empty, `[k:v]` for occupied.

Initial table (all empty):

```
Index:   0      1      2      3      4
        [ ]    [ ]    [ ]    [ ]    [ ]
```

**Insert A (hash = 2):** slot 2 empty → place it.

```
Index:   0      1      2      3      4
        [ ]    [ ]    [A:10] [ ]    [ ]
```

**Insert B (hash = 2):** collision at index 2 → probe: (2+1)%5 = 3 → empty → place B.

```
Index:   0      1      2      3      4
        [ ]    [ ]    [A:10] [B:20] [ ]
```

**Insert C (hash = 2):** collision at index 2 → probe to 3 (occupied, not C) → probe to 4 (empty) → place C.

```
Index:   0      1      2      3      4
        [ ]    [ ]    [A:10] [B:20] [C:30]
```

**Insert D (hash = 3):** index 3 occupied but key not D → probe to 4 (occupied, not D) → probe to 0 (empty) → place D.

```
Index:   0      1      2      3      4
        [D:40] [ ]    [A:10] [B:20] [C:30]
```

**Insert A again (hash = 2):** index 2 contains A → update value (e.g., to 15).

```
Index:   0      1      2      3      4
        [D:40] [ ]    [A:15] [B:20] [C:30]
```

### ASCII Representation of Probing Steps (for B’s insertion)

```
Initial hash for B = 2
Check slot 2: [A:10] → occupied, key not B → probe
    → (2+1)%5 = 3 → slot 3: [ ] → empty → insert B at slot 3
```

---

## 5. Retrieval Algorithm (`get`) – Step by Step with ASCII Workflow

### Steps in `get(key)`:

1. Compute initial index: `initial_index = hash_function(key)`.
2. Set `current_position = initial_index`.
3. While the slot at `current_position` is not `None`:
   - If the key matches, return the corresponding value.
   - Else, move to next position: `current_position = rehash(current_position)`.
   - If we have returned to `initial_index` (wrapped around the whole table), stop (key not found).
4. If loop ends without match, return `"Not Found"`.

### ASCII Workflow for Get (using the table from above)

Table state:

```
Index:   0      1      2      3      4
        [D:40] [ ]    [A:15] [B:20] [C:30]
```

**Get D (hash = 3):**
- `initial_index = 3`
- Check slot 3: key = B → no.
- Probe: 3→4 (C) → no.
- Probe: 4→0 (D) → match → return 40.

**Get B (hash = 2):**
- `initial_index = 2`
- Check slot 2: A → no.
- Probe: 2→3 (B) → match → return 20.

**Get X (hash = 4, but X not present):**
- `initial_index = 4`
- Check slot 4: C → not X.
- Probe: 4→0 (D) → not X.
- Probe: 0→1 (empty) → stop → return "Not Found".

---

## 6. Special Methods (`__setitem__`, `__getitem__`, `__str__`)

These make the class behave like a Python dictionary.

- `__setitem__(self, key, value)`: delegates to `insert`. Allows `h[key] = value`.
- `__getitem__(self, key)`: delegates to `get`. Allows `value = h[key]`.
- `__str__(self)`: returns a string representation, showing only occupied slots.

---

## 7. Linear Probing vs Quadratic Probing vs Double Hashing

The code uses **linear probing** (`rehash` increments by 1). However, there are other open addressing strategies:

### Linear Probing
- **Probe sequence:** `h(k, i) = (h(k) + i) mod m` where `i = 0,1,2,...`
- **Pros:** Simple, good cache locality.
- **Cons:** Primary clustering – long runs of occupied slots can form, degrading performance.

### Quadratic Probing
- **Probe sequence:** `h(k, i) = (h(k) + c1*i + c2*i^2) mod m`
- Common choice: `c1 = 1, c2 = 0` → `(h(k) + i^2) mod m`
- **Pros:** Reduces primary clustering; spreads keys better.
- **Cons:** Secondary clustering (keys with same initial hash follow same probe sequence). May not cover all slots if `m` is not chosen carefully.

### Double Hashing
- **Probe sequence:** `h(k, i) = (h1(k) + i * h2(k)) mod m`
- Two hash functions: `h1` gives initial index, `h2` gives the step size (must be non-zero and relatively prime to `m`).
- **Pros:** Virtually eliminates clustering; provides excellent distribution.
- **Cons:** More computation; need to ensure `h2` never returns 0.

**Comparison Table:**

| Method          | Probe Formula                | Clustering     | Cache Locality | Complexity     |
|-----------------|------------------------------|----------------|----------------|----------------|
| Linear Probing  | (h + i) mod m                | Primary        | High           | O(1) average   |
| Quadratic Probing| (h + c1*i + c2*i²) mod m    | Secondary      | Medium         | O(1) average   |
| Double Hashing  | (h1 + i*h2) mod m            | None           | Low            | O(1) average   |

In this code, only linear probing is implemented, but the other methods can be added by modifying the `rehash` function.

---

## 8. Complexity and Performance Considerations

- **Insert:** In the average case (with a good hash function and load factor < 0.7), O(1). In the worst case (all keys hash to same index), O(n) because we may probe through the entire table.
- **Get:** Same as insert: average O(1), worst O(n).
- **Space:** O(capacity) – the arrays are fixed size; unused slots waste memory if capacity is much larger than size.

**Load factor** = `size / capacity`. When it gets too high (e.g., > 0.7), performance degrades because clusters increase. In practice, hash tables resize (double capacity) when load factor exceeds a threshold. This implementation does not resize; it’s a simplified educational example.

---

## 9. Recursion? – Why This Implementation is Iterative

The code contains **no recursive functions**. All operations use loops (while) to probe sequentially. Recursion is not suitable here because:
- Probing requires a linear scan; recursion would add unnecessary function call overhead.
- The number of probes can be large (up to capacity), which could lead to recursion depth errors if implemented recursively.
- Iterative loops are more natural and efficient for this type of sequential search.

If one were to implement a recursive version of `get`, it would look like:
```python
def get_recursive(self, key, current_index, start_index):
    if self.slots[current_index] is None:
        return "Not Found"
    if self.slots[current_index] == key:
        return self.values[current_index]
    next_index = self.rehash(current_index)
    if next_index == start_index:
        return "Not Found"
    return self.get_recursive(key, next_index, start_index)
```
But this is not used because iteration is simpler and safer.

---

## 10. Example Walkthrough with a Small Capacity

Let’s trace through the example from the code:

```python
h1 = Hashmaps(3)
h1.insert("Apple", 10)     # Step 1
h1['orange'] = 45          # Step 2
print(h1['orange'])        # Step 3
```

### Step 1: Insert "Apple" with value 10
- `hash_function("Apple")` → suppose returns 1 (for capacity 3, maybe `abs(hash("Apple")) % 3 = 1`).
- `slots[1]` is `None` → store key "Apple" at index 1, value 10 at index 1.

Table:
```
Index:   0      1        2
        [ ]    [Apple:10] [ ]
```

### Step 2: Insert "orange" with value 45 using `__setitem__` (calls `insert`)
- `hash_function("orange")` → suppose returns 1 as well (collision).
- `slots[1]` is occupied with key "Apple" (not "orange").
- Start probing: `new_hash_value = rehash(1) = (1+1)%3 = 2`.
- `slots[2]` is `None` → store "orange" at index 2, value 45.

Table:
```
Index:   0      1        2
        [ ]    [Apple:10] [orange:45]
```

### Step 3: Retrieve `h1['orange']` (calls `__getitem__` → `get`)
- `initial_index = hash_function("orange") = 1`.
- `current_position = 1`, `slots[1] = "Apple"` → not match.
- Probe: `current_position = rehash(1) = 2`.
- `slots[2] = "orange"` → match → return 45.
- Output: `45`.

---

## 11. Conclusion

This implementation provides a clear, educational example of a hash map using open addressing with linear probing. It demonstrates:
- The need for a hash function to map keys to indices.
- Collision handling via probing.
- Separate arrays for keys and values.
- Dictionary-like syntax via special methods.

For production use, one would add resizing, better handling of deletions (tombstones), and possibly switch to quadratic probing or double hashing to reduce clustering. Nonetheless, this code serves as a solid foundation for understanding the inner workings of hash tables.

**Key takeaways:**
- Open addressing keeps all data within the array.
- Linear probing is simple but can cause clustering.
- The algorithm is iterative, not recursive.
- Average case operations are O(1) when load factor is low.