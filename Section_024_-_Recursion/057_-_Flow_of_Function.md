# Table of Contents

1. [Calling a Function from Another Function](#1-calling-a-function-from-another-function)  
   1.1 Definition and Concept Overview  
   1.2 Core Principles and Internal Mechanics  
   1.3 Step-by-Step Logical Breakdown  
   1.4 Implementation (Python)  
   1.5 Time and Space Complexity Analysis  
   1.6 Edge Cases and Failure Scenarios  
   1.7 Practical Use Cases  
   1.8 Limitations and Trade-offs  

2. [Function Lifetime in Memory](#2-function-lifetime-in-memory)  
   2.1 Definition and Concept Overview  
   2.2 Core Principles and Internal Mechanics  
   2.3 Step-by-Step Logical Breakdown  
   2.4 Implementation (Python)  
   2.5 Time and Space Complexity Analysis  
   2.6 Edge Cases and Failure Scenarios  
   2.7 Practical Use Cases  
   2.8 Limitations and Trade-offs  

3. [DRY Principle](#3-dry-principle)  
   3.1 Definition and Concept Overview  
   3.2 Core Principles and Internal Mechanics  
   3.3 Step-by-Step Logical Breakdown  
   3.4 Implementation (Python)  
   3.5 Time and Space Complexity Analysis  
   3.6 Edge Cases and Failure Scenarios  
   3.7 Practical Use Cases  
   3.8 Limitations and Trade-offs  

4. [Handling Similar Operations with Functions](#4-handling-similar-operations-with-functions)  
   4.1 Definition and Concept Overview  
   4.2 Core Principles and Internal Mechanics  
   4.3 Step-by-Step Logical Breakdown  
   4.4 Implementation (Python)  
   4.5 Time and Space Complexity Analysis  
   4.6 Edge Cases and Failure Scenarios  
   4.7 Practical Use Cases  
   4.8 Limitations and Trade-offs  

---

## 1. Calling a Function from Another Function

### 1.1 Definition and Concept Overview

Function composition allows one function to invoke another. This is a fundamental mechanism for code reuse and modular design. The calling function temporarily suspends its execution, transfers control to the called function, and resumes after the called function returns.

### 1.2 Core Principles and Internal Mechanics

- **Call Stack**: Each function call pushes a new stack frame containing local variables, parameters, and the return address.  
- **Control Transfer**: The CPU jumps to the called function’s code; after completion, it pops the frame and returns to the saved address.  
- **Data Flow**: Arguments are passed (by object reference in Python), and return values are passed back.  
- **Nesting**: Functions can call other functions arbitrarily deep, subject to stack size limits.

### 1.3 Step-by-Step Logical Breakdown

1. Function A executes statements until it reaches a call to Function B.  
2. Arguments for B are evaluated and placed in the stack frame for B.  
3. The current instruction pointer in A is saved.  
4. Control jumps to the start of B’s bytecode.  
5. B executes, possibly calling other functions.  
6. When B finishes, its return value is placed in A’s frame.  
7. The saved instruction pointer is restored, and A continues.

### 1.4 Implementation (Python)

```python
def fetch_data(source_id: int) -> dict:
    """Simulate retrieving data from a database."""
    # Assume a database lookup
    return {"id": source_id, "value": 100}

def process_data(data: dict) -> float:
    """Perform calculations on the data."""
    return data["value"] * 1.05

def main_flow(source_id: int) -> None:
    """Orchestrate data fetching and processing."""
    raw = fetch_data(source_id)          # Call to fetch_data
    result = process_data(raw)           # Call to process_data
    print(f"Processed result: {result}")

main_flow(42)
```

### 1.5 Time and Space Complexity Analysis

- **Time**: Overhead of a function call is O(1) – includes argument passing, stack manipulation, and return. The total time is dominated by the work inside the called functions.  
- **Space**: Each active call occupies a stack frame. Space complexity is O(depth) where depth is the maximum call chain length.

### 1.6 Edge Cases and Failure Scenarios

- **Recursion depth exceeding limit**: Python raises `RecursionError`.  
- **Unhandled exceptions in called function**: Propagate up the stack if not caught.  
- **Incorrect number/type of arguments**: Raises `TypeError` at call site.

### 1.7 Practical Use Cases

- Modularising business logic (e.g., controller calling service layer).  
- Implementing divide‑and‑conquer algorithms.  
- Callback patterns (though typically used with higher‑order functions).

### 1.8 Limitations and Trade-offs

- Deep call chains increase stack memory usage.  
- Excessive function calls may add minor overhead, but Python’s function call cost is non‑negligible in tight loops; inlining may be preferred for performance‑critical code.

---

## 2. Function Lifetime in Memory

### 2.1 Definition and Concept Overview

A function’s lifetime begins when it is called and ends when it returns (or raises an exception). During that period, its stack frame, local variables, and execution context reside in memory. After return, the frame is deallocated (unless referenced elsewhere, e.g., by closures).

### 2.2 Core Principles and Internal Mechanics

- **Stack Frames**: Created on the call stack. They store local variables, parameters, and the return address.  
- **Garbage Collection**: Local variables that reference objects keep those objects alive; when the frame is popped, reference counts decrease, possibly freeing objects.  
- **Closures**: If a nested function references variables from an outer function, those variables may outlive the outer call because they are stored in cells attached to the nested function.

### 2.3 Step-by-Step Logical Breakdown

1. Call to function F: Python allocates a new frame and pushes it onto the call stack.  
2. Local variables are created as they are assigned.  
3. If F returns, the frame is popped, and local variables are destroyed (objects may be garbage‑collected).  
4. If an exception is raised and not handled, the stack unwinds, popping frames until a handler is found.  
5. If F contains a nested function that references its locals, those locals may be promoted to heap‑allocated cells, surviving beyond F’s return.

### 2.4 Implementation (Python)

```python
def outer(x: int):
    """Demonstrate variable lifetime with a closure."""
    y = x * 2

    def inner():
        # y is captured from outer's local scope
        return y + 10

    return inner          # Returns a closure that keeps y alive

# Call outer, get a closure, then outer's frame is popped.
closure_func = outer(5)

# Even though outer has returned, y (value 10) still exists inside the closure.
print(closure_func())     # 20
```

### 2.5 Time and Space Complexity Analysis

- **Time**: Frame allocation/deallocation is O(1).  
- **Space**: Each active call uses O(locals) memory for its frame. Closures add persistent heap storage for captured variables.

### 2.6 Edge Cases and Failure Scenarios

- **Recursion**: Many frames may coexist, potentially exhausting stack memory.  
- **Cyclic references**: Objects referenced by frames may not be collected immediately if cycles exist; the garbage collector handles this.  
- **Large local data**: Holding large data structures in locals keeps them alive until function return.

### 2.7 Practical Use Cases

- Understanding memory usage in recursive algorithms.  
- Designing long‑running services where functions are called repeatedly – frames are ephemeral.  
- Implementing generators and coroutines, which preserve state between calls.

### 2.8 Limitations and Trade-offs

- Stack size limits recursion depth. Python’s default recursion limit is ~1000.  
- Closures can inadvertently keep large objects alive longer than necessary, causing memory leaks if not managed.

---

## 3. DRY Principle

### 3.1 Definition and Concept Overview

DRY (Don’t Repeat Yourself) is a software engineering principle aimed at reducing duplication. Every piece of knowledge should have a single, unambiguous representation within a system. In the context of functions, it means encapsulating repeated logic into reusable functions.

### 3.2 Core Principles and Internal Mechanics

- **Abstraction**: Identify common patterns and extract them into a named function.  
- **Single Source of Truth**: Changes to the logic are made in one place, reducing bugs and maintenance effort.  
- **Function Invocation**: The extracted function is called wherever the logic is needed, rather than copying code.

### 3.3 Step-by-Step Logical Breakdown

1. Identify duplicate code blocks across the codebase.  
2. Factor out the common logic into a new function, with parameters for varying parts.  
3. Replace each duplicate block with a call to the new function.  
4. Test to ensure behavior remains identical.

### 3.4 Implementation (Python)

**Before DRY** (duplicated validation logic):

```python
def create_user(username, email):
    if not username or not email:
        raise ValueError("Missing fields")
    # ... create user

def update_user(user_id, username, email):
    if not username or not email:
        raise ValueError("Missing fields")
    # ... update user
```

**After DRY**:

```python
def validate_non_empty(**fields):
    """Raise ValueError if any field value is empty."""
    for name, value in fields.items():
        if not value:
            raise ValueError(f"Field '{name}' cannot be empty")

def create_user(username, email):
    validate_non_empty(username=username, email=email)
    # ... create user

def update_user(user_id, username, email):
    validate_non_empty(username=username, email=email)
    # ... update user
```

### 3.5 Time and Space Complexity Analysis

- **Time**: No significant overhead; function call cost is negligible compared to the duplicated code it replaces.  
- **Space**: One additional function definition; reduces total code size.

### 3.6 Edge Cases and Failure Scenarios

- Over‑abstraction: creating functions for code that appears only twice may reduce readability if the abstraction is not clear.  
- Parameter explosion: trying to handle too many variations in one function can lead to complex conditional logic.  
- Performance: in extremely performance‑sensitive contexts, function call overhead might be undesirable; inlining may be preferred.

### 3.7 Practical Use Cases

- Validation logic reused across multiple endpoints.  
- Common mathematical calculations (e.g., discount computation).  
- Logging setup and configuration.

### 3.8 Limitations and Trade-offs

- DRY must be balanced with readability and appropriate abstraction levels.  
- Changes to the shared function must be tested against all its callers.  
- Over‑adherence can lead to “tight coupling” between disparate parts of the system that happen to use the same function.

---

## 4. Handling Similar Operations with Functions

### 4.1 Definition and Concept Overview

Functions can be parameterized to perform similar but slightly different tasks. By accepting arguments that control behavior, a single function can serve multiple related use cases, reducing code duplication while maintaining flexibility.

### 4.2 Core Principles and Internal Mechanics

- **Parameters as Control Knobs**: Use arguments to alter the function’s behavior (e.g., flags, thresholds, callback functions).  
- **Conditional Logic Inside**: The function branches based on parameters to execute the appropriate variant.  
- **Higher‑Order Functions**: Passing functions as arguments allows even more flexible behavior (strategy pattern).  
- **Polymorphism**: Using the same interface for different data types or operations.

### 4.3 Step-by-Step Logical Breakdown

1. Identify operations that share a common structure but differ in specific details (e.g., sorting by different keys, applying different discounts).  
2. Design a function signature that captures the varying parts as parameters.  
3. Implement the common workflow, using the parameters at the points of variation.  
4. Call the function with appropriate arguments for each specific case.

### 4.4 Implementation (Python)

**Example: Processing a list of numbers with different operations**

```python
from typing import Callable, List

def process_numbers(numbers: List[float], operation: Callable[[float], float]) -> List[float]:
    """
    Apply a given operation to each number in the list.
    The operation is a function that takes a float and returns a float.
    """
    result = []
    for n in numbers:
        result.append(operation(n))
    return result

# Define several operations
def square(x: float) -> float:
    return x * x

def double(x: float) -> float:
    return x * 2

def as_percentage(x: float) -> float:
    return x / 100

# Use the same processing function for different tasks
data = [1.0, 2.5, 3.0]
squared = process_numbers(data, square)
doubled = process_numbers(data, double)
percent = process_numbers(data, as_percentage)

print(squared)   # [1.0, 6.25, 9.0]
print(doubled)   # [2.0, 5.0, 6.0]
print(percent)   # [0.01, 0.025, 0.03]
```

**Example: Parameterizing with a flag**

```python
def calculate_discount(price: float, customer_type: str) -> float:
    """Apply different discount rates based on customer type."""
    if customer_type == "regular":
        discount = 0.05
    elif customer_type == "premium":
        discount = 0.10
    elif customer_type == "vip":
        discount = 0.20
    else:
        discount = 0.0
    return price * (1 - discount)
```

### 4.5 Time and Space Complexity Analysis

- **Time**: Overhead of condition checks inside the function; if the operation is simple, the cost of branching is constant.  
- **Space**: No extra memory aside from the function definition itself.

### 4.6 Edge Cases and Failure Scenarios

- **Too many parameters**: Functions with many flags become hard to understand and test. Consider splitting into multiple functions.  
- **Unsupported parameter values**: Must handle invalid inputs gracefully (e.g., raise appropriate exceptions).  
- **Performance**: Branching inside a loop may be less efficient than separate specialized functions if the branch condition is invariant across iterations (though JIT compilers may optimise).

### 4.7 Practical Use Cases

- Sorting with custom key functions.  
- Data transformation pipelines (e.g., map, filter).  
- UI event handlers where different actions are triggered by the same function with different parameters.  
- Mathematical functions with different modes (e.g., rounding modes).

### 4.8 Limitations and Trade-offs

- Functions that try to do too much violate the Single Responsibility Principle.  
- Parameterization via callbacks (higher‑order functions) is often cleaner than flag parameters because it separates the variation into distinct, testable units.  
- Excessive parameterization can lead to code that is harder to refactor and understand compared to having several small, focused functions.