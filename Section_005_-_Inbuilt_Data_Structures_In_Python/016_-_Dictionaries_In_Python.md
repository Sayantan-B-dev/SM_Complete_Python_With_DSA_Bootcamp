# Python Dictionaries

## 1. Definition and Concept Overview
A dictionary is an unordered (in Python <3.7) or insertion-ordered (Python ≥3.7) collection of key-value pairs. Each key must be unique and hashable (immutable). Dictionaries are mutable and provide amortized O(1) access, insertion, and deletion by key. They are implemented as hash tables, offering a mapping between keys and values.

## 2. Core Principles and Internal Mechanics
- **Hash table**: The underlying array stores entries (hash, key, value). The hash of a key determines the bucket index. Collisions are resolved via open addressing (usually).
- **Key requirements**: Keys must be hashable (implement `__hash__` and `__eq__`). Built-in immutable types (int, float, str, tuple) are hashable; mutable types (list, dict, set) are not.
- **Resizing**: When the load factor exceeds a threshold, the table is resized and rehashed to maintain performance. This operation is O(n) but infrequent.
- **Insertion order preservation**: Since Python 3.7, dictionaries preserve insertion order as a language specification. This is achieved by maintaining a doubly-linked list of entries alongside the hash table.
- **Memory**: Dictionaries trade memory for speed. Overhead includes hash table array, entry structures, and (in modern CPython) combined table storing both hash/key/value in a single array for compactness.

## 3. Step-by-Step Logical Breakdown
- **Creation**: Allocate hash table; for each key-value pair, compute key hash, insert into table (handle collisions). 
- **Key lookup**: Compute hash → locate bucket → compare keys (if bucket occupied) → return value if match; else continue collision chain.
- **Insert/Update**: Compute hash → locate bucket → if bucket empty, insert new entry; if bucket contains equal key, replace value.
- **Delete**: Compute hash → locate entry → mark as dummy (tombstone) to preserve collision chains; actual removal may happen during resize.
- **Iteration**: Traverse the entries in insertion order (modern dict) or hash order (older). Yields keys, values, or items.

## 4. Implementation Examples

### 4.1 Creating Dictionaries
```python
# Empty dictionary
empty = {}

# Literal with key-value pairs
person = {"name": "Alice", "age": 30, "city": "New York"}

# Using dict() constructor from sequence of pairs
items = [("name", "Bob"), ("age", 25)]
person2 = dict(items)

# From keyword arguments – keys must be valid identifiers
person3 = dict(name="Charlie", age=35, city="London")

# Using dict.fromkeys() – all keys map to same value
keys = ["a", "b", "c"]
default_dict = dict.fromkeys(keys, 0)          # {'a': 0, 'b': 0, 'c': 0}

# Dict comprehension – squares mapping
squares = {x: x**2 for x in range(5)}          # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

### 4.2 Accessing Dictionary Elements
```python
data = {"name": "Diana", "age": 28, "country": "UK"}

# Direct key access – raises KeyError if missing
name = data["name"]                             # "Diana"
# city = data["city"]                            # KeyError: 'city'

# get() method – returns None (or default) if missing
city = data.get("city")                          # None
city_default = data.get("city", "Unknown")       # "Unknown"

# Accessing with default using setdefault – inserts default if missing
# Returns value for key, sets default if absent
age = data.setdefault("age", 99)                 # age already 28, returns 28
country = data.setdefault("country", "USA")      # country already "UK", returns "UK"
job = data.setdefault("job", "Engineer")         # inserts "job": "Engineer", returns "Engineer"
```

### 4.3 Handling Keys Not Present with Default Value
```python
counts = {"apple": 5, "banana": 3}

# Use get() for read-only default
orange_count = counts.get("orange", 0)           # 0

# Use setdefault() when you want to set default and also get the value
# Useful for initializing nested structures
inventory = {}
fruit = "pear"
inventory.setdefault(fruit, []).append(10)       # inventory = {"pear": [10]}

