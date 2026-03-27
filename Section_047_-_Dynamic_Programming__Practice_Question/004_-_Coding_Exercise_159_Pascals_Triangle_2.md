## Table of Contents

1.  Full Commented Code
2.  Overview of Pascal's Triangle and the Problem
3.  Algorithm Explanation (In‑Place Dynamic Programming)
    - 3.1  Core Idea
    - 3.2  Why Right‑to‑Left Update?
    - 3.3  Step‑by‑Step Walkthrough for `rowIndex = 4` (ASCII Workflow)
4.  Recursion Tree for Naive Recursive Approach
    - 4.1  Recursive Definition
    - 4.2  Recursion Tree for a Single Element
5.  Complexity Analysis
6.  Example Usage and Output
7.  How to Use the Function
8.  Extended Usage and Customization
9.  Conclusion

---

## 1. Full Commented Code

```python
def getRow(rowIndex):
    """
    Function to return the rowIndex-th row of Pascal's triangle.
    
    :param rowIndex: int -> Index of the row to return (0‑based)
    :return: List[int] -> The rowIndex-th row of Pascal's triangle
    """
    # Initialize the row with all elements as 1.
    # For row index 0, the row is [1]; for index 1, [1,1]; and so on.
    row = [1] * (rowIndex + 1)

    # Build the row using in-place dynamic programming.
    # We start from the second row (index 2) because rows 0 and 1 are already correct.
    for current_row in range(2, rowIndex + 1):
        # Update values from right to left to avoid overwriting needed values.
        # The first and last elements are always 1, so we skip them.
        for position in range(current_row - 1, 0, -1):
            # Pascal's rule: row[position] = row[position] + row[position-1]
            row[position] = row[position] + row[position - 1]

    return row
```

---

## 2. Overview of Pascal's Triangle and the Problem

Pascal's triangle is a triangular array of binomial coefficients. Each row contains the coefficients of `(1+x)^n`. The rows are built using the recurrence:

```
C(n, k) = C(n-1, k-1) + C(n-1, k)
```

where `C(n,0) = C(n,n) = 1`.

The first few rows (0‑indexed) are:

```
row 0: [1]
row 1: [1, 1]
row 2: [1, 2, 1]
row 3: [1, 3, 3, 1]
row 4: [1, 4, 6, 4, 1]
```

Given a row index `rowIndex`, the function `getRow(rowIndex)` returns that row as a list of integers.

---

## 3. Algorithm Explanation (In‑Place Dynamic Programming)

### 3.1 Core Idea

We can compute the next row from the previous row using the recurrence. Instead of storing all previous rows, we update a single array in‑place. For row index `r`, the array initially contains `[1, 1, …, 1]` (r+1 elements). We then simulate the process row by row, starting from row 2 upward.

For each `current_row` (the row we are building), we iterate over its elements (excluding the first and last, which are always 1) and apply:

```
new_value = old_value + previous_value
```

where `old_value` is the current value in the array (which corresponds to the element from the previous row at the same index) and `previous_value` is the element at index-1 (from the same previous row). To avoid overwriting the `old_value` before it is used for the next element, we update **from right to left**.

### 3.2 Why Right‑to‑Left Update?

If we update left to right, we would overwrite the previous row’s value at index `position` before it is needed to compute the value at index `position+1`. By iterating from the rightmost interior element backward, each update uses the still‑unchanged left neighbour from the previous row.

### 3.3 Step‑by‑Step Walkthrough for `rowIndex = 4` (ASCII Workflow)

Let’s compute `getRow(4)`.

**Initialisation**  
`row = [1] * (4+1) = [1, 1, 1, 1, 1]`  
This corresponds to the initial state before any updates.

**Row 2** (current_row = 2)  
We update positions from `current_row - 1 = 1` down to 1 (only position 1).  
- position = 1:  
  `row[1] = row[1] + row[0] = 1 + 1 = 2`  
  Result: `[1, 2, 1, 1, 1]` → This is now row 2.

**Row 3** (current_row = 3)  
Update positions from 2 down to 1.  
- position = 2:  
  `row[2] = row[2] + row[1] = 1 + 2 = 3`  
  `row` becomes `[1, 2, 3, 1, 1]`  
- position = 1:  
  `row[1] = row[1] + row[0] = 2 + 1 = 3`  
  `row` becomes `[1, 3, 3, 1, 1]` → This is now row 3.

**Row 4** (current_row = 4)  
Update positions from 3 down to 1.  
- position = 3:  
  `row[3] = row[3] + row[2] = 1 + 3 = 4`  
  `row` → `[1, 3, 3, 4, 1]`  
- position = 2:  
  `row[2] = row[2] + row[1] = 3 + 3 = 6`  
  `row` → `[1, 3, 6, 4, 1]`  
- position = 1:  
  `row[1] = row[1] + row[0] = 3 + 1 = 4`  
  `row` → `[1, 4, 6, 4, 1]` → Final row 4.

