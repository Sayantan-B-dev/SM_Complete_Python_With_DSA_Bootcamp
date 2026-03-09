# Recursion and Factorial Computation in Python

## Table of Contents

1. Definition and Concept Overview
2. Mathematical Foundation of Factorial
3. Recursive Problem Decomposition
4. Recursive Factorial Implementation
5. Execution Trace of Recursive Calls
6. Computing Multiple Factorials Recursively
7. Python Recursion Depth Limit
8. Inspecting the Current Recursion Limit
9. Modifying the Recursion Limit
10. Time and Space Complexity Analysis
11. Edge Cases and Failure Scenarios
12. Practical Use Cases
13. Limitations and Trade-offs

# 1. Definition and Concept Overview

Recursion is a computational strategy where a function solves a problem by invoking itself with progressively smaller inputs. The original problem is repeatedly decomposed into subproblems until reaching a trivial instance that can be solved directly.

Key structural elements required for recursion:

| Component      | Description                                            |
| -------------- | ------------------------------------------------------ |
| Recursive Case | Defines how the function reduces the problem size      |
| Base Case      | A terminating condition preventing infinite recursion  |
| Stack Frames   | Each recursive call occupies a frame in the call stack |

A recursive algorithm forms a chain of function calls where each invocation pauses execution and delegates part of the work to another invocation. The execution resumes only after the delegated call completes.

Factorial computation provides a canonical example of recursive problem decomposition.

# 2. Mathematical Foundation of Factorial

The factorial function represents the product of all positive integers up to a given number.

| Symbol | Meaning                  |
| ------ | ------------------------ |
| `n!`   | factorial of integer `n` |

Mathematical definition:

```
n! = n × (n-1) × (n-2) × ... × 1
```

Base definition:

```
0! = 1
1! = 1
```

Recursive mathematical identity:

```
n! = n × (n-1)!
```

Example expansion:

```
5! = 5 × 4!
4! = 4 × 3!
3! = 3 × 2!
2! = 2 × 1!
1! = 1
```

This identity naturally maps to recursive computation because the factorial of `n` depends on factorial of `n-1`.

# 3. Recursive Problem Decomposition

Recursive computation proceeds by repeatedly breaking the problem into smaller subproblems.

| Input | Subproblem | Remaining Work  |
| ----- | ---------- | --------------- |
| `5!`  | `4!`       | multiply by `5` |
| `4!`  | `3!`       | multiply by `4` |
| `3!`  | `2!`       | multiply by `3` |
| `2!`  | `1!`       | multiply by `2` |
| `1!`  | base case  | return `1`      |

The sequence of reductions eventually reaches the base case, which stops recursion and begins unwinding the call stack.

# 4. Recursive Factorial Implementation

### Python Implementation

```python
def factorial_recursive(number: int) -> int:
    """
    Computes factorial using recursion.

    Parameters:
        number (int): Non-negative integer whose factorial will be computed.

    Returns:
        int: factorial value.
    """

    # Base condition ensures recursion terminates.
    # Without this condition the function would continue indefinitely.
    if number == 0 or number == 1:
        return 1

    # Recursive case reduces the problem size by one.
    # The function delegates computation of (number - 1)!
    # and multiplies the result with the current number.
    return number * factorial_recursive(number - 1)
```

### Expected Output

```python
print(factorial_recursive(5))
```

```
120
```

# 5. Execution Trace of Recursive Calls

Step-by-step evaluation of:

```
factorial_recursive(5)
```

### Call Stack Growth

```
factorial_recursive(5)
    -> factorial_recursive(4)
        -> factorial_recursive(3)
            -> factorial_recursive(2)
                -> factorial_recursive(1)
```

### Base Case Trigger

```
factorial_recursive(1) returns 1
```

### Stack Unwinding

```
factorial_recursive(2) returns 2 × 1 = 2
factorial_recursive(3) returns 3 × 2 = 6
factorial_recursive(4) returns 4 × 6 = 24
factorial_recursive(5) returns 5 × 24 = 120
```

### Visualization of Stack Frames

| Stack Level | Function Call | Result |
| ----------- | ------------- | ------ |
| 1           | factorial(5)  | 120    |
| 2           | factorial(4)  | 24     |
| 3           | factorial(3)  | 6      |
| 4           | factorial(2)  | 2      |
| 5           | factorial(1)  | 1      |

Each frame remains suspended until the deeper recursive invocation completes.

# 6. Computing Multiple Factorials Recursively

Recursive factorial can be applied repeatedly to a collection of values.

### Example: Factorial of a List of Numbers