# Alternative: collections.defaultdict for automatic defaults
from collections import defaultdict
dd = defaultdict(int)                            # default 0 for missing keys
dd["apple"] += 1                                  # works without pre-initializing
```

### 4.4 Modifying Dictionary Elements
```python
user = {"name": "Eve", "age": 32}

# Adding a new key-value pair
user["email"] = "eve@example.com"                # {"name": "Eve", "age": 32, "email": "eve@example.com"}

# Updating existing value
user["age"] = 33                                  # {"name": "Eve", "age": 33, "email": "eve@example.com"}

# Using update() to merge another dict or iterable of pairs
user.update({"city": "Paris", "age": 34})         # adds city, updates age
user.update([("phone", "123456"), ("age", 35)])   # works with any iterable of pairs

# Deleting a key
del user["phone"]                                 # removes key "phone"; raises KeyError if missing

# pop() removes key and returns value
email = user.pop("email")                         # removes "email", returns "eve@example.com"
# email = user.pop("fax", None)                    # safe: returns None if missing

# popitem() removes and returns an arbitrary (key, value) pair (LIFO order in Python 3.7+)
key, value = user.popitem()                       # removes last inserted item

# clear() removes all items
user.clear()                                      # user becomes {}
```

### 4.5 Dictionary Methods: keys, values, items
```python
scores = {"math": 90, "physics": 85, "chemistry": 92}

# keys() returns a view of keys (dynamic)
keys_view = scores.keys()                          # dict_keys(['math', 'physics', 'chemistry'])
for subject in keys_view:
    print(subject)

# values() returns a view of values
values_view = scores.values()                      # dict_values([90, 85, 92])

# items() returns a view of (key, value) pairs
items_view = scores.items()                        # dict_items([('math', 90), ('physics', 85), ('chemistry', 92)])

# Views reflect changes to the dictionary
scores["biology"] = 88
print(keys_view)                                   # includes "biology" now

# Convert to list if needed
keys_list = list(scores.keys())
```

### 4.6 Shallow Copy – What to Avoid, copy() vs deepcopy
```python
original = {"a": 1, "b": [2, 3]}

# Incorrect copying – assignment creates alias, not copy
alias = original
alias["a"] = 99                                     # modifies original as well

# Shallow copy using .copy() method
shallow = original.copy()
shallow["a"] = 100                                   # original["a"] unchanged
shallow["b"].append(4)                               # original["b"] also modified because list is shared

# Why not copy directly? Because shallow copy does not recursively copy nested mutable objects.

# To avoid unintended sharing, use deepcopy from copy module
from copy import deepcopy
deep = deepcopy(original)
deep["b"].append(5)                                   # original["b"] unchanged

# Shallow copy is sufficient if all values are immutable
```

### 4.7 Iterating Over Dictionaries
```python
inventory = {"apples": 5, "bananas": 3, "oranges": 2}

# Iterate over keys (default)
for key in inventory:
    print(key, inventory[key])

# Explicit keys() iteration
for key in inventory.keys():
    print(key)

# Iterate over values
for value in inventory.values():
    print(value)

# Iterate over key-value pairs
for key, value in inventory.items():
    print(f"{key}: {value}")

# Modifying while iterating – avoid changing size, but updating values is safe
# Safe: changing values
for key in inventory:
    inventory[key] += 1

# Unsafe: adding or deleting keys – may raise RuntimeError or skip items
# for key in inventory:
#     if key == "apples":
#         del inventory[key]           # RuntimeError: dictionary changed size during iteration
# Correct approach: iterate over a copy of keys
for key in list(inventory.keys()):
    if key == "apples":
        del inventory[key]               # safe because list is a static copy
```

### 4.8 Nested Dictionaries
```python
# Dictionary containing dictionaries as values
users = {
    "alice": {"age": 30, "city": "Seattle"},
    "bob": {"age": 25, "city": "Austin"},
    "carol": {"age": 27, "city": "Boston"}
}

