# Printing n to 1 Using Recursion

## Table of Contents
1. [Definition and Concept Overview](#definition-and-concept-overview)  
2. [Core Principles and Internal Mechanics](#core-principles-and-internal-mechanics)  
3. [Step-by-Step Logical Breakdown](#step-by-step-logical-breakdown)  
   - [Mathematical Formulation](#mathematical-formulation)  
   - [Pseudocode](#pseudocode)  
   - [Flowchart Description](#flowchart-description)  
   - [Call Stack Diagram](#call-stack-diagram)  
4. [Implementation (Python)](#implementation-python)  
5. [Time and Space Complexity Analysis](#time-and-space-complexity-analysis)  
6. [Edge Cases and Failure Scenarios](#edge-cases-and-failure-scenarios)  
7. [Practical Use Cases](#practical-use-cases)  
8. [Limitations and Trade-offs](#limitations-and-trade-offs)  

---

## 1. Definition and Concept Overview

Recursion solves a problem by reducing it to smaller instances of the same problem. Two essential components are required:

- **Base case**: The simplest instance, solvable directly without recursion.  
- **Recursive case**: A step that moves toward the base case and invokes the function again.

The problem *“print numbers from n down to 1”* requires outputting the integers n, n-1, …, 1 in strictly decreasing order, using only recursion.

A natural recursive formulation:

- If `n == 1`, print 1 and stop.  
- If `n > 1`, first print the current `n`, then recursively print all numbers from `n-1` down to 1.

This pattern is an example of **tail recursion** in the conceptual sense (the recursive call is the last operation), although Python does not optimize tail calls. The work (printing) occurs **before** the recursive call, so the output order descends naturally.

---

## 2. Core Principles and Internal Mechanics

The execution flow is governed by the call stack. Each function call pushes a new frame containing the parameter `n` and the return address. Frames are popped when the call returns.

For `print_n_to_1(3)`:

1. `print_n_to_1(3)` is called. It prints `3`, then calls `print_n_to_1(2)` and suspends until that call returns.
2. `print_n_to_1(2)` prints `2`, then calls `print_n_to_1(1)`.
3. `print_n_to_1(1)` prints `1` (base case) and returns to its caller (`print_n_to_1(2)`).
4. `print_n_to_1(2)` has no further work after the recursive call (printing already done) and returns to `print_n_to_1(3)`.
5. `print_n_to_1(3)` similarly returns.

The output order is determined by the **pre‑recursion** print statements: the outermost call prints first, then the next, and so on, yielding descending order. After the base case, no additional printing occurs; the stack unwinds silently.

---

## 3. Step-by-Step Logical Breakdown

### Mathematical Formulation

Define a function `Q(n)` that outputs the sequence n, n-1, …, 1.

Base case:  
`Q(1) = print(1)`

Recursive case:  
`Q(n) = print(n) ; Q(n-1)` for n > 1  
where `;` denotes sequential composition.

### Pseudocode

```
function print_n_to_1(n):
    if n == 1:
        print(1)
    else:
        print(n)
        print_n_to_1(n - 1)
```

### Flowchart Description

The flowchart contains a single decision point:

- **Start**
- Input: integer n
- Is n equal to 1?
  - Yes: Print 1 → End
  - No: Print n, then recursively call `print_n_to_1(n-1)` → End

No loops are required; recursion manages the repetition.

### Call Stack Diagram

Below is an ASCII representation for `print_n_to_1(3)`. Time moves downward.

```
Step 1: print_n_to_1(3) called
        prints 3
        calls print_n_to_1(2) – 3 waits
        Stack: [3*]  (* = active)

Step 2: print_n_to_1(2) called
        prints 2
        calls print_n_to_1(1) – 2 waits
        Stack: [3] [2*]

Step 3: print_n_to_1(1) called
        prints 1 (base case)
        returns to 2
        Stack: [3] [2*]   (2 resumes)

Step 4: print_n_to_1(2) returns to 3
        Stack: [3*]       (3 resumes)

Step 5: print_n_to_1(3) returns
        Stack: empty
```

The output is `3 2 1` as printed during the descent.

---

## 4. Implementation (Python)

The Python code directly mirrors the pseudocode. Comments explain each logical block.

```python
def print_n_to_1(n: int) -> None:
    """
    Prints integers from n down to 1 in decreasing order using recursion.

    Parameters:
        n (int): The starting upper bound (inclusive). Assumes n >= 1.

    Returns:
        None
    """
    # Base case: when n is 1, print it and stop recursing
    if n == 1:
        print(1)
    else:
        # Recursive case: first print the current n,
        # then recursively print the rest from n-1 down to 1
        print(n)
        print_n_to_1(n - 1)
```

**Example usage:**

```python
print_n_to_1(5)
# Output:
# 5
# 4
# 3
# 2
# 1
```

---

## 5. Time and Space Complexity Analysis

- **Time Complexity:** O(n)  
  Exactly one `print` operation per recursive call, and the function is invoked n times. Constant overhead per call yields linear time.

- **Space Complexity:** O(n)  
  Each recursive call consumes a stack frame. The maximum stack depth is n (when the base case is reached). No additional memory is allocated beyond the integer parameter and the call frames. Python lacks tail‑call optimization, so stack space grows linearly with n.

---

## 6. Edge Cases and Failure Scenarios

- **n = 0 or negative**  
  The implementation expects n ≥ 1. For n ≤ 0, the base case never triggers, causing infinite recursion until a `RecursionError` occurs. A robust version should validate input, e.g., printing nothing or raising an exception for invalid values.

- **n very large**  
  Python’s default recursion limit (typically 1000) will be exceeded for n > 1000, raising `RecursionError`. The limit can be increased with `sys.setrecursionlimit()`, but this is generally unsafe for production code.

- **n = 1**  
  The base case executes directly, printing 1. No recursion occurs.

- **Non-integer input**  
  Python’s dynamic typing does not enforce type hints; passing a non-integer (e.g., a string or float) may lead to errors during comparison or printing.

---

## 7. Practical Use Cases

While printing a descending sequence is trivial, the problem serves to:

- Demonstrate the difference between **pre‑recursion work** (printing before the call) and **post‑recursion work** (printing after the call), contrasting with the ascending version.  
- Illustrate the concept of **tail recursion** (where the recursive call is the final action) and its relationship to iteration.  
- Provide a simple foundation for understanding recursion patterns used in tree traversals (e.g., pre‑order traversal).  
- Act as a building block for recursive algorithms where an action must be performed before descending into subproblems (e.g., generating prefixes, backtracking).

In practice, an iterative loop (`for i in range(n, 0, -1): print(i)`) is more efficient and avoids recursion depth limits.

---

## 8. Limitations and Trade-offs

- **Recursion depth limit**: Python imposes an artificial limit on the number of nested function calls, making recursion impractical for large n.  
- **Performance**: Function call overhead in Python is higher than simple loop iteration; for large n, the iterative version is faster and more memory‑efficient.  
- **Readability**: For this specific problem, the iterative solution is arguably clearer. Recursion adds cognitive overhead without tangible benefit.  
- **Tail call optimization absence**: Even though the algorithm is conceptually tail‑recursive, Python does not optimize it, so it still consumes O(n) stack space.

The recursive approach is retained for educational purposes, illustrating recursion mechanics and preparing for problems where recursion is the natural fit (e.g., traversing recursive data structures). For any production use requiring large ranges, iteration is the appropriate choice.