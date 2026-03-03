# Python Sets

## 1. Definition and Concept Overview
A set is an unordered collection of unique, hashable objects. Sets are mutable – elements can be added or removed after creation. They do not permit duplicates; adding an element already present has no effect. The primary characteristics are:
- **Unordered**: No guarantee of element order (though insertion order may be preserved in CPython 3.7+ as an implementation detail, but should not be relied upon).
- **Unique elements**: Each element appears at most once.
- **Hash-based**: Membership testing, addition, and removal are average O(1) due to underlying hash table.

## 2. Core Principles and Internal Mechanics
- **Hash table implementation**: A set uses an array of buckets. Each element’s hash value determines the bucket index. Collisions are resolved via open addressing (typically).
- **Hashing requirement**: All elements must be hashable (immutable built-in types like int, float, str, tuple are hashable; list, dict, set are not).
- **Load factor and resizing**: When the number of elements exceeds a threshold relative to table size, the table is rehashed to a larger capacity to maintain O(1) operations.
- **Element equality**: Two objects are considered duplicates if they compare equal (`==`) and have the same hash. Custom classes must define `__hash__` and `__eq__` appropriately to be usable in sets.
- **Memory overhead**: Sets consume more memory than lists due to hash table overhead and empty buckets.

## 3. Step-by-Step Logical Breakdown
- **Creation**: Allocate an empty hash table or populate from an iterable. For each element, compute hash, insert into table if not already present.
- **Add element**: Compute hash → locate bucket → if bucket empty or contains an element that is not equal, place new element. If element with same hash and equality exists, do nothing.
- **Remove element**: Compute hash → locate bucket → if found and equal, remove entry and possibly mark as dummy for collision chain.
- **Membership test**: Compute hash → locate bucket → check equality with any element in that bucket chain.
- **Set operations**:
  - Union: Iterate over both sets, add each element to a new set.
  - Intersection: Iterate over smaller set, check membership in larger set, collect common elements.
  - Difference: Iterate over first set, collect elements not in second set.
  - Symmetric difference: Collect elements in either set but not both.

## 4. Implementation Examples

### 4.1 Creating Sets
```python
# Empty set – must use set(), not {} (which creates dict)
empty_set = set()

# Set literal with elements
numbers = {1, 2, 3, 4, 5}

# Using set() constructor from any iterable
from_list = set([1, 2, 2, 3])          # {1, 2, 3} – duplicates removed
from_string = set("hello")              # {'h', 'e', 'l', 'o'} – order not preserved
from_tuple = set((1, 2, 3))

# Set comprehension
squares = {x**2 for x in range(5)}      # {0, 1, 4, 9, 16}
```

### 4.2 Type Casting List with set()
```python
# Convert list to set – removes duplicates
data = [3, 1, 4, 1, 5, 9, 2, 6, 5]
unique = set(data)                       # {1, 2, 3, 4, 5, 6, 9}

# Useful for deduplication while preserving order? Not directly – sets unordered.
# To preserve order, use dict.fromkeys(data) (Python 3.7+)
ordered_unique = list(dict.fromkeys(data))
```

### 4.3 Basic Set Operations (Add, Remove, Discard, Pop, Clear)
```python
s = {10, 20, 30}

# Add element – O(1) average
s.add(40)                                # {10, 20, 30, 40}
s.add(20)                                 # no change (already present)

# Remove – raises KeyError if element missing
s.remove(20)                              # {10, 30, 40}
# s.remove(99)                            # KeyError: 99

# Discard – no error if element missing
s.discard(99)                             # no effect, no error

# Pop – removes and returns an arbitrary element
# Raises KeyError if set empty
elem = s.pop()                             # e.g., 10 (arbitrary)
print(s)                                   # {30, 40} (or whatever remains)

# Clear – removes all elements
s.clear()                                  # set()
```

### 4.4 Errors with Remove, Pop, Discard
```python
# KeyError from remove on non-existent element
s = {1, 2, 3}
try:
    s.remove(4)
except KeyError as e:
    print(f"KeyError: {e}")                # KeyError: 4

# KeyError from pop on empty set
empty = set()
# empty.pop()                               # KeyError: 'pop from an empty set'

# discard is safe – no exception
empty.discard(5)                             # nothing happens
```

### 4.5 Set Membership Test
```python
colors = {"red", "green", "blue"}

# Membership test – O(1) average
is_present = "green" in colors              # True
is_absent = "yellow" not in colors          # True

# Use membership for filtering
valid_codes = {200, 201, 204}
code = 404
if code in valid_codes:
    print("Valid response code")
else:
    print("Unexpected code")
```

### 4.6 Mathematical Operations (Union, Intersection, Difference, Symmetric Difference)
```python
a = {1, 2, 3}
b = {3, 4, 5}

# Union – elements in either set
union1 = a | b                              # {1, 2, 3, 4, 5}
union2 = a.union(b)                         # same

# Intersection – elements in both
intersection1 = a & b                        # {3}
intersection2 = a.intersection(b)            # same

# Difference – elements in a but not b
diff1 = a - b                                # {1, 2}
diff2 = a.difference(b)                      # same

# Symmetric difference – elements in exactly one set
symdiff1 = a ^ b                              # {1, 2, 4, 5}
symdiff2 = a.symmetric_difference(b)          # same

# Update versions (modify original set)
a.update(b)                                   # a now {1,2,3,4,5} (union)
a.intersection_update(b)                      # a now {3,4,5} (intersection)
a.difference_update(b)                        # a now set() (difference)
a.symmetric_difference_update(b)               # a now {3,4,5}? (depends)
```

