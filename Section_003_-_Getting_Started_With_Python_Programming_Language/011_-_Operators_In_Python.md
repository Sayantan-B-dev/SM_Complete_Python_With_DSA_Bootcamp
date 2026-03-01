# A Deep Dive into Python Operators

Operators are special symbols that perform operations on variables and values. Python provides a rich set of operators, and understanding their nuances will save you from countless bugs and help you write more expressive code. We’ll explore **arithmetic**, **comparison**, and **logical** operators with an excessive number of examples, covering every weird, tricky, and funny corner case.

---

## 1. Arithmetic Operators

These are used to perform mathematical operations.

| Operator | Name           | Example        |
|----------|----------------|----------------|
| `+`      | Addition       | `3 + 5` → `8`  |
| `-`      | Subtraction    | `7 - 2` → `5`  |
| `*`      | Multiplication | `4 * 3` → `12` |
| `/`      | Division       | `8 / 2` → `4.0`|
| `//`     | Floor Division | `7 // 3` → `2` |
| `%`      | Modulo         | `7 % 3` → `1`  |
| `**`     | Exponentiation | `2 ** 3` → `8` |

### Basic Examples with Numbers

```python
# Integers
print(10 + 3)      # 13
print(10 - 3)      # 7
print(10 * 3)      # 30
print(10 / 3)      # 3.3333333333333335  (always returns float)
print(10 // 3)     # 3  (floor division)
print(10 % 3)      # 1
print(10 ** 3)     # 1000

# Floats
print(5.5 + 2.2)   # 7.7
print(5.5 - 2.2)   # 3.3
print(5.5 * 2)     # 11.0
print(5.5 / 2)     # 2.75
print(5.5 // 2)    # 2.0  (floor division with float returns float)
print(5.5 % 2)     # 1.5
print(5.5 ** 2)    # 30.25

# Complex numbers
c1 = 3 + 4j
c2 = 1 - 2j
print(c1 + c2)     # (4+2j)
print(c1 * c2)     # (11-2j)
# Floor division and modulo are not defined for complex numbers!
# print(c1 // c2)  # TypeError: can't take floor of complex number.
```

### Mixing Types

Python automatically converts narrower types to wider ones: `bool` → `int` → `float` → `complex`.

```python
print(True + 2)        # 3   (True behaves as 1)
print(False * 10)      # 0   (False behaves as 0)
print(3 + 4.5)         # 7.5 (int becomes float)
print(3 + 4.5j)        # (3+4.5j) (int becomes complex)
```

### Tricky Behavior: Division Always Returns Float

Even if the result is a whole number, `/` returns a float:

```python
print(4 / 2)           # 2.0, not 2
```

### Floor Division (`//`) and Modulo (`%`) with Negative Numbers

This is where things get interesting. The rule: **`a = (a // b) * b + (a % b)`** always holds, and the remainder has the same sign as the divisor (Python’s choice).

```python
# Positive divisor
print(7 // 3)      # 2
print(7 % 3)       # 1
print(7 // -3)     # -3   (because floor division goes toward negative infinity)
print(7 % -3)      # -2   (since 7 = (-3)*(-3) + (-2))

# Negative dividend
print(-7 // 3)     # -3
print(-7 % 3)      # 2    (-7 = 3*(-3) + 2)
print(-7 // -3)    # 2
print(-7 % -3)     # -1   (-7 = (-3)*2 + (-1))

# With floats
print(7.5 // 2.5)  # 3.0
print(7.5 % 2.5)   # 0.0
print(-7.5 // 2.5) # -4.0
print(-7.5 % 2.5)  # 2.5
```

**Weird but consistent**: Modulo result always has the same sign as the divisor (or zero). Many languages differ, so be careful when porting code.

### Exponentiation (`**`) – More Than Just Powers

```python
print(2 ** 3)       # 8
print(2 ** -3)      # 0.125  (negative exponent gives float)
print(4 ** 0.5)     # 2.0  (square root)
print(16 ** 0.25)   # 2.0  (fourth root)
print(2 ** 2 ** 3)  # 256  because exponentiation is right-associative: 2**(2**3)
```

