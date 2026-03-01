# A Deep Dive into Python Conditional Statements

Conditional statements allow your program to make decisions and execute different code paths based on certain conditions. In Python, the primary conditional statements are `if`, `elif`, and `else`. Mastering them is essential for writing logic that responds to inputs, data, or program state.

We’ll explore every aspect – from basic syntax to the weirdest edge cases – with an excessive number of examples, common errors, best practices, and even some funny quirks.

---

## 1. The `if` Statement

The simplest conditional: execute a block of code **only if** a condition is true.

### Syntax
```python
if condition:
    # indented block
    # executed if condition is truthy
```

### Basic Examples
```python
age = 18
if age >= 18:
    print("You are an adult.")   # This line runs

temperature = 25
if temperature > 30:
    print("It's hot outside.")   # Won't run because 25 is not >30
print("Always printed.")          # Outside the if block
```

### Truthiness and `if`
The condition is evaluated for **truthiness** (see the logical operators section for details). Any non‑zero, non‑empty, non‑`None` value is considered true.

```python
if "hello":          # Non‑empty string → truthy
    print("True!")   # This runs

if 0:                # Zero → falsy
    print("Not printed")

if []:               # Empty list → falsy
    print("Not printed")

if [1,2]:            # Non‑empty list → truthy
    print("Printed")
```

### Indentation is Crucial
The block must be consistently indented (usually 4 spaces). Mismatched indentation causes `IndentationError`.

```python
if 5 > 2:
print("This is indented incorrectly")   # IndentationError
```

---

## 2. The `else` Clause

Provides an alternative block when the `if` condition is false.

### Syntax
```python
if condition:
    # block if condition true
else:
    # block if condition false
```

### Examples
```python
age = 16
if age >= 18:
    print("You can vote.")
else:
    print("Too young to vote.")   # This runs

# else is optional
password = "secret"
if password == "secret":
    print("Access granted")
# No else here – if wrong, nothing happens.
```

### `else` Always Executes if `if` is False
```python
x = 5
if x > 10:
    print("x is large")
else:
    print("x is not large")   # Runs because x>10 is false
```

---

## 3. The `elif` Clause (Else If)

When you need multiple mutually exclusive conditions, use `elif` (short for “else if”). You can have any number of `elif` blocks, and optionally an `else` at the end.

### Syntax
```python
if condition1:
    # block1
elif condition2:
    # block2
elif condition3:
    # block3
...
else:
    # default block
```

### Examples
```python
score = 85
if score >= 90:
    grade = 'A'
elif score >= 80:
    grade = 'B'      # This runs (85 >=80 but <90)
elif score >= 70:
    grade = 'C'
else:
    grade = 'F'
print(grade)   # B

# Important: conditions are evaluated in order, and the first true one runs.
# Subsequent elif/else blocks are skipped.
```

### Without `elif`, you'd need nested `if` inside `else`, which is messier:
```python
if score >= 90:
    grade = 'A'
else:
    if score >= 80:
        grade = 'B'
    else:
        if score >= 70:
            grade = 'C'
        else:
            grade = 'F'
```
`elif` is cleaner and more readable.

---

## 4. Nested Conditional Statements

You can place one conditional inside another. This is called nesting.

### Syntax
```python
if outer_condition:
    # outer block
    if inner_condition:
        # inner block
    else:
        # inner else
else:
    # outer else
```

### Examples
```python
username = "admin"
password = "secret"
if username == "admin":
    if password == "secret":
        print("Welcome, admin!")
    else:
        print("Wrong password.")
else:
    print("Unknown user.")
```

### Deep Nesting (and why to avoid it)
While Python allows arbitrary nesting, deep nesting makes code hard to read. Often you can flatten using `elif`, logical operators, or early returns.

```python
# Hard to read
x = 5
if x > 0:
    if x < 10:
        if x % 2 == 0:
            print("x is a positive even digit")
        else:
            print("x is positive odd digit")
    else:
        print("x >= 10")
else:
    print("x <= 0")

# Better: combine conditions
if 0 < x < 10 and x % 2 == 0:
    print("x is a positive even digit")
elif 0 < x < 10 and x % 2 != 0:
    print("x is positive odd digit")
elif x >= 10:
    print("x >= 10")
else:
    print("x <= 0")
```

---

## 5. Practical Basic Examples

Let's see many everyday scenarios.