```python
def factorial(number: int) -> int:
    """
    Standard recursive factorial implementation.
    """

    if number <= 1:
        return 1

    return number * factorial(number - 1)


def factorial_list(numbers: list[int]) -> list[int]:
    """
    Computes factorial for each number in a list.
    """

    results = []

    for value in numbers:
        # Each element invokes a separate recursive computation
        results.append(factorial(value))

    return results
```

### Expected Output

```python
numbers = [3, 4, 5]

print(factorial_list(numbers))
```

```
[6, 24, 120]
```

# 7. Python Recursion Depth Limit

Python protects the interpreter stack by limiting recursion depth.

Reason for the limit:

| Issue                 | Explanation                                        |
| --------------------- | -------------------------------------------------- |
| Stack overflow risk   | Every recursive call consumes stack memory         |
| Interpreter stability | Unlimited recursion could crash the interpreter    |
| OS limitations        | Stack size is bounded by system memory constraints |

Each recursive call allocates:

* local variables
* return address
* execution state

The accumulation of frames eventually exceeds safe stack memory.

# 8. Inspecting the Current Recursion Limit

Python exposes recursion limit through the `sys` module.

### Implementation

```python
import sys

# Retrieve interpreter recursion depth limit
current_limit = sys.getrecursionlimit()

print("Current recursion depth limit:", current_limit)
```

### Expected Output

```
Current recursion depth limit: 1000
```

The default limit typically equals `1000`, although it may vary across Python distributions.

# 9. Modifying the Recursion Limit

Python allows modification of recursion depth through `sys.setrecursionlimit()`.

### Implementation

```python
import sys

# Increase recursion depth to support deeper recursive algorithms
sys.setrecursionlimit(2000)

print("Updated recursion depth limit:", sys.getrecursionlimit())
```

### Expected Output

```
Updated recursion depth limit: 2000
```

Excessively large limits increase risk of interpreter crashes due to uncontrolled stack growth.

# 10. Time and Space Complexity Analysis

### Recursive Factorial

| Metric           | Complexity |
| ---------------- | ---------- |
| Time Complexity  | O(n)       |
| Space Complexity | O(n)       |

Explanation:

Time complexity equals `n` because each recursive call reduces the argument by exactly one.

Space complexity equals `n` due to the call stack containing `n` active frames.

### Iterative Comparison

| Approach            | Time | Space |
| ------------------- | ---- | ----- |
| Recursive factorial | O(n) | O(n)  |
| Iterative factorial | O(n) | O(1)  |

Recursion introduces additional stack overhead.

# 11. Edge Cases and Failure Scenarios

| Scenario          | Behavior                         |
| ----------------- | -------------------------------- |
| Negative input    | Mathematical factorial undefined |
| Very large input  | Recursion depth exceeded         |
| Missing base case | Infinite recursion               |
| Non-integer input | Invalid computation domain       |

### Defensive Implementation

```python
def safe_factorial(number: int) -> int:

    # Reject negative values explicitly
    if number < 0:
        raise ValueError("Factorial undefined for negative numbers")

    # Base condition prevents infinite recursion
    if number <= 1:
        return 1

    return number * safe_factorial(number - 1)
```

### Expected Output

```python
print(safe_factorial(4))
```

```
24
```

# 12. Practical Use Cases

| Domain                   | Example                       |
| ------------------------ | ----------------------------- |
| Combinatorics            | Permutations and combinations |
| Probability theory       | Binomial distributions        |
| Algorithm design         | Backtracking recursion        |
| Mathematical computation | Series expansions             |
| Compiler design          | Recursive descent parsing     |

Factorial appears frequently within combinatorial mathematics:

```
nCr = n! / (r! × (n-r)!)
```

This relationship demonstrates factorial’s foundational role in discrete mathematics.

# 13. Limitations and Trade-offs

| Factor                | Impact                                          |
| --------------------- | ----------------------------------------------- |
| Stack consumption     | Memory grows linearly with recursion depth      |
| Debug complexity      | Deep recursion complicates stack inspection     |
| Performance overhead  | Function calls incur overhead compared to loops |
| Recursion depth limit | Large inputs require alternative strategies     |

Large factorial computations typically adopt iterative or dynamic programming approaches to avoid recursion depth limitations.

For example:

```python
def factorial_iterative(number: int) -> int:
    """
    Iterative factorial implementation avoiding recursion overhead.
    """

    result = 1

    for value in range(2, number + 1):
        result *= value

    return result
```

### Expected Output

```python
print(factorial_iterative(5))
```

```
120
```