**ASCII representation of the array evolution for row 4:**

```
Initial:        [1, 1, 1, 1, 1]

After row 2:    [1, 2, 1, 1, 1]
                 ^   (update pos1)

After row 3:    [1, 3, 3, 1, 1]
                     ^   (pos2 first)
                [1, 3, 3, 1, 1]
                 ^        (then pos1)

After row 4:    [1, 3, 3, 4, 1]
                        ^   (pos3)
                [1, 3, 6, 4, 1]
                     ^      (pos2)
                [1, 4, 6, 4, 1]
                 ^           (pos1)

Final:          [1, 4, 6, 4, 1]
```

---

## 4. Recursion Tree for Naive Recursive Approach

### 4.1 Recursive Definition

A direct recursive implementation to compute a single element `C(n,k)` using the recurrence:

```python
def pascal_element(n, k):
    if k == 0 or k == n:
        return 1
    return pascal_element(n-1, k-1) + pascal_element(n-1, k)
```

To get the entire row for index `n`, one would call this function for each `k` from 0 to n. This leads to massive overlapping sub‑problems.

### 4.2 Recursion Tree for a Single Element

Consider computing `C(4,2)` (which should be 6). The recursion tree is:

```
                      C(4,2)
                     /      \
                C(3,1)      C(3,2)
               /    \       /    \
          C(2,0)  C(2,1)  C(2,1)  C(2,2)
            |      /   \   /   \     |
            1   C(1,0)C(1,1)C(1,0)C(1,1)  1
                 |     |    |     |
                 1     1    1     1
```

Each node represents a call. There are many repeated calls (e.g., `C(2,1)` appears twice). The total number of calls grows exponentially (≈ 2^n). Using this naive recursion for a full row would be highly inefficient.

---

## 5. Complexity Analysis

- **Time Complexity:**  
  The in‑place DP algorithm runs two nested loops: the outer loop from 2 to `rowIndex` (n-1 iterations), and the inner loop runs up to `current_row - 1` steps. The total number of updates is:

  ```
  ∑_{r=2}^{n} (r-1) = n(n-1)/2 - 1 = O(n²)
  ```

  Hence **O(n²)** time.

- **Space Complexity:**  
  Only one list of length `rowIndex + 1` is used, so **O(n)** space.

In contrast, a naive recursive generation would require **O(2ⁿ)** time and **O(n)** stack space, which is impractical for even moderate `n`.

---

## 6. Example Usage and Output

```python
print(getRow(0))   # [1]
print(getRow(1))   # [1, 1]
print(getRow(2))   # [1, 2, 1]
print(getRow(3))   # [1, 3, 3, 1]
print(getRow(4))   # [1, 4, 6, 4, 1]
print(getRow(5))   # [1, 5, 10, 10, 5, 1]
print(getRow(10))  # [1, 10, 45, 120, 210, 252, 210, 120, 45, 10, 1]
```

When run, the output will be:

```
[1]
[1, 1]
[1, 2, 1]
[1, 3, 3, 1]
[1, 4, 6, 4, 1]
[1, 5, 10, 10, 5, 1]
[1, 10, 45, 120, 210, 252, 210, 120, 45, 10, 1]
```

---

## 7. How to Use the Function

1. **Input:** A non‑negative integer `rowIndex` (0‑based).
2. **Output:** A list of integers representing that row of Pascal's triangle.
3. The function does not modify any external state; it is pure.
4. No error handling is provided in the original code; for production, you might add input validation (e.g., `if rowIndex < 0: raise ValueError(...)`).

---

## 8. Extended Usage and Customization

If you need to generate Pascal's triangle up to a certain row, you can adapt the function to return all rows:

```python
def getTriangle(rows):
    triangle = []
    for i in range(rows):
        row = [1] * (i+1)
        for j in range(1, i):
            row[j] = triangle[i-1][j-1] + triangle[i-1][j]
        triangle.append(row)
    return triangle
```

Or you can use the in‑place technique to build each row without storing the whole triangle, saving space.

Another common variation is to compute the row directly using the combinatorial formula `C(n,k) = C(n,k-1) * (n-k+1)/k`, which yields O(n) time and O(n) space. However, the DP method shown here is intuitive and avoids division and potential floating‑point issues.

---

## 9. Conclusion

The in‑place dynamic programming method for generating a row of Pascal’s triangle is:

- **Efficient** – O(n²) time, O(n) space.
- **Elegant** – uses the recurrence `C(n,k) = C(n-1,k-1) + C(n-1,k)` directly.
- **Practical** – handles large row indices well (up to the limits of integer size).

The key insight is the right‑to‑left update, which allows overwriting the same array in‑place without losing the necessary previous row values. This technique is a classic example of using dynamic programming to reduce space complexity from O(n²) to O(n) while maintaining the time complexity of the straightforward iterative construction.