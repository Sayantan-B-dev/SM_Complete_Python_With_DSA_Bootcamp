## Left Rotation of Array: Explanation, Approach, Algorithm, Pseudocode

**Explanation:**  
Left rotation of an array means shifting all elements to the left by a given number of positions (D). Elements that are shifted out from the left end reappear at the right end in the same order. For example, rotating `[1,2,3,4,5]` left by 2 gives `[3,4,5,1,2]`.

**Approach:**  
Several approaches exist: using slicing (simple copy), using a deque (double-ended queue), manual rotation with loops, reversal algorithm (efficient in-place), or using modulo indexing to build a new list.

**Algorithm (using slicing):**  
1. Let `N = len(arr)`.  
2. Let `D = D % N` (to handle cases where D > N).  
3. Return `arr[D:] + arr[:D]`.

**Pseudocode:**  
```
function rotateLeft(arr, D):
    N = length(arr)
    D = D mod N
    return arr[D .. N-1] concatenated with arr[0 .. D-1]
```

---

## Original Slicing Method

```python
def rotate_left_slice(arr, d):
    n = len(arr)
    d = d % n                                # normalize d to avoid extra rotations
    # Main logic: slice from d to end and from start to d, then concatenate
    return arr[d:] + arr[:d]

# Example call
arr = [1, 2, 3, 4, 5]
d = 2
result = rotate_left_slice(arr, d)
print("Left rotated:", result)   # Output: Left rotated: [3, 4, 5, 1, 2]
```

---

## Using `collections.deque`

```python
from collections import deque

def rotate_left_deque(arr, d):
    dq = deque(arr)                          # create deque from list
    dq.rotate(-d)                            # negative rotates left
    return list(dq)                           # convert back to list

# Example call
arr = [1, 2, 3, 4, 5]
d = 2
result = rotate_left_deque(arr, d)
print("Left rotated:", result)   # Output: Left rotated: [3, 4, 5, 1, 2]
```

---

## List Comprehension with Modulo Indexing

```python
def rotate_left_comprehension(arr, d):
    n = len(arr)
    d = d % n
    # Main logic: new list using (i+d) % n to pick elements in rotated order
    return [arr[(i + d) % n] for i in range(n)]

# Example call
arr = [1, 2, 3, 4, 5]
d = 2
result = rotate_left_comprehension(arr, d)
print("Left rotated:", result)   # Output: Left rotated: [3, 4, 5, 1, 2]
```

---

## Manual Loop with Pop and Append (Inefficient)

```python
def rotate_left_pop_append(arr, d):
    n = len(arr)
    d = d % n
    # Make a copy to avoid modifying original (optional)
    rotated = arr[:]
    for _ in range(d):
        # Special: pop first element and append it to the end
        rotated.append(rotated.pop(0))       # pop(0) is O(n) each time
    return rotated

# Example call
arr = [1, 2, 3, 4, 5]
d = 2
result = rotate_left_pop_append(arr, d)
print("Left rotated:", result)   # Output: Left rotated: [3, 4, 5, 1, 2]
```

---

## Reversal Algorithm (In-Place)

```python
def reverse_sublist(arr, start, end):
    # Helper to reverse a portion of the list in-place
    while start < end:
        arr[start], arr[end] = arr[end], arr[start]
        start += 1
        end -= 1

def rotate_left_reversal(arr, d):
    n = len(arr)
    d = d % n
    if d == 0:
        return arr[:]                          # no rotation needed
    # Main logic: three reversals
    reverse_sublist(arr, 0, d - 1)            # reverse first part
    reverse_sublist(arr, d, n - 1)            # reverse second part
    reverse_sublist(arr, 0, n - 1)            # reverse whole array
    return arr                                  # original list modified

# Example call (creates copy to keep original unchanged for demo)
arr = [1, 2, 3, 4, 5]
d = 2
result = rotate_left_reversal(arr[:], d)      # pass a copy
print("Left rotated:", result)   # Output: Left rotated: [3, 4, 5, 1, 2]
```

---

## Using `itertools.chain` and `islice`

```python
from itertools import chain, islice

def rotate_left_itertools(arr, d):
    n = len(arr)
    d = d % n
    # chain two slices together using islice to avoid creating intermediate lists
    return list(chain(islice(arr, d, n), islice(arr, 0, d)))

# Example call
arr = [1, 2, 3, 4, 5]
d = 2
result = rotate_left_itertools(arr, d)
print("Left rotated:", result)   # Output: Left rotated: [3, 4, 5, 1, 2]
```

---

## Using NumPy (if available)

```python
import numpy as np

def rotate_left_numpy(arr, d):
    np_arr = np.array(arr)
    # numpy.roll with negative shift rotates left
    return np.roll(np_arr, -d).tolist()

# Example call
arr = [1, 2, 3, 4, 5]
d = 2
result = rotate_left_numpy(arr, d)
print("Left rotated:", result)   # Output: Left rotated: [3, 4, 5, 1, 2]
```

---

## Building a New List with Two Separate Loops

```python
def rotate_left_two_loops(arr, d):
    n = len(arr)
    d = d % n
    rotated = []
    # First loop: add elements from index d to end
    for i in range(d, n):
        rotated.append(arr[i])
    # Second loop: add elements from start to d-1
    for i in range(d):
        rotated.append(arr[i])
    return rotated

# Example call
arr = [1, 2, 3, 4, 5]
d = 2
result = rotate_left_two_loops(arr, d)
print("Left rotated:", result)   # Output: Left rotated: [3, 4, 5, 1, 2]
```

---

## Using Extended Slicing with Step (Not Typical)

```python
def rotate_left_extended_slice(arr, d):
    n = len(arr)
    d = d % n
    # This is essentially the same as original slicing, but written differently
    return arr[d:n] + arr[0:d]

# Example call
arr = [1, 2, 3, 4, 5]
d = 2
result = rotate_left_extended_slice(arr, d)
print("Left rotated:", result)   # Output: Left rotated: [3, 4, 5, 1, 2]
```

---

## Using a Temporary Array and Copying

```python
def rotate_left_temp_array(arr, d):
    n = len(arr)
    d = d % n
    temp = [0] * n
    # Copy elements to new positions
    for i in range(n):
        temp[i] = arr[(i + d) % n]           # place element at rotated index
    return temp

# Example call
arr = [1, 2, 3, 4, 5]
d = 2
result = rotate_left_temp_array(arr, d)
print("Left rotated:", result)   # Output: Left rotated: [3, 4, 5, 1, 2]
```