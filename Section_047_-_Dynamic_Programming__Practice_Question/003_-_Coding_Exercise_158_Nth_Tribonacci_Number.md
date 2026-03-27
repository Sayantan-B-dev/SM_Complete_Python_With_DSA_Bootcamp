## Table of Contents

1.  Full Commented Code
2.  Overview and Tribonacci Sequence Definition
3.  Algorithm Explanation (Iterative Dynamic Programming)
    - 3.1  Base Cases
    - 3.2  Iterative DP Approach
    - 3.3  Step-by-Step Walkthrough for n = 5 (ASCII Workflow)
4.  Recursion Tree for the Naive Recursive Approach
    - 4.1  Naive Recursive Implementation
    - 4.2  Recursion Tree for n = 5
5.  Complexity Analysis
6.  Example Usage and Output
7.  How to Use the Function
8.  Extended Usage and Customization
9.  Conclusion

---

## 1. Full Commented Code

```python
def tribonacci(n):
    """
    Function to calculate the nth Tribonacci number using dynamic programming.
    
    :param n: int -> The index of the Tribonacci sequence (n >= 0)
    :return: int -> The nth Tribonacci number
    """
    # Base cases as defined by Tribonacci sequence
    if n == 0:
        return 0
    if n == 1 or n == 2:
        return 1

    # Initialize first three values
    first = 0   # T0
    second = 1  # T1
    third = 1   # T2

    # Compute iteratively up to nth value
    for current_index in range(3, n + 1):
        # Tn = Tn-1 + Tn-2 + Tn-3
        current = first + second + third
        
        # Shift values forward for next iteration
        first = second
        second = third
        third = current

    return third

# Example usage (commented out – will be shown later)
# print(tribonacci(4))   # Output: 4
# print(tribonacci(25))  # Output: 1389537
```

---

## 2. Overview and Tribonacci Sequence Definition

The **Tribonacci sequence** is a generalization of the Fibonacci sequence where each term is the sum of the three preceding terms.  
The standard definition is:

```
T0 = 0
T1 = 1
T2 = 1
Tn = Tn-1 + Tn-2 + Tn-3   for n >= 3
```

The first few numbers are:

```
n : 0   1   2   3   4   5   6   7    8    9   10
Tn: 0   1   1   2   4   7  13  24   44   81  149
```

The function `tribonacci(n)` returns the nth Tribonacci number (starting with index 0).

---

## 3. Algorithm Explanation (Iterative Dynamic Programming)

The algorithm uses **bottom‑up dynamic programming** to avoid redundant calculations. Instead of recomputing the same sub‑problems repeatedly, it builds the sequence from the base values upward, storing only the three most recent values.

### 3.1 Base Cases

For `n = 0` return `0`.  
For `n = 1` or `n = 2` return `1`.  
These are the starting seeds of the sequence.

### 3.2 Iterative DP Approach

If `n > 2`, we initialise three variables:

```
first  = 0   (T0)
second = 1   (T1)
third  = 1   (T2)
```

Then we loop from `current_index = 3` to `n` (inclusive). In each iteration we:

1. Compute `current = first + second + third`   → this gives the next Tribonacci number.
2. Shift the window:  
   `first  = second`  
   `second = third`  
   `third  = current`
3. After the loop, `third` holds the nth Tribonacci number.

This technique uses **O(1)** extra space and **O(n)** time.

### 3.3 Step-by-Step Walkthrough for n = 5 (ASCII Workflow)

Let’s compute `tribonacci(5)` step by step.

```
Initial state:
   first = 0    (T0)
   second = 1   (T1)
   third = 1    (T2)

Iteration for current_index = 3:
   current = first + second + third = 0 + 1 + 1 = 2  (T3)
   Shift:
      first  = second = 1
      second = third  = 1
      third  = current = 2
   -> after i=3: first=1, second=1, third=2

Iteration for current_index = 4:
   current = first + second + third = 1 + 1 + 2 = 4  (T4)
   Shift:
      first  = second = 1
      second = third  = 2
      third  = current = 4
   -> after i=4: first=1, second=2, third=4

Iteration for current_index = 5:
   current = first + second + third = 1 + 2 + 4 = 7  (T5)
   Shift:
      first  = second = 2
      second = third  = 4
      third  = current = 7
   -> after i=5: first=2, second=4, third=7

Loop ends. Return third = 7.
```