# Access nested element
alice_city = users["alice"]["city"]               # "Seattle"

# Iterate over nested dict
for username, info in users.items():
    print(f"User: {username}")
    for attr, value in info.items():
        print(f"  {attr}: {value}")

# Adding nested structure
users["dave"] = {"age": 32, "city": "Denver"}

# Safely accessing deeply nested keys – use get() with default
age = users.get("eve", {}).get("age", "unknown")   # "unknown"
```

### 4.9 Iterating Over Nested Dictionaries
```python
nested = {
    "section1": {"item1": 10, "item2": 20},
    "section2": {"item3": 30, "item4": 40}
}

# Flatten nested dict into key paths
for section, contents in nested.items():
    for item, value in contents.items():
        full_key = f"{section}.{item}"
        print(f"{full_key}: {value}")

# Using dictionary comprehension to transform nested structure
# Example: increment all values by 1
incremented = {
    section: {item: val + 1 for item, val in contents.items()}
    for section, contents in nested.items()
}
```

### 4.10 Dictionary Comprehension
```python
# Basic comprehension: square numbers as values
squares = {x: x**2 for x in range(5)}               # {0: 0, 1: 1, 2: 4, 3: 9, 4: 4}

# With condition: even numbers only
even_squares = {x: x**2 for x in range(10) if x % 2 == 0}  # {0: 0, 2: 4, 4: 16, 6: 36, 8: 64}

# Swap keys and values (if values are hashable)
original = {"a": 1, "b": 2, "c": 3}
swapped = {v: k for k, v in original.items()}        # {1: 'a', 2: 'b', 3: 'c'}

# From two lists using zip
keys = ["name", "age", "city"]
values = ["Frank", 40, "Chicago"]
person = {k: v for k, v in zip(keys, values)}        # {"name": "Frank", "age": 40, "city": "Chicago"}
```

### 4.11 Practical Example: Count Frequency of Numbers in List
```python
numbers = [1, 2, 3, 2, 4, 1, 2, 5, 3, 3]

# Method 1: using dict.get()
freq = {}
for n in numbers:
    freq[n] = freq.get(n, 0) + 1
# freq = {1: 2, 2: 3, 3: 3, 4: 1, 5: 1}

# Method 2: using collections.Counter (specialized for counting)
from collections import Counter
freq_counter = Counter(numbers)                       # Counter({2: 3, 3: 3, 1: 2, 4: 1, 5: 1})
# Counter is a dict subclass

# Method 3: using setdefault
freq2 = {}
for n in numbers:
    freq2.setdefault(n, 0)
    freq2[n] += 1
```

### 4.12 Practical Example: Merge Two Dicts in One
```python
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}

# Python 3.5+ unpacking – creates new dict
merged = {**dict1, **dict2}                           # {'a': 1, 'b': 2, 'c': 3, 'd': 4}
# If overlapping keys, later dict overwrites earlier

# Python 3.9+ merge operator (|)
merged = dict1 | dict2                                 # same as above

# Using update() to merge into existing dict (modifies original)
dict1.update(dict2)                                    # dict1 now merged

# Handling duplicate keys with custom logic
# Example: sum values for common keys
dict_a = {"x": 10, "y": 20}
dict_b = {"y": 5, "z": 7}
merged_custom = dict_a.copy()
for k, v in dict_b.items():
    merged_custom[k] = merged_custom.get(k, 0) + v     # {'x': 10, 'y': 25, 'z': 7}
```

### 4.13 Common Errors and Pitfalls
```python
# 1. KeyError when accessing missing key
d = {}
# value = d["missing"]                                 # KeyError

# 2. Using mutable objects as keys (unhashable)
# d[[1,2]] = "list"                                    # TypeError: unhashable type: 'list'
# Solution: use tuple if the sequence is immutable

# 3. Modifying dictionary while iterating (as shown earlier)