### Example 1: Even or Odd
```python
num = int(input("Enter a number: "))
if num % 2 == 0:
    print("Even")
else:
    print("Odd")
```

### Example 2: Check if a string is empty
```python
text = input("Say something: ")
if text:
    print(f"You said: {text}")
else:
    print("You said nothing.")
```

### Example 3: Simple login system
```python
user = input("Username: ")
if user == "admin":
    pwd = input("Password: ")
    if pwd == "1234":
        print("Access granted")
    else:
        print("Wrong password")
else:
    print("Unknown user")
```

### Example 4: Temperature advisory
```python
temp = float(input("Current temperature (°C): "))
if temp > 30:
    print("It's hot! Stay hydrated.")
elif temp > 20:
    print("Nice weather.")
elif temp > 10:
    print("A bit chilly. Bring a jacket.")
else:
    print("Cold! Wear warm clothes.")
```

### Example 5: Leap year check
```python
year = int(input("Year: "))
if (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0):
    print("Leap year")
else:
    print("Not a leap year")
```

### Example 6: Discount calculator
```python
price = 100
is_member = True
if is_member:
    if price > 50:
        discount = 0.1
    else:
        discount = 0.05
else:
    discount = 0
final = price * (1 - discount)
print(f"Final price: ${final:.2f}")
```

### Example 7: Determine quadrant of a point
```python
x = float(input("x: "))
y = float(input("y: "))
if x > 0 and y > 0:
    print("Quadrant I")
elif x < 0 and y > 0:
    print("Quadrant II")
elif x < 0 and y < 0:
    print("Quadrant III")
elif x > 0 and y < 0:
    print("Quadrant IV")
else:
    print("On an axis")
```

---

## 6. Common Errors and Best Practices

### Error 1: Indentation Errors
```python
if 5 > 3:
print("Indented wrong")   # IndentationError
```
Always use consistent spaces (or tabs, but spaces are PEP8 recommended).

### Error 2: Using `=` instead of `==`
```python
x = 5
if x = 5:   # SyntaxError: invalid syntax. Assignment in condition is not allowed.
```
In Python, assignment is a statement, not an expression. If you accidentally write `if x = 5:`, you get a syntax error. (In some languages this is allowed and causes bugs; Python prevents it.)

### Error 3: Forgetting the colon `:`
```python
if 5 > 3   # SyntaxError: invalid syntax (missing colon)
    print("hello")
```
Always end the `if`, `elif`, `else` line with a colon.

### Error 4: Wrongly assuming `elif` is mandatory after `if`
You can have just `if` alone. `elif` and `else` are optional.

### Error 5: Over‑nesting when `elif` would be clearer
```python
# Bad
if condition1:
    # ...
else:
    if condition2:
        # ...
    else:
        if condition3:
            # ...
# Better: use elif
```

### Error 6: Not considering truthiness
```python
items = []
if items:          # This correctly checks emptiness
    print(items.pop())
# But beginners might write:
if len(items) > 0:  # Works but more verbose
```

### Error 7: Comparing with `True`/`False` explicitly
```python
flag = True
if flag == True:   # Redundant – just use `if flag:`
    ...
if flag is True:   # If you need identity (rare)
```
Similarly, `if flag is False:` is usually written `if not flag:`.

### Error 8: Using `== None` instead of `is None`
```python
x = None
if x == None:   # Works but not idiomatic; better: `if x is None:`
```
`is None` is preferred because `None` is a singleton.

### Error 9: Chaining comparisons incorrectly
```python
x = 5
if 0 < x < 10:   # Correct Pythonic way
    ...
# Some might write:
if 0 < x and x < 10:   # Also correct, but longer
```

### Error 10: Side effects in conditions
Because logical operators short‑circuit, functions with side effects may not run.
```python
def risky():
    print("risky called")
    return True

if False and risky():   # risky() never called
    pass
```
This is usually a feature, but be aware if you rely on the side effect.

### Best Practice 1: Keep conditions simple
If a condition becomes complex, extract it into a named variable or function.
```python
# Instead of:
if (user.is_active and user.has_credit and not user.blocked) or user.is_admin:
    ...

# Better:
can_proceed = (user.is_active and user.has_credit and not user.blocked) or user.is_admin
if can_proceed:
    ...
```

### Best Practice 2: Use `in` for membership
```python
if fruit == "apple" or fruit == "banana" or fruit == "cherry":
    ...
# Better:
if fruit in ["apple", "banana", "cherry"]:
    ...
```