### Operators on Strings and Sequences

The `+` and `*` operators are also defined for sequences (strings, lists, tuples).

```python
# String concatenation
print("Hello, " + "world!")   # "Hello, world!"
# String repetition
print("Ha" * 3)               # "HaHaHa"

# List concatenation
print([1, 2] + [3, 4])        # [1, 2, 3, 4]
# List repetition
print([0] * 5)                 # [0, 0, 0, 0, 0]

# Tuple concatenation
print((1, 2) + (3, 4))        # (1, 2, 3, 4)

# But no subtraction or division for sequences!
# print("Hello" - "H")         # TypeError
```

### Funny/Weird Cases

- **`True` and `False` are just fancy 1 and 0**:

  ```python
  print(True + True)       # 2
  print(False ** 0)        # 1 (0**0? Actually False**0 -> 0**0 -> 1 in Python)
  # Wait, 0**0 is defined as 1 in Python.
  print(0 ** 0)            # 1
  ```

- **`inf` and `nan`** (float infinity and not-a-number):

  ```python
  inf = float('inf')
  nan = float('nan')
  print(inf + 1)           # inf
  print(inf * 0)           # nan
  print(nan == nan)        # False  (NaN is not equal to itself)
  print(nan is nan)        # True   (but identity holds)
  ```

- **Division by zero**:

  ```python
  # 1 / 0                  # ZeroDivisionError: division by zero
  # 1 // 0                 # ZeroDivisionError
  # 1 % 0                  # ZeroDivisionError
  print(1 / 0.0)           # inf   (float division by zero gives inf, not error!)
  print(0 / 0.0)           # nan
  ```

---

## 2. Comparison Operators

These compare values and return `True` or `False`.

| Operator | Meaning                  |
|----------|--------------------------|
| `==`     | equal                    |
| `!=`     | not equal                |
| `>`      | greater than             |
| `<`      | less than                |
| `>=`     | greater than or equal    |
| `<=`     | less than or equal       |

### Comparing Numbers

```python
print(5 == 5)        # True
print(5 != 3)        # True
print(5 > 3)         # True
print(5 < 3)         # False
print(5 >= 5)        # True
print(5 <= 4)        # False

# Mixing numeric types works fine
print(5 == 5.0)      # True  (values are equal, types are converted)
print(5 == 5.1)      # False
print(5 < 5.1)       # True
print(1 == True)     # True  (True == 1)
print(0 == False)    # True
print(1 == False)    # False
```

### Comparing Strings (Lexicographic Order)

Strings are compared lexicographically based on Unicode code points.

```python
print("apple" < "banana")     # True  ('a' < 'b')
print("Apple" < "apple")      # True  (because 'A' (65) < 'a' (97))
print("10" < "2")             # True? Wait: '1' < '2', so "10" < "2" is True! 
# This is a common pitfall – string comparison is character by character, not numeric.
# To compare as numbers, convert first: int("10") < int("2") -> False.
```

### Comparing Different Types

In Python 3, comparing different **incompatible** types raises a `TypeError` (unlike Python 2, which would give arbitrary results).

```python
# print(5 < "hello")    # TypeError: '<' not supported between instances of 'int' and 'str'
# print([1,2] > (1,2))  # TypeError: '>' not supported between tuple and list
```

But numeric types (int, float, complex) are compatible in the sense that you can compare int with float, but complex numbers can’t be ordered with `<`, `>`, `<=`, `>=`:

```python
print(5 < 5.5)               # True
# print(3+4j < 5+2j)         # TypeError: '<' not supported between instances of 'complex' and 'complex'
# Equality works:
print(3+4j == 3+4j)          # True
```

### Chaining Comparisons

Python allows chaining like `a < b < c`, which is equivalent to `a < b and b < c` (but `b` is evaluated only once).

