# All Data Types in Python

Python provides a rich set of built-in data types, each designed for specific tasks. Understanding their behaviors, quirks, and common errors is essential for writing robust code.

We’ll cover:

- **Numeric Types**: `int`, `float`, `complex`
- **Sequence Types**: `str`, `list`, `tuple`, `range`
- **Mapping Type**: `dict`
- **Set Types**: `set`, `frozenset`
- **Boolean Type**: `bool`
- **None Type**: `NoneType`
- **Binary Types**: `bytes`, `bytearray`, `memoryview`

For each, we’ll explore examples, cool features, tricky parts, weird behaviors, and common errors. Finally, we’ll discuss concatenating strings with integers/floats.

---

## 1. Numeric Types

### `int` – Integer
**Example**: `x = 42`

- **Cool behaviors**:
  - Arbitrary precision (unlike many languages, integers can be arbitrarily large, limited only by memory).
  - Supports bitwise operations (`&`, `|`, `<<`, `>>`).
- **Tricky parts**:
  - Division with `/` always returns a `float`; use `//` for integer division.
  - Large integers may consume significant memory and slow down operations.
- **Weird things**:
  - `True` and `False` are actually subclasses of `int`; `True == 1` and `False == 0` are true.
- **Common errors**:
  - `TypeError` when mixing with incompatible types (e.g., `int + str`).
  - `ValueError` when converting invalid strings: `int("abc")`.

### `float` – Floating‑point
**Example**: `pi = 3.14159`

- **Cool behaviors**:
  - Can represent very large or very small numbers using scientific notation (`1.2e-5`).
  - Supports `math` module functions like `isinf()`, `isnan()`.
- **Tricky parts**:
  - **Precision issues**: Floats are binary fractions; some decimal numbers cannot be represented exactly (e.g., `0.1 + 0.2 == 0.3` is `False`).
  - Never compare floats directly for equality; use a tolerance (`math.isclose`).
- **Weird things**:
  - `float('inf')` and `float('nan')` exist. `nan` is not equal to itself.
  - Dividing by zero yields `inf` (not an error) for floats, but `ZeroDivisionError` for integers.
- **Common errors**:
  - `ValueError` when converting non‑numeric strings: `float("hello")`.
  - Unexpected rounding in financial calculations (use `Decimal` from `decimal` module).

### `complex` – Complex Numbers
**Example**: `z = 3 + 4j`

- **Cool behaviors**:
  - Built‑in support for complex arithmetic; use `z.real` and `z.imag` to access parts.
  - `cmath` module provides functions for complex math.
- **Tricky parts**:
  - `j` (or `J`) is used instead of `i` for the imaginary unit.
- **Weird things**:
  - Comparing two complex numbers (with `<`, `>`) raises `TypeError` because they are not ordered.
  - Equality (`==`) works, but due to floating‑point precision, you might get surprises.
- **Common errors**:
  - `TypeError` when comparing with `<`/`>`.
  - Forgetting `j` suffix: `3+4` is just integer addition.

---

## 2. Sequence Types

### `str` – String
**Example**: `name = "Alice"`

- **Cool behaviors**:
  - Immutable, but supports many methods (`upper()`, `split()`, `replace()`, etc.).
  - String interpolation with f‑strings: `f"Hello, {name}"`.
  - Can be indexed and sliced: `name[1:4]` → `"lic"`.
- **Tricky parts**:
  - Strings are sequences of Unicode characters; indexing gives a string of length 1.
  - Repeating strings with `*`: `"Hi" * 3` → `"HiHiHi"`.
  - Comparisons are lexicographic based on Unicode code points, which may give unexpected results with accented characters.
- **Weird things**:
  - The empty string `""` is falsy, but `" "` (space) is truthy.
  - `"🍕" * 2` works (emojis are Unicode).
  - String interning: Python may reuse the same object for short strings, but don’t rely on it.
- **Common errors**:
  - `TypeError` when concatenating string with non‑string (see section on concatenation).
  - `IndexError` when accessing index out of range.
  - `ValueError` when using `int()` on a string that doesn’t represent an integer.

### `list` – List
**Example**: `fruits = ["apple", "banana", "cherry"]`

- **Cool behaviors**:
  - Mutable, dynamic: can grow/shrink with `append()`, `extend()`, `pop()`, etc.
  - Slicing returns a new list: `fruits[1:]` → `["banana", "cherry"]`.
  - List comprehensions: `[x**2 for x in range(5)]`.
- **Tricky parts**:
  - **Aliasing**: assigning a list to another variable creates a reference, not a copy. Changes affect both.
    ```python
    a = [1,2]
    b = a
    b.append(3)   # a also becomes [1,2,3]
    ```
    Use `copy()` or `deepcopy()` for independent copies.
  - **Mutable default arguments**: Defining a function with `def func(lst=[])` can lead to surprises because the default list is shared across calls.
