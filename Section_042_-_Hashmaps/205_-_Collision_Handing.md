### Collision Resolution in Hashmaps

A collision occurs when two distinct keys hash to the same bucket index. Without a resolution strategy, subsequent insertions would overwrite existing entries or be lost. Two primary families of solutions exist: **open addressing** (closed hashing) and **closed addressing** (separate chaining). Each maintains the integrity of the hashmap while managing collisions differently.

---

## 1. Open Addressing (Closed Hashing)

In open addressing, all entries are stored directly in the bucket array. When a collision occurs, the algorithm probes for the next available slot according to a deterministic sequence. The table size must be at least as large as the number of stored entries, and deletion requires special handling (tombstones).

The general probe function is:

```
g(i) = (h(key) + f(i)) % capacity
```

where `i` is the probe number (starting from 0), and `f(i)` defines the probe sequence.

### 1.1 Linear Probing

`f(i) = i`

The probe sequence walks sequentially through the array. For each increment, the index advances by one (modulo capacity).

**Behavior**:
- Simple to implement.
- Primary clustering: consecutive occupied slots form clusters, increasing the average probe length.
- Cache‑friendly due to sequential memory access.

**Implementation sketch**:

```python
def _find_slot_linear(self, key, for_insert=False):
    index = self._hash(key)
    start = index
    i = 0
    while self._buckets[index] is not None:
        entry = self._buckets[index]
        if entry.key == key:
            return index, True   # found
        i += 1
        index = (start + i) % self._capacity
        # Detect full table (should not happen if load factor enforced)
    return index, False   # empty slot
```

### 1.2 Quadratic Probing

`f(i) = i²`

The probe sequence moves by increasing squares: index, index+1, index+4, index+9, …

**Behavior**:
- Reduces primary clustering but introduces secondary clustering: keys that hash to the same initial index follow the same probe sequence.
- Guarantees that all slots are visited if the capacity is a prime number and the load factor < 0.5.

**Implementation sketch**:

```python
def _find_slot_quadratic(self, key, for_insert=False):
    index = self._hash(key)
    i = 0
    while self._buckets[index] is not None:
        entry = self._buckets[index]
        if entry.key == key:
            return index, True
        i += 1
        index = (index + 2*i - 1) % self._capacity   # incremental square
    return index, False
```

### 1.3 Double Hashing

`f(i) = i * h₂(key)`, where `h₂` is a second hash function.

The second hash function should never return 0 and should be independent of the first. A common choice: `h₂(key) = 1 + (hash(key) % (capacity - 1))`.

**Behavior**:
- Produces a pseudo‑random probe sequence unique to each key, virtually eliminating clustering.
- Requires two hash calculations per operation.
- Works best when capacity is prime.

**Implementation sketch**:

```python
def _hash2(self, key):
    # Returns a step size relatively prime to capacity
    return 1 + (hash(key) % (self._capacity - 1))

def _find_slot_double(self, key, for_insert=False):
    index = self._hash(key)
    step = self._hash2(key)
    i = 0
    while self._buckets[index] is not None:
        entry = self._buckets[index]
        if entry.key == key:
            return index, True
        i += 1
        index = (index + step) % self._capacity
    return index, False
```

---

## 2. Closed Addressing (Separate Chaining)

Each bucket in the array holds a **head pointer to a linked list** (or another structure) containing all key‑value pairs that hash to that index. Collisions are resolved by appending to the chain.

**Advantages**:
- No need to resize as long as chains remain manageable.
- Deletion is straightforward (unlinking a node).
- Works well even with high load factors.

**Disadvantages**:
- Additional memory overhead for pointers.
- Poor cache locality compared to open addressing.

**Enhancement: Balanced Trees**  
When chains become long, performance degrades to O(n). To mitigate this, a chain can be promoted to a balanced binary search tree (e.g., red‑black tree) after exceeding a threshold (e.g., 8). This maintains O(log n) worst‑case operations for that bucket.

**Implementation pattern**:

```python
class _TreeNode:
    # Red‑black tree node implementation
    pass

def _get_bucket(self, key):
    index = self._hash(key)
    entry = self._buckets[index]
    if isinstance(entry, _TreeNode):
        # search tree
        return self._tree_search(entry, key)
    else:
        # linear search through linked list
        current = entry
        while current:
            if current.key == key:
                return current
            current = current.next
        return None
```

---

## 3. Comparison and Trade‑offs

| Strategy       | Best for                              | Memory Use | Cache Locality | Worst‑Case Operations |
|----------------|---------------------------------------|------------|----------------|-----------------------|
| Linear Probing | Small to medium tables, simple code   | Low        | Excellent      | O(n) (clustering)     |
| Quadratic      | Moderate load, moderate clustering    | Low        | Good           | O(n) (secondary clusters) |
| Double Hashing | High‑performance, minimal clustering  | Low        | Good           | O(n) (still possible) |
| Separate Chaining | High load factors, frequent deletions | Moderate   | Poor           | O(n) (chain length) |
| Chaining + BST | Unpredictable key distribution        | Higher     | Poor           | O(log n) (per bucket) |

---

## 4. Choosing a Strategy

- **Open addressing** is preferred when memory is tight and the table size is carefully managed (load factor < 0.7). Linear probing offers simplicity; double hashing provides the best distribution.
- **Separate chaining** is simpler to implement correctly, handles high load factors gracefully, and avoids tombstone complexities. The addition of tree‑based buckets provides a robust worst‑case guarantee.

Modern hashmap implementations often combine techniques: Python’s `dict` uses open addressing with pseudo‑random probing; Java’s `HashMap` uses separate chaining that converts long chains to balanced trees (since Java 8). The choice depends on the expected workload and performance requirements.

---

The core principle remains: a well‑designed hash function combined with an appropriate collision resolution strategy ensures that the hashmap maintains its O(1) average‑case performance across a wide range of use cases.