```python
x = 5
print(1 < x < 10)            # True
print(1 < x < 5)             # False (5 is not less than 5)
print(1 < x <= 5)            # True

# Works with any expressions
print(2 < 3 < 4 < 5)         # True
print(2 < 3 > 2)             # True  (2 < 3 and 3 > 2)
print(2 < 3 and 3 > 2)       # same
```

### Identity vs Equality

`is` and `is not` check if two references point to the same object (identity), not if they have the same value.

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a
print(a == b)        # True (same values)
print(a is b)        # False (different objects)
print(a is c)        # True (same object)

# For small integers, Python may reuse objects (interning)
x = 256
y = 256
print(x is y)        # True (usually, but don't rely on it!)
x = 257
y = 257
print(x is y)        # False (outside small integer cache)

# Strings can also be interned, but not always.
```

**Best practice**: Use `==` for value equality, `is` for `None` comparisons (`x is None`).

### Comparing Floating Point Numbers

Because of floating-point precision, direct equality often fails:

```python
print(0.1 + 0.2 == 0.3)      # False! (0.1+0.2 = 0.30000000000000004)
# Use math.isclose for tolerance
import math
print(math.isclose(0.1+0.2, 0.3))   # True
```

### Weird Comparisons

- **`nan` is not equal to anything, including itself**:

  ```python
  nan = float('nan')
  print(nan == nan)          # False
  print(nan != nan)          # True
  ```

- **`None` compared with anything (except None) is False**:

  ```python
  print(None == 0)           # False
  print(None == False)       # False
  print(None == None)        # True
  ```

- **Comparisons with complex numbers only allowed for `==` and `!=`**:

  ```python
  print(3+4j == 3+4j)        # True
  print(3+4j != 4+3j)        # True
  # print(3+4j > 2+5j)       # TypeError
  ```

---

## 3. Logical Operators

Logical operators are used to combine conditional statements. They operate on boolean values, but Python’s logical operators are more flexible: they return **the actual operand value** (not necessarily `True`/`False`) based on truthiness.

| Operator | Description |
|----------|-------------|
| `and`    | Returns the first falsy operand, or the last truthy operand. |
| `or`     | Returns the first truthy operand, or the last falsy operand. |
| `not`    | Returns `True` if the operand is falsy, `False` otherwise (always bool). |

### Truthiness

In Python, the following are considered **falsy**:
- `None`
- `False`
- Zero of any numeric type: `0`, `0.0`, `0j`
- Empty sequences/collections: `''`, `[]`, `()`, `{}`, `set()`, `range(0)`
- Everything else is **truthy**.

```python
# Examples of truthiness
print(bool(0))           # False
print(bool(10))          # True
print(bool(""))          # False
print(bool("hello"))     # True
print(bool([]))          # False
print(bool([1,2]))       # True
```

### `and` Operator

`x and y` evaluates `x`; if `x` is falsy, returns `x`; otherwise evaluates `y` and returns it.

```python
print(0 and 42)              # 0
print(10 and 20)             # 20
print(10 and 0)              # 0
print("" and "something")    # ''
print("Hi" and "Bye")        # 'Bye'
print([] and [1,2])          # []
print([1] and [2,3])         # [2,3]
print(True and False)        # False
print(False and True)        # False
```

### `or` Operator

`x or y` evaluates `x`; if `x` is truthy, returns `x`; otherwise evaluates `y` and returns it.

```python
print(0 or 42)               # 42
print(10 or 20)              # 10
print(10 or 0)               # 10
print("" or "default")       # 'default'
print("Hi" or "Bye")         # 'Hi'
print([] or [1,2])           # [1,2]
print([1] or [2,3])          # [1]
print(True or False)         # True
print(False or True)         # True
```

### `not` Operator

`not x` returns `True` if `x` is falsy, else `False`. It always returns a boolean.

```python
print(not 0)                 # True
print(not 42)                # False
print(not "")                # True
print(not "hello")           # False
print(not [])                # True
print(not [1,2])             # False
print(not None)              # True
print(not not 10)            # True (double negative)
```

### Short‑Circuit Evaluation

Both `and` and `or` short‑circuit: they stop evaluating as soon as the result is determined.

```python
def expensive():
    print("expensive called")
    return True