### 4.7 Set Methods: issubset, issuperset, and Comparisons
```python
x = {1, 2, 3}
y = {1, 2}
z = {1, 2, 3, 4}

# Subset: all elements of first are in second
is_sub = y.issubset(x)                        # True (y ⊆ x)
is_sub_op = y <= x                             # True (same)

# Proper subset: y ⊂ x
is_proper_sub = y < x                          # True (y ⊆ x and y != x)

# Superset: all elements of second are in first
is_super = x.issuperset(y)                     # True (x ⊇ y)
is_super_op = x >= y                            # True

# Proper superset
is_proper_super = x > y                         # True

# Disjoint sets – no common elements
a = {1, 2}
b = {3, 4}
are_disjoint = a.isdisjoint(b)                  # True
```

### 4.8 String Separation with Sets
```python
# Extract unique characters from a string
text = "abracadabra"
unique_chars = set(text)                        # {'a', 'b', 'c', 'd', 'r'}

# Count distinct characters in a string
distinct_count = len(set(text))                  # 5

# Check if a string has all unique characters
def all_unique(s):
    return len(set(s)) == len(s)

# Find characters present in both strings
str1 = "hello"
str2 = "world"
common = set(str1) & set(str2)                   # {'o', 'l'}

# Find characters in one string but not another
diff = set(str1) - set(str2)                     # {'h', 'e'}
```

### 4.9 Common Errors and Pitfalls
```python
# 1. Using {} for empty set – creates dict, not set
empty_dict = {}                                   # type dict
empty_set = set()                                 # correct

# 2. Adding unhashable types
s = set()
# s.add([1, 2])                                   # TypeError: unhashable type: 'list'
# Solution: use tuple instead
s.add((1, 2))                                     # OK

# 3. Modifying set while iterating
numbers = {1, 2, 3, 4, 5}
# for n in numbers:
#     if n % 2 == 0:
#         numbers.remove(n)                       # RuntimeError: set changed size during iteration
# Correct: iterate over copy
for n in numbers.copy():
    if n % 2 == 0:
        numbers.remove(n)

# 4. Assuming order
s = {3, 1, 2}
print(s)                                           # {1, 2, 3} (order may vary, do not rely)

# 5. Using set as a key in another set/dict – not possible
# set of sets: use frozenset
inner = frozenset([1, 2])
outer = {inner}                                    # OK
```

## 5. Time and Space Complexity Analysis
| Operation               | Time Complexity (Average) | Notes                                      |
|-------------------------|----------------------------|--------------------------------------------|
| Add (`add`)             | O(1)                       | May trigger rehash if load factor exceeded |
| Remove (`remove` / `discard`) | O(1)                | Hash and locate                            |
| Pop                     | O(1)                       | Removes arbitrary element                   |
| Membership (`in`)       | O(1)                       |                                            |
| Union (`|` / `union`)   | O(len(s) + len(t))         | Constructs new set                         |
| Intersection (`&`)      | O(min(len(s), len(t)))     | Iterates over smaller set                   |
| Difference (`-`)        | O(len(s))                  | Iterates over first set                     |
| Symmetric difference (`^`)| O(len(s) + len(t))       |                                            |
| Subset check (`<=`)     | O(len(subset))             | Iterates over subset                        |

Space complexity: The set itself requires O(n) storage for the hash table, which typically has extra capacity (load factor < 1). Memory overhead per element is higher than list due to bucket array.

## 6. Edge Cases and Failure Scenarios
- **Empty set**: `pop()` raises `KeyError`; `remove()` also raises; `discard()` safe; iteration yields nothing.
- **Single element set**: Works normally.
- **Unhashable elements**: `TypeError` on insertion.
- **Custom objects**: Must define `__hash__` and `__eq__` consistently; if mutable, hash may change after insertion leading to unexpected behavior.
- **Rehashing during iteration**: Not allowed; modifying size while iterating raises `RuntimeError`.
- **Floating point issues**: `NaN` compares unequal to itself, so multiple `float('nan')` may appear as distinct elements (implementation‑dependent).
- **Large sets**: Performance degrades if hash collisions become frequent (e.g., poor hash function).

## 7. Practical Use Cases
- **Deduplication**: Removing duplicates from a sequence (order not required).
- **Membership testing**: Fast lookups for large collections.
- **Set operations**: Comparing groups (e.g., user permissions, tags, categories).
- **Graph algorithms**: Tracking visited nodes (unique).
- **Data cleaning**: Identifying unique values in a column.
- **String analysis**: Finding unique characters, common letters between words.
- **Avoiding duplicates in real‑time**: Maintain a set of seen items.

## 8. Limitations and Trade-offs
- **Unordered**: Cannot rely on element order; if order is needed, use `list` or `collections.OrderedDict`.
- **Hashable requirement**: Cannot store mutable containers like lists or dicts directly.
- **Memory overhead**: Larger than list for small collections; not suitable when memory is critical.
- **No indexing**: Cannot access by position; must iterate or convert to list.
- **Rehashing cost**: Occasional O(n) resize during adds; not suitable for real‑time systems with strict latency guarantees.
- **Thread safety**: Not thread‑safe for concurrent modifications without external locks.
- **Performance degradation**: Poor hash functions can lead to many collisions, reducing to O(n) in worst case.