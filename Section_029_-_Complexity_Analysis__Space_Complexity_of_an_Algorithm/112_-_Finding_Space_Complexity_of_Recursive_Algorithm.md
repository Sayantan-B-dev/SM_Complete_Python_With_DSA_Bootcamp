## Space Complexity Explained

Space complexity measures the total memory an algorithm needs relative to the input size. It consists of:

- **Input space**: Memory for the input data.
- **Auxiliary space**: Extra memory (temporary variables, data structures, recursion stack).

We usually express it in Big O notation, e.g., \(O(1)\), \(O(n)\), \(O(\log n)\).

---

### 1. Insertion Sort – Space Complexity

Insertion sort works by building a sorted array one element at a time, shifting elements to insert the current element in the correct position.

**Auxiliary space**: It sorts **in-place**, meaning it uses only a constant amount of extra memory (a few variables like `key`, `i`, `j`). So auxiliary space is \(O(1)\).

**Total space complexity**: Including input space (the array of size \(n\)), it is \(O(n)\) for the input plus \(O(1)\) auxiliary, but we usually focus on auxiliary space when comparing algorithms.

---

### 2. Another Algorithm: Merge Sort

Merge sort divides the array into halves, recursively sorts them, and merges.

- **Auxiliary space**: It needs temporary arrays during merging. Typically, we allocate an extra array of size \(n\) for merging, so auxiliary space is \(O(n)\).
- **Recursion stack**: Depth is \(\log n\), so stack space is \(O(\log n)\). However, the dominant factor is the temporary array, so overall auxiliary space is \(O(n)\).

**Contrast**: Insertion sort uses less memory (\(O(1)\)) but is slower (\(O(n^2)\)) compared to merge sort’s \(O(n \log n)\) time but \(O(n)\) space.

---

### 3. Space Complexity of Recursive Algorithms

Recursion uses the **call stack** to store function calls. Each recursive call adds a stack frame containing local variables, parameters, and return address. The space complexity depends on the **maximum depth of recursion**, not the total number of calls.

#### How to Find It
- Identify the recursion tree.
- Determine the longest chain of calls (depth).
- Multiply by the space per call (usually constant, but can vary if large local data is created).

#### Example 1: Factorial (Recursive)
```python
def fact(n):
    if n <= 1:
        return 1
    return fact(n-1) * n
```
- Each call waits for the next, forming a chain of length \(n\).
- Stack depth = \(n\).
- Each frame uses constant space → auxiliary space = \(O(n)\).

#### Example 2: Fibonacci (Naive Recursion)
```python
def fib(n):
    if n <= 1:
        return n
    return fib(n-1) + fib(n-2)
```
- The recursion tree is binary, but the stack depth is the longest path from root to leaf, which is \(n\) (e.g., repeatedly calling `fib(n-1)`).
- So **auxiliary space = \(O(n)\)**, even though the number of calls is exponential. The stack never holds more than \(n\) frames at once.
- *Note*: If we store results (memoization), we use additional memory for the cache, changing space complexity.

---

### 4. Step-by-Step: Finding Space Complexity

#### Non-Mathematical Approach
1. List all variables and data structures used.
2. For recursion, draw the call tree and find the deepest branch.
3. Count memory for each level (if each level creates new data, multiply).
4. Express in Big O.

#### Mathematical Approach
For recursive algorithms, we can derive a recurrence for space. Let \(S(n)\) be the space needed for input size \(n\). Then:
- \(S(n) = S(\text{subproblem}) + \text{space per call}\) (for stack).
- Usually, space per call is constant, so \(S(n) = S(n-1) + O(1)\) for factorial, giving \(O(n)\).
- For Fibonacci, the recurrence is similar because we only go deeper along one branch before returning.

---

### 5. Summary Table

| Algorithm       | Auxiliary Space | Why                                      |
|-----------------|-----------------|------------------------------------------|
| Insertion sort  | \(O(1)\)        | In-place, only a few variables           |
| Merge sort      | \(O(n)\)        | Needs temporary array for merging        |
| Quicksort       | \(O(\log n)\)   | In-place partitioning, recursion stack   |
| Factorial (rec) | \(O(n)\)        | Stack depth = n                          |
| Fibonacci (rec) | \(O(n)\)        | Stack depth = n (longest chain)          |

