# Table of Contents

1. [Function Copy](#function-copy)  
   1.1 Definition and Concept Overview  
   1.2 Core Principles and Internal Mechanics  
   1.3 Step-by-Step Logical Breakdown  
   1.4 Implementation (Python)  
   1.5 Time and Space Complexity Analysis  
   1.6 Edge Cases and Failure Scenarios  
   1.7 Practical Use Cases  
   1.8 Limitations and Trade-offs  

2. [Closures](#closures)  
   2.1 Definition and Concept Overview  
   2.2 Core Principles and Internal Mechanics  
   2.3 Step-by-Step Logical Breakdown  
   2.4 Implementation (Python)  
   2.5 Time and Space Complexity Analysis  
   2.6 Edge Cases and Failure Scenarios  
   2.7 Practical Use Cases  
   2.8 Limitations and Trade-offs  

3. [Decorators](#decorators)  
   3.1 Definition and Concept Overview  
   3.2 Core Principles and Internal Mechanics  
   3.3 Step-by-Step Logical Breakdown  
   3.4 Implementation (Python)  
   3.5 Time and Space Complexity Analysis  
   3.6 Edge Cases and Failure Scenarios  
   3.7 Practical Use Cases  
   3.8 Limitations and Trade-offs  

4. [Decorators with Arguments](#decorators-with-arguments)  
   4.1 Definition and Concept Overview  
   4.2 Core Principles and Internal Mechanics  
   4.3 Step-by-Step Logical Breakdown  
   4.4 Implementation (Python) – Extreme Line-by-Line Explanation  
   4.5 Time and Space Complexity Analysis  
   4.6 Edge Cases and Failure Scenarios  
   4.7 Practical Use Cases  
   4.8 Limitations and Trade-offs  

5. [Comparison: JavaScript Callbacks vs Python Decorators](#comparison-javascript-callbacks-vs-python-decorators)  

---

## Function Copy

### 1.1 Definition and Concept Overview

In Python, functions are first‑class objects. Assigning a function to another variable does **not** create a copy of the function; it creates an additional reference to the same function object. The term “function copy” in this context refers to the behavior of duplicating a reference, not a deep copy of the function’s code or closure.

### 1.2 Core Principles and Internal Mechanics

- Functions are objects of type `function`.  
- Assignment (`wel = welcome`) binds the name `wel` to the same object that `welcome` references.  
- The function object itself resides on the heap, and its reference count increases with each new alias.  
- Deleting one name (`del welcome`) removes that reference; the function object remains accessible as long as other references exist.

### 1.3 Step-by-Step Logical Breakdown

1. Define a function `welcome`.  
2. Assign `wel = welcome` – both names point to the identical function object.  
3. Delete the original name `welcome`. The function object persists because `wel` still references it.  
4. Invoking `wel()` executes the original function.

### 1.4 Implementation (Python)

```python
def welcome() -> str:
    """Return a welcome message."""
    return "Welcome to the advanced python course"

# Assign the function object to another name
wel = welcome          # wel now references the same function object

# Delete the original reference
del welcome

# Call through the new reference
print(wel())           # Output: Welcome to the advanced python course
```

### 1.5 Time and Space Complexity Analysis

- **Time**: O(1) – assignment and deletion are constant‑time operations.  
- **Space**: No new function object is created; only the reference count changes. Memory consumption remains unchanged.

### 1.6 Edge Cases and Failure Scenarios

- Attempting to copy a function with a closure: the closure is part of the function object; assignment copies the reference to the same closure, not a new one.  
- Deleting the original name is safe as long as at least one reference exists; otherwise, the function becomes eligible for garbage collection.

### 1.7 Practical Use Cases

- Aliasing functions for readability or conditional dispatch.  
- Passing functions as arguments (callbacks) without altering the original.  
- Implementing decorators, where the decorator returns a wrapper that may replace the original reference.

### 1.8 Limitations and Trade-offs

- Assignment does **not** provide a true copy. If modification of the function object were possible (it is not, because function objects are immutable in practice), changes would affect all references.  
- To create a functionally independent copy, one would need to reconstruct the function using its code object and closure, which is non‑trivial and rarely necessary.

---

## Closures

### 2.1 Definition and Concept Overview

A closure is a nested function that captures and remembers the values of variables from its enclosing lexical scope, even when that scope is no longer active. The captured variables are referred to as **free variables**.

### 2.2 Core Principles and Internal Mechanics

- When a nested function references a variable from an outer function, Python stores that variable in a special attribute called `__closure__`.  
- Each free variable is represented as a **cell** object that holds a reference to the variable’s current value.  
- The closure is created at runtime when the outer function is called and the inner function is defined.

### 2.3 Step-by-Step Logical Breakdown

1. Define an outer function that contains a nested function referencing an outer variable.  
2. Call the outer function – this executes its body, creating the nested function **each time** the outer function runs.  
3. The nested function object retains references to the outer variables it uses via cells.  
4. Return the nested function (or store it). Later calls to the nested function access those captured variables.

### 2.4 Implementation (Python)

```python
def make_multiplier(factor: float):
    """
    Return a function that multiplies its argument by a fixed factor.
    The factor is captured via a closure.
    """
    def multiplier(x: float) -> float:
        return x * factor
    return multiplier

# Create two closure instances
double = make_multiplier(2)
triple = make_multiplier(3)

print(double(5))          # 10
print(triple(5))          # 15

# Inspect the closure cells
print(double.__closure__[0].cell_contents)   # 2
print(triple.__closure__[0].cell_contents)   # 3
```

### 2.5 Time and Space Complexity Analysis

- **Time**: Creating a closure is O(1) plus the cost of the outer function’s execution. Calling the inner function is as fast as a normal function call.  
- **Space**: Each closure instance stores references to its free variables. If the outer function is called many times, many separate closure objects may be created, each with its own cells.

### 2.6 Edge Cases and Failure Scenarios

- **Late binding**: Variables captured in a loop may share the same value if the loop variable changes before the closure is called. Use default arguments to bind the current value.  
- **Modifying captured variables**: By default, assignment to a captured variable creates a new local variable inside the nested function. To modify the outer variable, use the `nonlocal` keyword.  
- Deletion of the outer function’s name does not affect existing closures; they retain references to the variables.

### 2.7 Practical Use Cases

- Factory functions that generate specialized functions (e.g., multipliers, configurable loggers).  
- Implementing decorators, where the wrapper function captures the original function and any arguments.  
- Callbacks that need to retain state without using global variables.

### 2.8 Limitations and Trade-offs

- Closures increase memory usage because each captured variable is stored in a cell.  
- Debugging can be more challenging due to the implicit state.  
- Overuse may lead to code that is harder to reason about compared to explicit classes.

---

## Decorators

### 3.1 Definition and Concept Overview

A decorator is a callable (usually a function) that takes another function as an argument and extends or modifies its behavior without explicitly changing its code. Decorators are applied using the `@decorator` syntax, which is syntactic sugar for `func = decorator(func)`.

### 3.2 Core Principles and Internal Mechanics

- Decorators rely on functions being first‑class objects.  
- The decorator is called at the time the function is defined, and its return value (usually a wrapper function) replaces the original function name in the namespace.  
- To preserve the original function’s metadata (name, docstring, etc.), `functools.wraps` should be used inside the wrapper.

### 3.3 Step-by-Step Logical Breakdown

1. Define a decorator function that accepts a function `func`.  
2. Inside the decorator, define a wrapper function that performs additional actions before/after calling `func`.  
3. Return the wrapper function.  
4. Apply the decorator using `@decorator_name` above the target function definition.  
5. At definition time, the interpreter executes `target_function = decorator(target_function)`.

### 3.4 Implementation (Python)

```python
import functools
import time

def timer(func):
    """
    Decorator that prints the execution time of the decorated function.
    """
    @functools.wraps(func)          # Preserves func's metadata
    def wrapper_timer(*args, **kwargs):
        start_time = time.perf_counter()
        value = func(*args, **kwargs)
        end_time = time.perf_counter()
        run_time = end_time - start_time
        print(f"Finished {func.__name__!r} in {run_time:.4f} secs")
        return value
    return wrapper_timer

@timer
def slow_function(seconds: float):
    """Simulate a slow operation."""
    time.sleep(seconds)
    return "Done"

# Calling the decorated function
result = slow_function(1.2)
print(result)
```

### 3.5 Time and Space Complexity Analysis

- **Time**: The wrapper adds a constant overhead (calls to `time.perf_counter`, print statements, etc.). The decorated function’s own complexity remains unchanged.  
- **Space**: The wrapper and its closure occupy memory. Using `functools.wraps` copies metadata, adding negligible overhead.

### 3.6 Edge Cases and Failure Scenarios

- **Decorating functions with arguments**: The wrapper must accept `*args` and `**kwargs` to be generic.  
- **Multiple decorators**: Applied from bottom to top; order matters.  
- **Decorating class methods**: The wrapper must handle `self` correctly – using `*args, **kwargs` suffices.  
- **Return values**: The wrapper should return the original function’s return value unless the decorator intends to replace it.

### 3.7 Practical Use Cases

- Logging function calls and parameters.  
- Measuring execution time.  
- Access control (e.g., checking permissions before executing).  
- Caching / memoization.  
- Retry logic.

### 3.8 Limitations and Trade-offs

- Debugging can be harder because stack traces show wrapper functions. `functools.wraps` mitigates this by copying the original function’s name and docstring.  
- Decorators can obscure the function signature, which may confuse introspection tools.  
- Overuse of decorators can lead to a “stack” of wrappers that complicates performance analysis.

---

## Decorators with Arguments

### 4.1 Definition and Concept Overview

A decorator that accepts its own arguments is implemented as a **decorator factory**. The outer function takes the decorator’s arguments, returns a closure that is the actual decorator, and that decorator returns the wrapper. This allows parameterized behavior (e.g., number of repetitions, retry delays).

### 4.2 Core Principles and Internal Mechanics

- The outermost function accepts the parameters (e.g., `n`) and returns a decorator.  
- The returned decorator is a function that takes the target function and returns the wrapper.  
- The wrapper uses the parameters captured from the outermost function via closure.  
- Syntax: `@repeat(3)` means `repeat(3)` is called first, returning a decorator; then that decorator is applied to the function below.

### 4.3 Step-by-Step Logical Breakdown

1. Define a factory function `repeat` that takes a parameter `n`.  
2. Inside `repeat`, define a function `decorator` that takes the target function `func`.  
3. Inside `decorator`, define a wrapper that accepts arbitrary arguments.  
4. The wrapper executes a loop `for _ in range(n): func(*args, **kwargs)`.  
5. Return the wrapper from `decorator`.  
6. Return `decorator` from `repeat`.  
7. When `@repeat(3)` is encountered, Python calls `repeat(3)`, which returns `decorator`. Then `decorator` is called with the function `say_hello` as its argument, and the result (the wrapper) replaces `say_hello`.

### 4.4 Implementation (Python) – Extreme Line-by-Line Explanation

```python
import functools

def repeat(n):
    """
    Decorator factory: accepts a repetition count and returns a decorator
    that calls the decorated function `n` times.
    """
    # `n` is captured by the closure of the inner functions.

    def decorator(func):
        """
        The actual decorator: takes the function and returns the wrapper.
        """
        # Use wraps to preserve func's metadata (name, docstring, etc.)
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            """
            Wrapper that calls the original function `n` times.
            Discards any return value from func (prints only).
            """
            # Loop n times, where n is captured from the outer factory.
            for _ in range(n):
                # Call the original function with whatever arguments were passed.
                func(*args, **kwargs)
            # No return statement – the wrapper implicitly returns None.
        return wrapper
    return decorator

# Apply the parameterized decorator
@repeat(3)
def say_hello():
    """Prints a greeting."""
    print("Hello")

# Call the decorated function
say_hello()
```

**Line‑by‑line commentary:**

- `def repeat(n):` – The factory function. It accepts the parameter `n` that will control the decorator’s behavior.  
- `def decorator(func):` – This is the actual decorator, returned by `repeat`. It takes the function to be decorated.  
- `@functools.wraps(func)` – Ensures that the wrapper retains the name, docstring, and other metadata of `func`. Without this, introspection would show `wrapper` instead of `say_hello`.  
- `def wrapper(*args, **kwargs):` – The wrapper that will replace the original function. It accepts any positional and keyword arguments, making it generic.  
- `for _ in range(n):` – Loop `n` times. The variable `_` is a conventional placeholder for an unused loop index. `n` is captured from the outer factory via closure.  
- `func(*args, **kwargs)` – Each iteration calls the original function with the same arguments that were passed to the wrapper.  
- `return wrapper` – The decorator returns the wrapper function.  
- `return decorator` – The factory returns the decorator.  
- `@repeat(3)` – Syntactic sugar: `say_hello = repeat(3)(say_hello)`.  
- `def say_hello():` – The original function definition.  
- `say_hello()` – After decoration, this actually calls the wrapper, which calls the original function three times.

**Output:**
```
Hello
Hello
Hello
```

### 4.5 Time and Space Complexity Analysis

- **Time**: The decorated function’s execution time is multiplied by `n` (assuming the function’s runtime is constant). The wrapper overhead is O(1) per call.  
- **Space**: Each decorator instance creates a closure capturing `n`. Multiple decorators on different functions each have their own closure cells.

### 4.6 Edge Cases and Failure Scenarios

- **`n = 0`**: The loop does not execute; the original function is never called. The wrapper returns `None`.  
- **`n` negative**: The loop runs with a negative count, which is treated as an empty range (no iterations). In Python, `range(negative)` yields an empty sequence.  
- **Function with return value**: The wrapper discards any return value. To preserve return values, the wrapper should accumulate or return the last call’s result.  
- **Decorating a method**: The wrapper must accept `self` – handled by `*args, **kwargs`.  
- **Multiple decorators**: If used with other decorators, order must be considered.

### 4.7 Practical Use Cases

- Retry logic (e.g., `@retry(max_attempts=3)`)  
- Rate limiting (e.g., `@rate_limit(calls_per_second=5)`)  
- Parameterized logging (e.g., `@log(level='DEBUG')`)  
- Performance testing (repeat a function many times to measure average runtime)

### 4.8 Limitations and Trade-offs

- Complexity increases with nesting – three levels of functions.  
- The wrapper’s signature is hidden; `functools.wraps` mitigates this but does not restore the exact signature for all introspection tools.  
- Parameter validation (e.g., ensuring `n` is a non‑negative integer) is not performed – adding validation inside the factory is advisable.  
- Overuse can lead to code that is difficult to follow.

---

## Comparison: JavaScript Callbacks vs Python Decorators

| Aspect                | JavaScript Callbacks                          | Python Decorators                             |
|-----------------------|-----------------------------------------------|-----------------------------------------------|
| **Definition**        | A function passed as an argument to another function, to be executed later (often asynchronously). | A function that takes another function and extends its behavior, applied at definition time. |
| **Purpose**           | Handle asynchronous results, event handling, or customise behavior of higher‑order functions (e.g., `map`, `filter`). | Modify or enhance functions (logging, timing, access control) without changing their source. |
| **Timing**            | Invoked when the receiving function decides (e.g., after an I/O operation). | Applied once at function definition; the wrapper is called every time the decorated function is invoked. |
| **Syntax**            | Passed directly: `setTimeout(callback, 1000)` or as arrow functions. | `@decorator` syntactic sugar above the function definition. |
| **State Retention**   | Closures can capture surrounding state (similar to Python closures). | Decorator’s wrapper captures the original function and any decorator arguments via closure. |
| **Asynchronicity**    | Often used for asynchronous operations (e.g., promises, event loops). | Purely synchronous by design; async decorators are possible but require returning an awaitable. |
| **Multiple Application** | Can be composed by passing through several functions manually. | Multiple decorators can be stacked naturally: `@a @b def f(): ...` |
| **Parameterization**  | Achieved by returning a function from a function (e.g., a function that creates a callback). | Achieved via decorator factories (three‑level nesting). |
| **Common Use Cases**  | Event handlers, `Array.prototype.map`, `setTimeout`, AJAX responses. | Logging, timing, access control, memoization, input validation. |
| **Debugging**         | Stack traces show the callback, but anonymous functions can obscure them. | Wrappers obscure the original function; `functools.wraps` helps but is not a perfect solution. |

Both concepts leverage first‑class functions, but they serve different architectural roles. Callbacks are primarily a tool for **control flow** (especially asynchronous), while decorators are a tool for **function transformation** and aspect‑oriented programming.