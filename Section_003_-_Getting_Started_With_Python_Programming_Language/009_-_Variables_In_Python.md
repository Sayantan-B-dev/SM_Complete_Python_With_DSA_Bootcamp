## Variables and Identifiers in Python

### What is a Variable?
A **variable** is a name that refers to a value stored in memory. It acts like a label for a data object. In Python, variables are created when you first assign a value to them.

### What is an Identifier?
An **identifier** is the name given to a variable, function, class, module, etc. It is used to identify the entity in the code. Rules for identifiers:
- Must start with a letter (a–z, A–Z) or an underscore `_`.
- Subsequent characters can be letters, digits (0–9), or underscores.
- Case‑sensitive (`age`, `Age`, and `AGE` are different).
- Cannot be a Python keyword (e.g., `if`, `for`, `while`, `def`, etc.).

---

## Variables in Python and Memory

In Python, variables are **references** (or pointers) to objects in memory. When you assign a value:
1. An object is created in memory (e.g., an integer, string, list).
2. The variable is bound to that object.
3. Multiple variables can reference the same object (aliasing).

Example:
```python
a = 10       # integer object 10 is created, 'a' references it
b = a        # 'b' now references the same integer object
a = 20       # a new integer object 20 is created, 'a' now references it; 'b' still references 10
```
Memory management is handled by Python’s **garbage collector**, which frees memory when objects are no longer referenced.

---

## Types of Variable Assignment

Python offers flexible ways to assign values:

### 1. Simple Assignment
```python
x = 5
name = "Alice"
```

### 2. Multiple Assignment
Assign several variables at once:
```python
a, b, c = 1, 2, 3          # a=1, b=2, c=3
x = y = z = 0               # all three refer to the same integer 0
```

### 3. Augmented Assignment
Combine an operation with assignment:
```python
x = 5
x += 3          # equivalent to x = x + 3 → x becomes 8
x *= 2          # x = x * 2 → 16
```

### 4. Unpacking Assignment
Assign values from iterables:
```python
numbers = [10, 20, 30]
p, q, r = numbers        # p=10, q=20, r=30
```

---

## Naming Conventions (PEP 8)

- **Be descriptive**: Use names that explain the purpose.
  ```python
  # Good
  student_age = 18
  total_price = 99.99

  # Bad
  a = 18
  tp = 99.99
  ```
- **Start with a letter or underscore**:
  ```python
  _hidden = "private"      # valid (convention for "internal use")
  2nd_place = "John"       # INVALID – starts with digit
  ```
- **Case‑sensitive**: `myVar` and `myvar` are different.
- **Use lowercase with underscores** for variable/function names (snake_case).
- **Use CamelCase** for class names (e.g., `StudentInfo`).
- **Avoid Python keywords**:
  ```python
  class = "Math"           # INVALID – 'class' is a keyword
  ```

### Valid vs. Invalid Examples

| Valid Identifiers      | Invalid Identifiers        |
|------------------------|----------------------------|
| `age`                  | `2age` (starts with digit) |
| `_count`               | `my-var` (hyphen not allowed) |
| `total_sum`            | `if` (keyword)             |
| `userName` (but not PEP8) | `@user` (`@` not allowed) |
| `camelCase` (allowed)  | `variable name` (space)    |

---

## Type Checking and Type Conversion

### Type Checking
Use `type()` or `isinstance()` to check the type of a variable:
```python
x = 10
print(type(x))               # <class 'int'>
print(isinstance(x, int))    # True
```

### Type Conversion (Type Casting)
Python provides built‑in functions to convert between types. Conversions can be **explicit** (you call the function) or **implicit** (automatic, e.g., `int + float` gives `float`).

#### Table of Explicit Type Conversions

| Target Type | Function   | Converts From                         | Example (from string)     | Notes                                       |
|-------------|------------|---------------------------------------|----------------------------|---------------------------------------------|
| **int**     | `int()`    | `float`, `str` (numeric), `bool`      | `int("42")` → `42`         | `int(3.9)` → `3` (truncates)               |
| **float**   | `float()`  | `int`, `str` (numeric), `bool`         | `float("3.14")` → `3.14`   | `float(5)` → `5.0`                          |
| **str**     | `str()`    | Any type (calls `__str__`)             | `str(100)` → `"100"`       | `str(True)` → `"True"`                      |
| **bool**    | `bool()`   | Any type (truthiness)                  | `bool(0)` → `False`        | `bool("hello")` → `True`                    |
| **list**    | `list()`   | Iterable (tuple, set, dict, string)    | `list("abc")` → `['a','b','c']` | Keys only if converting a dict         |
| **tuple**   | `tuple()`  | Iterable                                | `tuple([1,2])` → `(1,2)`   |                                             |
| **set**     | `set()`    | Iterable                                | `set([1,2,2])` → `{1,2}`   | Duplicates removed                          |
| **dict**    | `dict()`   | List of tuples, keyword args, etc.      | `dict([('a',1),('b',2)])` → `{'a':1,'b':2}` | For mapping types                     |
| **complex** | `complex()`| `int`, `float`, `str`                   | `complex("1+2j")` → `(1+2j)` |                                             |

**Implicit conversion** happens automatically when mixing types:
```python
result = 5 + 2.5      # int + float → float (7.5)
```

---

## Dynamic Typing

Python is **dynamically typed** – the type of a variable is determined at runtime and can change as the program executes. You can assign different types to the same variable over its lifetime.

### Example: Same Variable, Multiple Types
```python
value = 10
print(value, type(value))      # 10 <class 'int'>

value = 3.14
print(value, type(value))      # 3.14 <class 'float'>

value = "Hello"
print(value, type(value))      # Hello <class 'str'>

value = [1, 2, 3]
print(value, type(value))      # [1, 2, 3] <class 'list'>
```
This flexibility is powerful but requires careful handling to avoid runtime errors.

---

## The `input()` Method

The `input()` function reads a line from the user (as a string). It optionally takes a prompt string.

### Basic Usage
```python
name = input("Enter your name: ")
print("Hello,", name)
```

### Converting Input
`input()` always returns a string, so you often need to convert it:
```python
age = input("Enter your age: ")        # returns string, e.g., "25"
age = int(age)                          # now an integer

# Or in one step:
height = float(input("Enter height in meters: "))
```
If conversion fails (e.g., user enters `"abc"` for age), a `ValueError` occurs. Use error handling (`try...except`) to manage invalid input.

---

Understanding these fundamentals will give you a solid foundation for writing correct and flexible Python code. Practice each concept to become comfortable with how variables, types, and input work together.