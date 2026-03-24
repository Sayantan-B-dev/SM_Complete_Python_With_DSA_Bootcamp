### Enhanced Hashmap Implementation: Prime Capacity and Uniform Distribution

The following implementation improves upon the basic separate-chaining hashmap by incorporating a prime‑based capacity strategy. Prime capacities reduce clustering and improve the uniformity of index distribution when using modulo compression.

#### Design Improvements

1. **Prime Capacity**  
   The underlying array size is always a prime number. When resizing, the new capacity is the next prime greater than twice the old capacity. This minimizes collisions caused by hash codes that share common factors with the array size.

2. **Hash Compression**  
   The index is computed as `hash(key) % capacity`. Using a prime modulus ensures that the index distribution remains uniform even if the hash function’s lower bits exhibit patterns.

3. **Load Factor Management**  
   Resizing occurs when the load factor exceeds 0.75. The new capacity is the smallest prime greater than `2 * old_capacity`.

4. **Hash Function**  
   Python’s built‑in `hash()` provides a good distribution. No additional transformation is required; the modulo operation with a prime capacity handles the rest.

```python
import math
from typing import TypeVar, Generic, Optional, List

K = TypeVar('K')
V = TypeVar('V')

class EnhancedHashMap(Generic[K, V]):
    """
    A hashmap using separate chaining with prime capacities for better distribution.
    """

    class _Node:
        __slots__ = ('key', 'value', 'next')
        def __init__(self, key: K, value: V):
            self.key = key
            self.value = value
            self.next: Optional[EnhancedHashMap._Node] = None

    @staticmethod
    def _is_prime(n: int) -> bool:
        """Check if n is a prime number."""
        if n < 2:
            return False
        if n == 2 or n == 3:
            return True
        if n % 2 == 0:
            return False
        limit = int(math.isqrt(n))
        for i in range(3, limit + 1, 2):
            if n % i == 0:
                return False
        return True

    @staticmethod
    def _next_prime(n: int) -> int:
        """Return the smallest prime >= n."""
        while not EnhancedHashMap._is_prime(n):
            n += 1
        return n

    def __init__(self, initial_capacity: int = 7, load_factor: float = 0.75):
        # Ensure initial capacity is prime
        self._capacity = self._next_prime(max(2, initial_capacity))
        self._load_factor = load_factor
        self._size = 0
        self._buckets: List[Optional[EnhancedHashMap._Node]] = [None] * self._capacity

    def _hash(self, key: K) -> int:
        """
        Compute the bucket index for a key.
        Uses Python's built-in hash and reduces modulo the prime capacity.
        """
        return hash(key) % self._capacity

    def _resize(self) -> None:
        """
        Double the capacity and rehash all entries.
        New capacity is the smallest prime greater than twice the old capacity.
        """
        old_buckets = self._buckets
        new_capacity = self._next_prime(self._capacity * 2)
        self._capacity = new_capacity
        self._buckets = [None] * self._capacity
        self._size = 0

        # Reinsert all entries
        for head in old_buckets:
            current = head
            while current:
                self.put(current.key, current.value)
                current = current.next

    def put(self, key: K, value: V) -> None:
        """Insert or update a key-value pair."""
        # Trigger resize if load factor threshold is exceeded
        if self._size >= self._capacity * self._load_factor:
            self._resize()

        index = self._hash(key)
        head = self._buckets[index]
        current = head

        # Check if key already exists
        while current:
            if current.key == key:
                current.value = value
                return
            current = current.next

        # Key not found; prepend new node
        new_node = self._Node(key, value)
        new_node.next = head
        self._buckets[index] = new_node
        self._size += 1

    def get(self, key: K) -> Optional[V]:
        """Retrieve the value for a key, or None if not found."""
        index = self._hash(key)
        current = self._buckets[index]
        while current:
            if current.key == key:
                return current.value
            current = current.next
        return None

    def remove(self, key: K) -> Optional[V]:
        """Remove and return the value for a key, or None if not found."""
        index = self._hash(key)
        head = self._buckets[index]
        dummy = self._Node(None, None)  # sentinel for head removal
        dummy.next = head
        prev = dummy
        current = head

        while current:
            if current.key == key:
                prev.next = current.next
                if prev is dummy:
                    self._buckets[index] = current.next
                self._size -= 1
                return current.value
            prev = current
            current = current.next
        return None

    def keys(self) -> List[K]:
        """Return a list of all keys."""
        result = []
        for head in self._buckets:
            current = head
            while current:
                result.append(current.key)
                current = current.next
        return result

    def __len__(self) -> int:
        return self._size
```

#### Why Prime Capacity Matters

When using modulo compression, the quality of the index distribution depends on the relationship between the hash values and the array size.

- **Composite capacities** can cause collisions when the hash values share common factors with the capacity. For example, if the hash function produces only even numbers and the capacity is a power of two, all indices will be even, leaving half the array unused.
- **Prime capacities** ensure that the modulo operation produces a uniform distribution regardless of the hash’s divisibility patterns. No non‑trivial divisor exists, so any bias in the hash values is less likely to lead to systematic collisions.

The implementation uses a prime‑finding helper to enforce this property during initialization and resizing. While this adds a small overhead, it significantly improves collision resistance in practice.

#### Time and Space Complexity

- **Average case**: O(1) for insert, lookup, delete.
- **Worst case**: O(n) if hash function fails (still possible, but prime capacity reduces likelihood).
- **Space**: O(n), with the array size always prime and at least 2× the number of entries due to load factor.

#### Limitations

- The prime check and search for the next prime add O(sqrt(n)) overhead during resizing. For most applications this is negligible.
- The use of separate chaining still requires linked list traversal; open addressing may offer better cache locality but is more complex to implement safely.