# 4. Assuming dictionaries maintain order in older Python (<3.7)
# Code relying on order should use collections.OrderedDict for compatibility.

# 5. Copying incorrectly (shallow vs deep) leading to unintended sharing
# Already covered.

# 6. Using default mutable values in setdefault – careful
d = {}
# This creates a new list each time? Actually setdefault returns the existing list or sets a new one.
# But if you use d.setdefault(k, []) multiple times with same k, you get the same list.
# However, if you mistakenly do: d.setdefault(k, []).append(x) works as expected.

# 7. Forgetting that keys must be hashable – but values can be anything.

# 8. Overriding built-in method names as keys: e.g., d["keys"] = ... then d.keys() is still available
# But d.keys() is a method, not the key. That's fine but can be confusing.

# 9. Using dict as a key in another dict – impossible, need to convert to something hashable.
```

## 5. Time and Space Complexity Analysis
| Operation               | Time Complexity (Average) | Notes                                      |
|-------------------------|----------------------------|--------------------------------------------|
| Get item `d[key]`       | O(1)                       | Hash and lookup; worst-case O(n) with collisions |
| Set item `d[key] = val` | O(1) amortized             | May trigger resize                         |
| Delete `del d[key]`     | O(1)                       | Tombstone marking                          |
| `in` (membership)       | O(1)                       |                                            |
| Iteration (`keys/values/items`) | O(n)                | Yields items in insertion order (Python 3.7+) |
| `copy()`                | O(n)                       | Shallow copy                               |
| `update()`              | O(len(update))             | Each insertion O(1) amortized               |
| `pop()`                 | O(1)                       |                                            |
| `popitem()`             | O(1)                       | Removes last inserted item                  |

Space complexity: O(n) for entries plus overhead for hash table (load factor typically < 2/3, so extra slots). Modern CPython uses combined table storing entries compactly, reducing overhead.

## 6. Edge Cases and Failure Scenarios
- **Empty dictionary**: Access with `d[key]` raises KeyError; `get()` returns None/default; iteration yields nothing; `popitem()` raises KeyError.
- **Key not found**: KeyError unless using safe methods.
- **Unhashable key**: TypeError at insertion.
- **Custom objects as keys**: Must implement `__hash__` and `__eq__`; if object mutable and hash changes after insertion, dictionary becomes corrupted (key may be unfindable).
- **Duplicate keys during construction**: Later occurrence overwrites earlier.
- **Resizing during iteration**: Modifying size while iterating raises `RuntimeError`; safe to modify values.
- **Large dictionaries**: Memory overhead significant; hash collisions may degrade performance if hash function poor.
- **Floating point keys**: May have precision issues; two floats that are mathematically equal may not compare equal (e.g., NaN).

## 7. Practical Use Cases
- **Lookup tables**: Mapping identifiers to values (e.g., HTTP status codes to messages).
- **Counting frequencies**: Tally occurrences (Counter).
- **Grouping data**: Mapping a key to a list of items (e.g., students per grade).
- **Memoization / caching**: Store results of expensive function calls.
- **Configuration settings**: Key-value pairs for application config.
- **JSON-like data structures**: Nested dictionaries for hierarchical data.
- **Graph representations**: Adjacency lists using dict mapping node to neighbors.

## 8. Limitations and Trade-offs
- **Key hashability**: Cannot directly use mutable types as keys; may need conversion (e.g., tuple of list).
- **Memory overhead**: Higher than list or tuple for small datasets.
- **Unordered (historical)**: Code relying on order needed OrderedDict; now insertion order is guaranteed (Python 3.7+), but order should not be relied upon for logical correctness (e.g., sorting required).
- **Performance degradation**: Poor hash functions (e.g., all keys colliding) can degrade to O(n) worst case.
- **Not thread-safe**: Concurrent modifications require external locks.
- **Resize cost**: Occasional O(n) rehash can cause latency spikes.
- **No slicing or indexing by position**: Must access by key only.