# In an 'and', if left is falsy, right is never evaluated
result = False and expensive()   # expensive() not called, result is False
print(result)

# In an 'or', if left is truthy, right is never evaluated
result = True or expensive()     # expensive() not called, result is True
print(result)
```

This is useful for guarding conditions:

```python
name = input("Enter name: ") or "Anonymous"   # if name is empty string, use default
print(f"Hello, {name}")
```

But beware of side effects! If the right-hand expression has side effects (like modifying a variable), short‑circuiting may skip them.

### Common Pitfalls

1. **Using `and`/`or` instead of `&`/`|` for bitwise operations**:
   ```python
   # For sets, & and | are intersection and union
   s1 = {1,2}; s2 = {2,3}
   print(s1 and s2)        # {2,3} (because s1 is truthy, returns s2)
   print(s1 & s2)          # {2}   (intersection)
   ```

2. **Chaining logical operators** can be confusing without parentheses:
   ```python
   # Precedence: not > and > or
   print(True or False and False)   # True  (because False and False is False, then True or False is True)
   print((True or False) and False) # False
   ```

3. **Return values are not necessarily boolean**:
   ```python
   result = 0 or 5           # result is 5 (int)
   if result:                # works because 5 is truthy
       print("ok")
   ```

### Funny Examples

- **Using `or` to provide default values** (common idiom):

  ```python
  def greet(name=None):
      name = name or "Guest"
      print(f"Hello, {name}")
  greet("Alice")   # Hello, Alice
  greet()          # Hello, Guest
  # But if name could be 0 or empty string, this fails (those are falsy).
  # Better: name = name if name is not None else "Guest"
  ```

- **`and` as a ternary-like expression** (not recommended for readability):

  ```python
  x = 5
  result = x > 0 and "positive" or "non-positive"   # Works but risky; use conditional expression.
  print(result)   # positive
  ```

- **Short‑circuiting can avoid errors**:

  ```python
  items = []
  # items.pop() would raise IndexError if items is empty.
  if items and items.pop() == 42:
      print("Found 42")
  # The 'and' short‑circuits, so items.pop() is never called when items is empty.
  ```

---

## Operator Precedence

When multiple operators appear in an expression, precedence determines the order of evaluation. Here’s a simplified table (highest to lowest):

| Precedence | Operators                         |
|------------|-----------------------------------|
| 1          | `**` (exponentiation)             |
| 2          | `+x`, `-x`, `~x` (unary)          |
| 3          | `*`, `/`, `//`, `%`                |
| 4          | `+`, `-` (binary)                  |
| 5          | `<<`, `>>` (shifts)                |
| 6          | `&` (bitwise AND)                  |
| 7          | `^` (bitwise XOR)                  |
| 8          | `|` (bitwise OR)                   |
| 9          | comparisons (`in`, `is`, `<`, `<=`, `>`, `>=`, `!=`, `==`) |
| 10         | `not`                              |
| 11         | `and`                              |
| 12         | `or`                               |

Parentheses can override precedence.

```python
print(2 + 3 * 4)          # 14 (3*4 first)
print((2 + 3) * 4)        # 20
print(2 ** 3 ** 2)        # 512 (right-associative: 2**(3**2) = 2**9)
print(not True and False) # False (not True is False, then False and False is False)
```

---

## Summary

- **Arithmetic operators** work with numbers, but also have special meanings for sequences (`+`, `*`). Watch out for floor division/modulo with negatives and float precision issues.
- **Comparison operators** compare values, returning `True`/`False`. Different types generally can’t be compared (except numeric types). String comparison is lexicographic, not numeric. Chaining is concise and powerful.
- **Logical operators** evaluate truthiness, short‑circuit, and return actual operand values. They are useful for conditional assignment and guarding.

Master these operators, and you’ll write Python that is both expressive and bug‑free. Remember: when in doubt, add parentheses or print intermediate results!