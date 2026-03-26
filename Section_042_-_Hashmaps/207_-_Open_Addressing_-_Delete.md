```py
class Hashmaps:
    """
    A hash map implementation using separate arrays for keys and values,
    with linear probing for collision resolution. Deleted slots are marked
    with a special sentinel to avoid breaking probe sequences.
    """
    _DELETED = object()  # unique sentinel to mark deleted slots

    def __init__(self, capacity=8):
        """
        Initialize the hash map with a given initial capacity.
        :param capacity: initial number of slots (will be doubled on resize)
        """
        self.capacity = capacity
        self.slots = [None] * capacity        # stores keys
        self.values = [None] * capacity       # stores values
        self.size = 0                         # number of active (non-deleted) entries

    def _hash(self, key):
        """
        Compute the initial index for a key using Python's built-in hash.
        """
        return abs(hash(key)) % self.capacity

    def _next_probe(self, index):
        """
        Linear probing: move to the next slot (wrap around at capacity).
        """
        return (index + 1) % self.capacity

    def _resize(self):
        """
        Double the capacity when load factor exceeds 0.7.
        Rehash all active entries into the new larger table.
        """
        old_slots = self.slots
        old_values = self.values
        old_capacity = self.capacity

        # double capacity
        self.capacity *= 2
        self.slots = [None] * self.capacity
        self.values = [None] * self.capacity
        self.size = 0

        # re-insert all non-deleted entries
        for i in range(old_capacity):
            key = old_slots[i]
            if key is not None and key is not self._DELETED:
                self.insert(key, old_values[i])

    def insert(self, key, value):
        """
        Insert or update a key-value pair.
        If the load factor exceeds 0.7, resize first.
        """
        # optional: resize if table is getting full
        if self.size >= self.capacity * 0.7:
            self._resize()

        index = self._hash(key)
        first_tombstone = None   # remember the first deleted slot we encounter

        # search for the key or an empty slot
        while self.slots[index] is not None and self.slots[index] is not self._DELETED:
            if self.slots[index] == key:
                # key already exists -> update value
                self.values[index] = value
                return
            # remember first tombstone if we haven't already
            if first_tombstone is None and self.slots[index] is self._DELETED:
                first_tombstone = index
            index = self._next_probe(index)

        # we exited because we hit None or a DELETED marker
        # if we found a tombstone earlier, use it; otherwise use the current index
        if first_tombstone is not None:
            index = first_tombstone

        self.slots[index] = key
        self.values[index] = value
        self.size += 1

    def get(self, key):
        """
        Retrieve the value associated with the key.
        Raises KeyError if the key is not present.
        """
        index = self._hash(key)
        start = index

        while self.slots[index] is not None:
            if self.slots[index] == key:
                return self.values[index]
            index = self._next_probe(index)
            if index == start:      # we have looped around the whole table
                break

        raise KeyError(key)

    def delete(self, key):
        """
        Remove the key-value pair from the hash map.
        Marks the slot as DELETED (tombstone) instead of None.
        Prints a message and does nothing if key not found.
        """
        index = self._hash(key)
        start = index

        while self.slots[index] is not None:
            if self.slots[index] == key:
                # mark as deleted, not None, to keep probe chains intact
                self.slots[index] = self._DELETED
                self.values[index] = None
                self.size -= 1
                print(f"{key} has been deleted")
                return
            index = self._next_probe(index)
            if index == start:
                break

        print(f"{key} not found")

    # ----- magic methods for Pythonic behaviour -----
    def __setitem__(self, key, value):
        """Allow assignment using h[key] = value"""
        self.insert(key, value)

    def __getitem__(self, key):
        """Allow retrieval using h[key]"""
        return self.get(key)

    def __str__(self):
        """Return a human-readable representation of the hash map"""
        lines = []
        for i in range(self.capacity):
            key = self.slots[i]
            if key is not None and key is not self._DELETED:
                lines.append(f"{key} : {self.values[i]}")
        return "\n".join(lines)
    


h1 = Hashmaps(3)
h1.insert("Apple",10)
h1['orange'] = 45
print(h1['orange'])
# print(h1.get("Apple"))
# h1.delete("Apple")
# print(h1.get("Apple"))
# print(h1)

```