ASCII representation of the window shift:

```
T0 = 0   T1 = 1   T2 = 1
  |        |        |
  v        v        v
first    second   third

After computing T3:
T3 = first+second+third = 0+1+1 = 2

Shift window:
  first ← second  (1)
  second ← third  (1)
  third ← T3      (2)

Now window holds (T1, T2, T3):
  first=1, second=1, third=2

Continue...
```

---

## 4. Recursion Tree for the Naive Recursive Approach

To appreciate the efficiency of the iterative DP method, it is useful to examine the **naive recursive** version. A direct translation of the recurrence leads to exponential time due to massive overlapping sub‑problems.

### 4.1 Naive Recursive Implementation

```python
def tribonacci_recursive(n):
    if n == 0:
        return 0
    if n == 1 or n == 2:
        return 1
    return tribonacci_recursive(n-1) + tribonacci_recursive(n-2) + tribonacci_recursive(n-3)
```

### 4.2 Recursion Tree for n = 5

The tree below shows the calls made for `tribonacci_recursive(5)`. Each node is labelled with its argument and its value after computation (shown in parentheses). The root makes three recursive calls; those in turn make more calls, and many are repeated.

```
                        T5
                       / | \
                     T4  T3  T2
                    /|\  /|\  |
                  T3 T2 T1 T2 T1 T0
                 /|\ |  |  |  |  |
               T2 T1 T0 1  1  1  0
               |  |  |
               1  1  0
```

Even for n=5 there are **13** calls (count the nodes). For larger n the number grows exponentially (≈ 1.84^n). The iterative DP version avoids this by computing each term only once.

---

## 5. Complexity Analysis

- **Time Complexity:**  
  *Iterative DP* – O(n) because we perform a single loop of length n-2.  
  *Naive Recursive* – O(α^n) where α ≈ 1.8393 (the tribonacci constant) due to the three‑branch recursion.

- **Space Complexity:**  
  *Iterative DP* – O(1) extra space (only three variables).  
  *Naive Recursive* – O(n) due to the recursion stack depth.

---

## 6. Example Usage and Output

Here are a few calls and their expected outputs:

```python
print(tribonacci(0))   # 0
print(tribonacci(1))   # 1
print(tribonacci(2))   # 1
print(tribonacci(3))   # 2
print(tribonacci(4))   # 4
print(tribonacci(5))   # 7
print(tribonacci(6))   # 13
print(tribonacci(7))   # 24
print(tribonacci(8))   # 44
print(tribonacci(9))   # 81
print(tribonacci(10))  # 149
print(tribonacci(25))  # 1389537
```

When run, the output will be:

```
0
1
1
2
4
7
13
24
44
81
149
1389537
```

---

## 7. How to Use the Function

1. **Basic usage:** Pass a non‑negative integer `n` to the function. It returns the nth Tribonacci number.
2. **Error handling:** The function does not validate input types. For production code, you may add a check like `if n < 0: raise ValueError("n must be non‑negative")`.
3. **Integration:** Because it is a pure function, you can easily embed it in larger programs that require Tribonacci numbers.

---

## 8. Extended Usage and Customization

If you need a sequence of Tribonacci numbers up to a certain index, you can modify the function to return a list:

```python
def tribonacci_sequence(limit):
    """
    Return a list of Tribonacci numbers from T0 up to Tlimit.
    """
    if limit < 0:
        return []
    seq = [0, 1, 1]
    if limit <= 2:
        return seq[:limit+1]
    for i in range(3, limit+1):
        seq.append(seq[-1] + seq[-2] + seq[-3])
    return seq
```

Or if you need to handle large `n` efficiently, you might consider using matrix exponentiation (O(log n) time), but the iterative DP is simple and sufficient for most practical ranges.

---

## 9. Conclusion

The iterative dynamic programming implementation of the Tribonacci sequence is:

- **Efficient** – O(n) time and O(1) space.
- **Simple** – uses only a loop and three variables.
- **Robust** – directly follows the recurrence without redundant calculations.

The naive recursive version, while mathematically elegant, suffers from exponential complexity and is impractical for even moderately large `n`. The DP approach demonstrated here is the recommended way to compute Tribonacci numbers.