- **Weird things**:
  - `list * n` repeats the list elements, but for nested lists, it repeats references, which can cause unexpected sharing.
    ```python
    x = [[0]] * 3   # [[0], [0], [0]]
    x[0][0] = 1     # [[1], [1], [1]] because all inner lists are the same object
    ```
  - Negative indices count from the end: `fruits[-1]` is `"cherry"`.
- **Common errors**:
  - `IndexError` when index out of range.
  - `AttributeError` if you mistakenly use string methods on a list.

### `tuple` – Tuple
**Example**: `point = (3, 4)`

- **Cool behaviors**:
  - Immutable, so they can be used as keys in dictionaries (if all elements are hashable).
  - Packing and unpacking: `a, b = (1, 2)`.
  - More memory‑efficient than lists for fixed collections.
- **Tricky parts**:
  - Single‑element tuple needs a trailing comma: `(5,)` not `(5)` (which is just the integer 5).
  - Immutability applies only to the tuple itself; if it contains mutable objects (like lists), those objects can still be changed.
- **Weird things**:
  - `tuple()` can convert any iterable to a tuple.
  - Comparing tuples works lexicographically.
- **Common errors**:
  - `TypeError` when trying to modify a tuple: `t[0] = 1`.
  - Forgetting the comma for a single‑element tuple leads to an integer, not a tuple.

### `range` – Range
**Example**: `r = range(5)` → `range(0,5)`

- **Cool behaviors**:
  - Generates numbers on demand (lazy), saving memory for large sequences.
  - Supports indexing, slicing, and membership tests efficiently.
  - Commonly used in `for` loops.
- **Tricky parts**:
  - In Python 2, `range` returned a list; in Python 3 it returns a range object.
  - `range(0, 10, 2)` gives even numbers.
  - Parameters must be integers; `range(0.5)` raises `TypeError`.
- **Weird things**:
  - `range(5, 0, -1)` counts down.
  - `len(range(1_000_000))` is fast because it’s computed arithmetically.
- **Common errors**:
  - `TypeError` if non‑integer arguments are given.
  - Assuming `range` returns a list; you need `list(range(5))` to get a list.

---

## 3. Mapping Type

### `dict` – Dictionary
**Example**: `person = {"name": "Alice", "age": 30}`

- **Cool behaviors**:
  - Keys must be hashable (immutable types like int, str, tuple). Values can be anything.
  - Dictionary comprehensions: `{x: x**2 for x in range(5)}`.
  - From Python 3.7+, insertion order is preserved.
- **Tricky parts**:
  - Accessing a missing key raises `KeyError`; use `.get(key, default)` to avoid.
  - Using mutable objects (like lists) as keys raises `TypeError`.
  - Merging dictionaries: `{**d1, **d2}` or `d1 | d2` (Python 3.9+).
- **Weird things**:
  - Equality comparison between dicts ignores order (before 3.7, order was arbitrary; now order matters for equality because it’s part of the data structure? Actually, equality still ignores order—it compares key-value pairs, but iteration order is guaranteed to be insertion order).
  - `bool({})` is `False`; any non‑empty dict is `True`.
- **Common errors**:
  - `KeyError` for missing keys.
  - `TypeError` for unhashable keys.
  - `RuntimeError` if you modify the dictionary while iterating over it (e.g., adding/removing keys). Use `list(d.items())` to iterate over a snapshot.

---

## 4. Set Types

### `set` – Set
**Example**: `s = {1, 2, 3}`

- **Cool behaviors**:
  - Unordered collection of unique, hashable elements.
  - Fast membership testing (`in`), mathematical set operations (`|`, `&`, `-`, `^`).
- **Tricky parts**:
  - Cannot contain mutable elements like lists or other sets.
  - `{}` creates an empty dictionary, not an empty set. Use `set()` for an empty set.
  - Sets are mutable, so they are not hashable and cannot be used as dictionary keys.
- **Weird things**:
  - The elements are stored in an order based on their hash; iteration order may appear arbitrary but is deterministic for a given Python run (though you shouldn’t rely on it).
- **Common errors**:
  - `TypeError` when adding an unhashable type.
  - Accidentally using `{}` expecting a set (you get a dict).

### `frozenset` – Frozenset
**Example**: `fs = frozenset([1, 2, 3])`

- **Cool behaviors**:
  - Immutable, hashable version of a set. Can be used as a dictionary key or element of another set.
- **Tricky parts**:
  - No methods that modify the set (like `add` or `remove`).
  - Created from an iterable, similar to `set`.
- **Weird things**:
  - Frozensets of the same elements are equal and have the same hash, so they can be used as keys.
- **Common errors**:
  - Trying to modify a frozenset (e.g., calling `add`) raises `AttributeError`.

---

## 5. Boolean Type

### `bool` – Boolean
**Example**: `flag = True`

