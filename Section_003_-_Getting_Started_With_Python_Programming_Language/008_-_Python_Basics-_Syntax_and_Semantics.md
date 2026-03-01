## Syntax vs. Semantics in Python

- **Syntax** refers to the set of rules that defines the structure of a valid program. It’s about *how* you write code — the correct arrangement of symbols, keywords, and punctuation. If you break syntax rules, you get a syntax error.
- **Semantics** refers to the meaning of the code — what happens when you run it. It deals with the logic, behaviour, and how data is manipulated. Even if code is syntactically correct, it can still have logical (semantic) errors.

---

## Comments

Comments are ignored by the Python interpreter and are used to explain code.

### Single‑line Comments
Start with `#` and continue until the end of the line.
```python
# This is a single-line comment
print("Hello, world!")  # This comment follows code
```

### Multi‑line Comments
Python doesn’t have a dedicated multi‑line comment syntax, but you can use triple‑quoted strings (which are ignored if not assigned) or multiple `#` lines.

**Using triple quotes** (commonly used as docstrings, but can act as comments):
```python
"""
This is a multi‑line comment
that spans several lines.
It is actually a string that is not assigned to any variable.
"""
```

**Using multiple `#` lines**:
```python
# This is also a multi‑line comment
# using several single‑line comments.
```

---

## Basic Python Rules

### 1. Case Sensitivity
Python is **case‑sensitive**. `variable`, `Variable`, and `VARIABLE` are three different names.
```python
name = "Alice"
Name = "Bob"
print(name)  # Output: Alice
print(Name)  # Output: Bob
```

### 2. Indentation
Indentation is **mandatory** in Python to define blocks of code (e.g., inside loops, conditionals, functions). Consistent indentation (usually 4 spaces) is required.
```python
if 5 > 2:
    print("Five is greater than two!")  # This line is indented
print("This line is outside the if block")  # No indentation
```
Incorrect indentation raises an `IndentationError`.

### 3. Line Continuation with Backslash
Long lines can be split across multiple lines using a backslash `\` at the end of the line.
```python
total = 1 + 2 + 3 \
        + 4 + 5
print(total)  # Output: 15
```
Alternatively, you can use parentheses `()`, brackets `[]`, or braces `{}` for implicit continuation without backslash.

### 4. Multiple Statements on the Same Line
Use a semicolon `;` to separate multiple statements on one line. This is generally discouraged for readability.
```python
x = 5; y = 10; print(x + y)  # Output: 15
```

---

## Understanding Semantics

### Variable Assignment
Variables are created when you first assign a value to them. Python uses **dynamic typing** – you don’t need to declare the type.
```python
message = "Hello"   # message is a string
count = 10          # count is an integer
pi = 3.14           # pi is a float
```
You can reassign a variable to a value of a different type later:
```python
count = "ten"       # now count is a string
```

### Type Inference
Python **infers** the type of a variable from the value assigned. You can check the type using `type()`:
```python
x = 42
print(type(x))      # <class 'int'>

x = 3.14
print(type(x))      # <class 'float'>
```

### NameError: Variable Not Defined
If you try to use a variable that has not been assigned a value, Python raises a `NameError`.
```python
print(age)  # NameError: name 'age' is not defined
```
To fix it, assign a value to the variable before using it.

---

Understanding these fundamentals will help you write correct and readable Python code. Practice each concept with small examples to get comfortable with the syntax and semantics.