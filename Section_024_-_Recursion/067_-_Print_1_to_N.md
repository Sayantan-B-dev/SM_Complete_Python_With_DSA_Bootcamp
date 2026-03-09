# Printing 1 to n Using Recursion

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

Recursion is a technique where a function solves a problem by calling itself with a smaller instance of the same problem. Every recursive solution requires:

- **Base case(s)**: The smallest instance that can be solved directly without further recursion.  
- **Recursive case(s)**: A step that reduces the problem toward a base case and invokes the function again.

The problem *“print numbers from 1 to n”* is a classic introductory recursion problem. The goal is to output the integers 1, 2, 3, …, n in increasing order, using only recursion and without explicit iteration constructs (loops).

The natural recursive formulation:

- If `n == 1`, simply print 1.
- If `n > 1`, first print all numbers from 1 to `n-1` (by recursively calling the function with `n-1`), then print `n`.

This is a **head recursion** pattern: the recursive call occurs before the work (printing) of the current call, so the numbers are printed on the way back up the call stack.

---

## 2. Core Principles and Internal Mechanics

Understanding the execution requires examining the call stack. Each recursive call pushes a new frame onto the stack, containing local variables (the parameter `n`) and the return address. Frames are popped when the call returns.

For `print_1_to_n(3)`:

1. `print_1_to_n(3)` is called. Since `3 > 1`, it calls `print_1_to_n(2)` and **suspends**, waiting for that call to complete.
2. `print_1_to_n(2)` is called. Since `2 > 1`, it calls `print_1_to_n(1)` and suspends.
3. `print_1_to_n(1)` is called. The base case is reached: it prints `1` and returns to its caller (`print_1_to_n(2)`).
4. `print_1_to_n(2)` resumes, prints `2`, and returns to `print_1_to_n(3)`.
5. `print_1_to_n(3)` resumes, prints `3`, and returns.

Thus, the output order is `1 2 3`. The stack depth at its peak is 3 frames. The key insight is that the **print after the recursive call** causes the numbers to be printed in ascending order because the smallest number is printed first (when the deepest call hits the base case).

---

## 3. Step-by-Step Logical Breakdown

### Mathematical Formulation

Define a function `P(n)` that produces the sequence of integers from 1 to n.

Base case:  
`P(1) = print(1)`

Recursive case:  
`P(n) = P(n-1) ; print(n)` for n > 1  
where `;` denotes sequential composition.

This recurrence directly mirrors the algorithm.

### Pseudocode

```
function print_1_to_n(n):
    if n == 1:
        print(1)
    else:
        print_1_to_n(n - 1)
        print(n)
```

### Flowchart Description

The flowchart consists of a single decision node:

- **Start**
- Input: integer n
- Is n equal to 1?
  - Yes: Print 1 → End
  - No: Recursively call `print_1_to_n(n-1)`, then print n → End

No loops are present; the recursion handles the repetition.

### Call Stack Diagram

Below is an ASCII representation of the stack frames for `print_1_to_n(3)`. Time progresses downward.

```
Step 1: print_1_to_n(3) called
        Stack: [3*]  (* = active)

Step 2: calls print_1_to_n(2) – 3 waits
        Stack: [3] [2*]

Step 3: calls print_1_to_n(1) – 2 waits
        Stack: [3] [2] [1*]

Step 4: base case prints 1; frame 1 returns
        Stack: [3] [2*]   (prints 2 after return)

Step 5: frame 2 prints 2 and returns
        Stack: [3*]       (prints 3 after return)

Step 6: frame 3 prints 3 and returns
        Stack: empty
```

The diagram illustrates the **last-in-first-out** nature of recursion and the deferred printing.

---

## 4. Implementation (Python)

The Python implementation follows directly from the pseudocode. Comments explain the logic.

```python
def print_1_to_n(n: int) -> None:
    """
    Prints integers from 1 to n in increasing order using recursion.
    
    Parameters:
        n (int): The upper bound (inclusive). Assumes n >= 1.
    
    Returns:
        None
    """
    # Base case: when n is 1, print 1 and stop recursing
    if n == 1:
        print(1)
    else:
        # Recursive case: first print all numbers from 1 to n-1
        print_1_to_n(n - 1)
        # Then print the current n after the recursive call returns
        print(n)
```

**Example usage:**

```python
print_1_to_n(5)
# Output:
# 1
# 2
# 3
# 4
# 5
```

---

## 5. Time and Space Complexity Analysis

- **Time Complexity:** O(n)  
  Exactly one `print` operation per recursive call, and there are n calls. The overhead of function calls is constant per invocation, leading to linear time.

- **Space Complexity:** O(n)  
  Each recursive call adds a frame to the call stack. The maximum stack depth equals n (when the base case is reached). No additional memory is allocated beyond the stack frames and the integer parameter (passed by value). Python’s lack of tail‑call optimization means stack space grows linearly with n.

---

## 6. Edge Cases and Failure Scenarios

- **n = 0 or negative integer**  
  The current implementation assumes n ≥ 1. For n ≤ 0, the base case never triggers, and the function recurses infinitely until a `RecursionError` occurs. A robust version should handle this, e.g., by printing nothing or raising an exception.

- **n very large**  
  Python’s default recursion limit is typically 1000. For n > 1000, a `RecursionError` will be raised. The limit can be increased with `sys.setrecursionlimit()`, but this is not recommended for production code as it risks stack overflow.

- **n = 1**  
  The base case executes directly, printing 1. No recursion occurs.

- **Non-integer input**  
  Python’s type hints are not enforced at runtime; passing a non-integer may cause errors during comparison or printing.

---

## 7. Practical Use Cases

While printing numbers is trivial, the problem serves as a pedagogical tool to:

- Illustrate the concept of recursion and the call stack.
- Demonstrate head recursion (work after recursive call).
- Highlight the difference between recursion and iteration.
- Provide a foundation for more complex recursive algorithms (tree traversals, divide-and-conquer).

In real-world software, an iterative loop (`for i in range(1, n+1): print(i)`) is almost always preferred for this task due to its simplicity and lack of stack depth issues.

---

## 8. Limitations and Trade-offs

- **Recursion depth limit**: Python’s stack frames consume memory, and the recursion depth is artificially capped. This makes recursion impractical for large n.  
- **Readability**: For this problem, the iterative version is arguably clearer. Recursion adds conceptual overhead without benefit.  
- **Performance**: Function call overhead in Python is higher than simple loop iteration, making recursion slower for large n.  
- **Alternative**: Converting to iteration (or using tail recursion with an accumulator) does not help in Python because TCO is absent. The iterative solution is the appropriate choice for any n where output is needed.

The recursive approach is retained primarily for educational purposes and as a building block for understanding recursion in contexts where it is genuinely necessary (e.g., traversing recursive data structures like trees).