### Best Practice 3: Avoid deep nesting with early returns
In functions, use early returns to flatten.
```python
def process(user):
    if not user:
        return "No user"
    if not user.is_active:
        return "User inactive"
    if user.age < 18:
        return "Underage"
    # All checks passed
    return "Welcome"
```

### Best Practice 4: Use the ternary operator for simple assignments
```python
# Instead of:
if age >= 18:
    status = "adult"
else:
    status = "minor"
# Use:
status = "adult" if age >= 18 else "minor"
```

### Best Practice 5: Parentheses for clarity
Even though Python has operator precedence, use parentheses to make complex conditions readable.
```python
if (score > 0 and score <= 100) or (score == -1):
    pass
```

---

## 7. Tricky, Funny, and Weird Parts

### 7.1. `if 0`, `if None`, etc.
```python
if 0:
    print("Never prints")   # 0 is falsy
if None:
    print("Never prints")   # None is falsy
if []:
    print("Never prints")   # empty list falsy
if [0]:
    print("Prints")         # non‑empty list truthy (even if it contains zero)
```

### 7.2. `if` with empty collections
```python
my_list = []
if my_list:          # False – empty
    print("won't run")
my_list = [0]
if my_list:          # True – because list is not empty, even though first element is falsy
    print("runs")
```

### 7.3. Using logical operators inside conditions
```python
x = 5
if x > 0 and x % 2 == 0:
    print("positive even")   # False because x%2 !=0
# The 'and' short‑circuits: if first part is false, second is never evaluated.
```

### 7.4. `if` with `or` returning non‑bool
Because `or` returns the first truthy operand, you can do:
```python
name = input("Name: ") or "Anonymous"
# If input returns empty string (falsy), name becomes "Anonymous"
```

But be careful: if input could be `0` (which is falsy) but valid, this would wrongly assign default.

### 7.5. `else` after loops? (Not conditional, but interesting)
Python allows `else` after `for` and `while` loops. It runs if the loop completed normally (no `break`). That's a separate topic.

### 7.6. `if` with function calls that have side effects
```python
def get_value():
    print("Computing...")
    return 42

if get_value() > 40:   # "Computing..." printed once
    print("Big")
```
The function is called once; its result is used in the condition.

### 7.7. Comparing floats
```python
if 0.1 + 0.2 == 0.3:   # False due to precision
    print("Math is broken?")
# Use math.isclose
import math
if math.isclose(0.1+0.2, 0.3):
    print("Close enough")
```

### 7.8. `if` with `is` vs `==`
```python
a = [1,2]
b = [1,2]
if a == b:    # True (same value)
if a is b:    # False (different objects)

# For small integers, interning may cause unexpected `is` results:
x = 256
y = 256
if x is y:    # True (CPython caches -5 to 256)
    print("Same object")
x = 257
y = 257
if x is y:    # False (not interned)
```

### 7.9. `if` with `in` and dictionary keys
```python
d = {'a': 1, 'b': 2}
if 'a' in d:    # Checks keys, not values
    print("Key exists")
if 1 in d:      # False (1 is not a key)
    pass
```

### 7.10. Conditional expression (ternary) can be nested
```python
result = "A" if score >= 90 else "B" if score >= 80 else "C"
# This is equivalent to:
if score >= 90:
    result = "A"
elif score >= 80:
    result = "B"
else:
    result = "C"
```
But nesting ternaries can become unreadable – use sparingly.

### 7.11. `if` with `not` and double negatives
```python
flag = True
if not not flag:   # same as if flag: – but confusing
    print("Double negative")
```

### 7.12. The `...` (ellipsis) in conditionals? Not really, but you can put anything truthy:
```python
if ...:          # Ellipsis is truthy (in Python 3)
    print("Ellipsis is truthy")
# Output: Ellipsis is truthy
```

---

## 8. Summary

- `if`, `elif`, `else` form the core of conditional logic.
- Indentation defines blocks; be consistent.
- Conditions are evaluated for truthiness.
- `elif` chains multiple conditions cleanly.
- Nesting is allowed but can reduce readability; use `elif`, logical operators, or early returns to flatten.
- Watch out for common errors like missing colons, assignment vs equality, and indentation.
- Use best practices: keep conditions simple, use `in` for membership, prefer `is None`, and leverage ternary for simple assignments.
- Be aware of weirdness: short‑circuiting, truthiness of collections, floating‑point comparisons, and interning.