## Hashmap Implementation Notes

### 1. Definition
A **hash map** (or hash table) is a data structure that stores key‑value pairs. It uses a **hash function** to compute an index into an array where the key’s value is stored. Ideally, each key maps to a unique index, but collisions (two keys mapping to the same index) must be handled. This implementation uses **linear probing** (a form of open addressing) to resolve collisions.

---

### 2. Algorithm Overview

#### 2.1 Hash Function
- Computes an integer hash of the key (using Python’s built‑in `hash()`).
- Takes the absolute value modulo the table capacity to produce a valid array index.

#### 2.2 Insertion
1. **Resize** if load factor (`size / capacity`) exceeds 0.7.
2. Compute the initial index.
3. **Probe** linearly:
   - If the slot is empty (`None`) or contains a **tombstone** (`_DELETED`), place the key‑value pair there.
   - If the slot contains the same key, update its value.
   - Otherwise, move to the next slot (wrap around at the end).
4. If a tombstone was encountered earlier, reuse that slot to avoid wasting space.

#### 2.3 Retrieval (`get`)
- Start at the hash index.
- Probe sequentially:
  - If the slot is `None`, the key is not present → raise `KeyError`.
  - If the slot contains the key, return the value.
  - If it contains a different key (or a tombstone), continue probing.
- If the entire table is traversed without finding the key, raise `KeyError`.

#### 2.4 Deletion
- **Important**: Instead of setting a deleted slot to `None` (which would break probing chains), mark it with a special sentinel `_DELETED`.
- Start probing from the hash index.
- If the key is found, set its slot to `_DELETED` and its value to `None`. Decrease the size.
- If the key is not found after a full cycle, print “not found”.

#### 2.5 Resizing
- When the load factor exceeds 0.7, the capacity is doubled.
- All existing (non‑deleted) entries are re‑inserted into the new larger array. This rehashes them, which typically reduces collisions.

---

### 3. Key Concepts Explained

| Concept | Explanation |
|---------|-------------|
| **Hash Function** | Transforms a key into an array index. A good hash distributes keys uniformly. |
| **Collision** | Two different keys produce the same index. |
| **Linear Probing** | On collision, check the next slot (index+1, then +2, …) until an empty or deleted slot is found. |
| **Tombstone** | A marker that a slot was previously occupied but now deleted. It allows probing to continue past deleted entries without breaking the search. |
| **Load Factor** | `size / capacity`. When it grows too high, performance degrades, so the table is resized (doubled). |
| **Magic Methods** | `__setitem__` and `__getitem__` enable the dictionary‑like syntax `h[key] = value` and `h[key]`. `__str__` provides a nice string representation. |

---

### 4. Why Tombstones Are Necessary

If a deleted slot were set to `None`, a later `get` for a key that had been inserted *after* the deleted one would stop probing when it encounters that `None`. The key would be “lost” because the probe sequence would be prematurely terminated. By using a tombstone, we signal that the slot is available for new insertions but must still be considered during searches.

---

### 5. Magic Methods in Detail

- **`__setitem__(self, key, value)`** – Called when you do `h[key] = value`. It simply calls `insert(key, value)`.
- **`__getitem__(self, key)`** – Called when you do `h[key]`. It calls `get(key)` and raises `KeyError` if the key is missing (matching Python’s dict behaviour).
- **`__str__(self)`** – Called by `print(h)`. It returns a formatted string of all active key‑value pairs.

---

### 6. Complexity Analysis

- **Average case** (with a good hash and low load factor):
  - Insert, get, delete: **O(1)**
- **Worst case** (many collisions, or many tombstones):
  - All operations: **O(n)** (where n is the number of entries)
- **Resize**: O(n) amortized over many insertions.

---

### 7. Limitations / Notes

- This implementation **does not** handle keys that are unhashable (e.g., lists). Python’s `hash()` already enforces this.
- It is **not thread‑safe**.
- The sentinel `_DELETED` is a class‑level object, which is safe because it is never equal to any real key.
- The capacity is always a power of two after the first resize (since we double each time), which helps with modulo operations.

---

These notes cover the essential algorithm, definitions, and reasoning behind the code. If you need further clarification on any point, let me know.