- **Cool behaviors**:
  - `True` and `False` are the only two instances.
  - They behave like integers `1` and `0` in numeric contexts (`True + True` → `2`).
- **Tricky parts**:
  - Many values are considered “truthy” or “falsy” in Boolean contexts (e.g., `0`, `""`, `[]`, `None` are falsy; everything else is truthy).
  - When using `==` vs `is`: `True` and `1` are equal (`==`) but not the same object (`is`).
- **Weird things**:
  - `bool("False")` is `True` because the string is non‑empty.
  - `bool(0.0)` is `False`, but `bool(0.1)` is `True`.
- **Common errors**:
  - Misunderstanding truthiness: `if my_list:` is common and correct; `if my_list == True:` is almost always wrong.

---

## 6. None Type

### `NoneType` – None
**Example**: `result = None`

- **Cool behaviors**:
  - Represents the absence of a value, often used as a default return or placeholder.
  - Singleton: there is only one `None` object.
- **Tricky parts**:
  - Comparisons: `None` is falsy, but it’s better to check `if x is None:` rather than `if not x:` because `0`, `[]`, etc., are also falsy.
  - `None` is not equal to `False`, `0`, or empty collections (though all are falsy).
- **Weird things**:
  - `print()` returns `None` implicitly.
  - `None` cannot be used in arithmetic; `None + 1` raises `TypeError`.
- **Common errors**:
  - `AttributeError` when trying to call a method on `None`.
  - Forgetting that a function returns `None` and trying to use its result.

---

## 7. Binary Types

### `bytes` and `bytearray`
**Examples**: `b = b"hello"`, `ba = bytearray(b"hello")`

- **Cool behaviors**:
  - `bytes` is immutable, `bytearray` is mutable.
  - Used for handling binary data (e.g., file I/O, network communication).
  - Similar to strings but each element is an integer (0–255).
- **Tricky parts**:
  - Indexing a `bytes` object returns an integer, not a one‑byte `bytes` object.
    ```python
    b = b"hello"
    b[0]       # 104 (integer)
    b[0:1]     # b'h' (bytes slice)
    ```
  - Converting strings to bytes requires an encoding: `"hello".encode()`.
- **Weird things**:
  - `bytes` literals can contain ASCII characters; non‑ASCII must be escaped.
  - `bytearray` has methods similar to lists, like `append()`.
- **Common errors**:
  - `TypeError` when mixing `str` and `bytes` (e.g., concatenation).
  - `ValueError` when a byte value is out of range (0–255).

### `memoryview`
**Example**: `mv = memoryview(b"hello")`

- **Cool behaviors**:
  - Provides a view into another object’s memory without copying.
  - Useful for efficient slicing of large binary data.
- **Tricky parts**:
  - Can be created from objects that support the buffer protocol (e.g., `bytes`, `bytearray`).
  - Slicing a memoryview returns a new memoryview.
- **Weird things**:
  - Changing the original object may affect the memoryview (and vice versa for mutable ones).
- **Common errors**:
  - Using a memoryview after the original object is destroyed (reference issues).

---

## Concatenating Strings with Integers or Floats

Python does **not** automatically convert numbers to strings when using the `+` operator. Attempting to concatenate a string and a number raises a `TypeError`:

```python
"Age: " + 25   # TypeError: can only concatenate str (not "int") to str
```

### Why?
The `+` operator is overloaded for both numeric addition and string concatenation. Python avoids ambiguity by not implicitly converting types. This prevents subtle bugs where you might accidentally mix types.

### How to Fix

1. **Convert the number to a string explicitly** using `str()`:
   ```python
   "Age: " + str(25)   # "Age: 25"
   ```

2. **Use f‑strings** (Python 3.6+):
   ```python
   age = 25
   f"Age: {age}"       # "Age: 25"
   ```

3. **Use the `format()` method**:
   ```python
   "Age: {}".format(25)   # "Age: 25"
   ```

4. **Use the `%` formatting** (old style):
   ```python
   "Age: %d" % 25         # "Age: 25"
   ```

### With Floats
The same applies; you may also want to control formatting (e.g., rounding):
```python
price = 19.99
"Price: " + str(price)                 # "Price: 19.99"
f"Price: {price:.2f}"                   # "Price: 19.99"
"Price: {}".format(price)               # "Price: 19.99"
```

### Common Pitfalls
- Forgetting to convert when using `+` leads to `TypeError`.
- Using `+` with a mix of strings and numbers inside a `print()` call: `print("Value:", 42)` works because `print` accepts multiple arguments separated by commas; it does not concatenate them into one string first. But if you try `print("Value:" + 42)`, you’ll get an error.
- In Python, `*` with a string and an integer is allowed (repetition), but `+` is not. This can be confusing: `"Hi" * 3` works, but `"Hi" + 3` doesn’t.

Understanding these data types and their quirks will help you write cleaner, bug‑free Python code. Always test edge cases and be mindful of type conversions!