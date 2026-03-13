## Space Complexity of Merge Sort: Recursive vs Iterative

Merge sort is a classic divide-and-conquer sorting algorithm. It works by:
- **Dividing** the unsorted list into \(n\) sublists, each containing one element.
- **Conquering** by repeatedly merging sublists to produce new sorted sublists until there is only one sorted list remaining.

The algorithm can be implemented **recursively (top-down)** or **iteratively (bottom-up)**. Both have the same time complexity \(O(n \log n)\), but their space usage differs slightly due to the recursion stack.

We will analyze the **space complexity** from the ground up, covering mathematical and intuitive explanations, graphical representations, and key terminology.

---

### 1. Recursive Merge Sort (Top-Down)

#### How It Works
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])   # recursive call
    right = merge_sort(arr[mid:])  # recursive call
    return merge(left, right)       # merge sorted halves

def merge(left, right):
    result = []                     # temporary array
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```
**Note:** The above creates new arrays for each recursive call. In practice, to save memory, we often use a single auxiliary array and indices to avoid allocating many small arrays. We'll analyze both.

#### Space Complexity Analysis

**Components of space:**
1. **Input space**: The original array of size \(n\) – common to all algorithms, usually ignored when comparing.
2. **Auxiliary space**: Extra memory needed besides the input.
   - **Temporary arrays during merging**: Each merge step creates a temporary array (or uses a global auxiliary array).
   - **Recursion stack**: Each recursive call adds a stack frame.

We'll consider two common implementations:

##### a) Naive implementation (creating new arrays per call)
- Each recursive call creates new `left` and `right` arrays (slices). At the deepest level of recursion, we have about \(n\) single-element arrays, but they are not all alive simultaneously. Actually, memory usage peaks during the merging phase when many temporary arrays exist. This approach can use \(O(n \log n)\) space because each level of recursion creates arrays that sum to \(n\), and there are \(\log n\) levels, but they are not all live at once (due to stack unwinding). However, the worst-case concurrent memory can be high. The typical analysis assumes a more efficient implementation using a single auxiliary array.

##### b) Efficient implementation (using a single auxiliary array)
Most textbook merge sorts allocate one auxiliary array of size \(n\) at the start and reuse it. The recursion then uses indices to specify subarrays, avoiding many allocations.

- **Auxiliary array**: \(O(n)\) – we need extra space to hold merged elements before copying back.
- **Recursion stack**: Depth of recursion = \(\log_2 n\) (because we split in half each time). Each stack frame stores local variables (e.g., `mid`, maybe references). So stack space = \(O(\log n)\).

**Total auxiliary space** = \(O(n) + O(\log n) = O(n)\) because \(n\) dominates \(\log n\) for large \(n\).

#### Mathematical Derivation

Let \(S(n)\) be the auxiliary space for sorting \(n\) elements.

- For the efficient version with a global auxiliary array:
  - The auxiliary array uses \(n\) units (constant across calls).
  - The recursion stack depth is \(\log_2 n\), each using constant space \(c\).
  - So \(S(n) = n + c \log n = O(n)\).

- For the naive version (if we ignore optimization):
  - At each level of recursion, we might have many temporary arrays. But the recursion is depth-first; at the moment of merging, we have two halves that are already sorted, and we allocate a new array of size equal to their sum. The total allocated over all merges is \(O(n \log n)\), but the maximum **simultaneous** memory could be analyzed: The recursion tree’s peak memory occurs when the leftmost branch has been fully explored and we are merging. Actually, a more precise analysis shows that naive recursive merge sort can use \(O(n \log n)\) space if each merge allocates new arrays and they are not freed until the function returns. However, in languages with garbage collection or if we use explicit deallocation, it might be less. To keep it simple, we focus on the optimized version that uses a single auxiliary array, which is standard.

**Recurrence for stack space only** (ignoring auxiliary array):
Let \(Stack(n)\) be the stack space for input size \(n\).
Each call uses constant space \(s\) (for its frame) and then calls itself twice. The maximum stack depth is the height of the recursion tree, which is \(\log n\). So \(Stack(n) = s \cdot \log n = O(\log n)\).

**Combined**: \(Aux(n) = n + O(\log n) = O(n)\).

#### Non-Mathematical Intuition
- **Auxiliary array**: Imagine you have two sorted piles of cards and you want to merge them into one sorted pile. You need an extra table (temporary space) to place the cards in order before moving them back to the original table. The size of that table must be as large as the total number of cards you are merging. Since you eventually merge the entire set, you need a table of size \(n\).
- **Recursion stack**: Think of the recursion as a to-do list. Each time you split, you write down a reminder to come back and merge later. The deepest you go is when you keep splitting until you have single cards. That’s about \(\log n\) levels deep. So you have about \(\log n\) pending reminders. That’s tiny compared to \(n\).
- **Overall**: The big memory hog is the extra table, not the to-do list. So the space is roughly proportional to \(n\).

#### Graphical Representation

**Recursion tree** for \(n=8\):

```
Level 0: [0..7]                (size 8)
         /      \
Level 1: [0..3]  [4..7]        (size 4 each)
        /   \    /   \
Level 2: [0..1][2..3][4..5][6..7] (size 2 each)
        / \   / \   / \   / \
Level 3: [0][1][2][3][4][5][6][7] (size 1 each)
```

**Stack depth** during recursion: The leftmost path is followed first: call on [0..7] → [0..3] → [0..1] → [0] (base case). At that moment, the stack contains 4 frames (levels 0,1,2,3). Then it returns, and we go to [1], etc. The maximum stack depth = number of levels = \( \log_2 8 + 1 = 4\).

**Auxiliary array usage**: During a merge of two subarrays of total size \(k\), we use a temporary array of size \(k\) (or a portion of the global auxiliary array). For example, merging [0,1,2,3] and [4,5,6,7] into a temp array of size 8, then copying back. So at any moment, the temp array holds at most \(n\) elements (the largest merge). Thus, total auxiliary memory = \(n\) (for temp) + stack frames (negligible).

ASCII art of stack at deepest point:

```
Stack (grows downward):
+-----------------+
| frame: merge_sort([0]) |  <- top (executing)
+-----------------+
| frame: merge_sort([0,1]) | waiting for left child
+-----------------+
| frame: merge_sort([0..3]) | waiting
+-----------------+
| frame: merge_sort([0..7]) | waiting (original call)
+-----------------+
```

---

### 2. Iterative Merge Sort (Bottom-Up)

#### How It Works
Instead of recursion, we start by merging adjacent pairs of size 1, then size 2, then size 4, etc., until the whole array is sorted.

```python
def merge_sort_iterative(arr):
    n = len(arr)
    # Create a temporary array of size n
    temp = [0] * n
    width = 1
    while width < n:
        for i in range(0, n, 2*width):
            left = i
            mid = min(i + width, n)
            right = min(i + 2*width, n)
            # merge arr[left:mid] and arr[mid:right] into temp
            merge(arr, temp, left, mid, right)
        # copy temp back to arr (or swap references)
        for i in range(n):
            arr[i] = temp[i]
        width *= 2
    return arr

def merge(arr, temp, left, mid, right):
    i, j, k = left, mid, left
    while i < mid and j < right:
        if arr[i] <= arr[j]:
            temp[k] = arr[i]
            i += 1
        else:
            temp[k] = arr[j]
            j += 1
        k += 1
    while i < mid:
        temp[k] = arr[i]
        i += 1
        k += 1
    while j < right:
        temp[k] = arr[j]
        j += 1
        k += 1
    # Usually we copy back in the main loop; here we just fill temp.
```

#### Space Complexity Analysis

- **Auxiliary array**: We explicitly create a temporary array `temp` of size \(n\). That’s \(O(n)\).
- **No recursion stack**: The algorithm uses loops and a few index variables – constant extra space.
- **Total auxiliary space**: \(O(n) + O(1) = O(n)\).

**Mathematical**: \(S(n) = n + c = O(n)\).

**Non-mathematical**: We still need a separate workspace of size \(n\) to hold merged elements, just like before. But we don't have the overhead of recursive calls, so the constant factor might be slightly smaller. However, the growth rate is still linear in \(n\).

#### Graphical Representation

Iterative process for \(n=8\):

```
Pass 1 (width=1): merge pairs of size 1 into size 2
Indices: merge [0]&[1] into temp[0..1], then [2]&[3] into temp[2..3], etc.

Pass 2 (width=2): merge size-2 blocks into size-4
Merge temp[0..1] & temp[2..3] into arr[0..3] (or vice versa)

Pass 3 (width=4): merge two size-4 blocks into size-8
```

At each pass, the temporary array holds the merged result of the current block. At the end of a pass, we copy back. So the temporary array is always size \(n\).

---

### 3. Why \(n + \log n \approx n\) for Large \(n\)

When we say space complexity is \(O(n)\), we mean that for large input sizes, the term that grows fastest (the linear term) dominates. The logarithmic term \(\log n\) becomes insignificant compared to \(n\). For example, if \(n = 10^6\), \(n + \log_2 n \approx 1,000,000 + 20\), which is essentially 1,000,000. So we drop the lower-order term and constant factors in Big O notation.

Thus, both recursive and iterative merge sort are \(O(n)\) in auxiliary space.

---

### 4. Visual Representation with Minimal ASCII

Let's illustrate the memory usage during a merge step.

**Before merge (two sorted subarrays in original array):**
```
arr: [2, 5, 1, 3, 7, 4, 6, 8]
      left part  right part
Indices 0..3 and 4..7 are sorted internally.
```

**During merge:**
We have a temporary array `temp` of size 8.
We compare elements from both parts and fill `temp` in order.

```
temp: [1, 2, 3, 4, 5, 6, 7, 8]  (after merge)
```

**After merge, we copy `temp` back to `arr` (or swap references).**

At any moment, `temp` holds at most 8 elements. So total auxiliary = 8 (temp) + a few pointers.

**Stack in recursive version:** While merging, the stack has frames for the current call and its ancestors. For example, during the final merge of the whole array, the stack might look like:

```
merge_sort([0..7])   (currently merging left and right)
  - left = merge_sort([0..3]) already returned its sorted array (stored somewhere)
  - right = merge_sort([4..7]) already returned
```

But note: the sorted halves are stored in the original array (if we use in-place merging with auxiliary array). Actually, after recursive calls return, the halves are sorted in the original array. So we don't need extra space for them; only the temporary array is used during merge.

---

### 5. Detailed Walkthrough of a Merge Step

To understand why we need \(O(n)\) auxiliary space, consider merging two sorted subarrays `L` and `R` into a single sorted array. If we try to do it in-place (without extra memory), we would have to shift elements, which could take \(O(n^2)\) time. The standard merge algorithm uses a temporary array to achieve linear time.

**Step-by-step merging of `L = [2,5,8]` and `R = [3,7,9]` (both in `arr` but logically separate):**

1. Create temporary array `temp` large enough to hold the merged result (size 6).
2. Initialize pointers `i` (for L), `j` (for R), `k` (for temp) to 0.
3. Compare `L[i]` (2) and `R[j]` (3): 2 ≤ 3 → place 2 in `temp[k]`, increment `i` and `k`.
4. Compare `L[i]` (5) and `R[j]` (3): 3 ≤ 5 → place 3, increment `j`, `k`.
5. Continue until one list is exhausted.
6. Copy remaining elements from the other list.
7. Now `temp` contains `[2,3,5,7,8,9]`.
8. Copy `temp` back to the original array positions.

The temporary array is essential for this \(O(n)\) process. Without it, we would need to shuffle elements, causing many moves.

---

### 6. Key Terminology

- **Auxiliary space**: Extra memory used by an algorithm excluding the input.
- **Input space**: Memory for storing the input data; usually ignored in complexity analysis because it's common to all solutions.
- **Recursion stack**: Memory used by the system to store function call information (return address, local variables) for each nested call.
- **Stack depth**: The maximum number of nested recursive calls at any point.
- **In-place algorithm**: An algorithm that uses only a small constant amount of extra space (\(O(1)\)). Merge sort is **not** in-place because it requires \(O(n)\) auxiliary space.
- **Stable sort**: Merge sort is stable; equal elements retain their original order.
- **Divide and conquer**: A strategy of breaking a problem into subproblems, solving them, and combining results.
- **Top-down vs bottom-up**: Recursive (top-down) and iterative (bottom-up) are two ways to implement divide and conquer.
- **Temporary array**: A workspace used during merging to hold intermediate results.
- **Big O notation**: Describes the upper bound of growth; for space, it tells how memory scales with input size.

---

### 7. Comparison: Recursive vs Iterative Merge Sort

| Aspect               | Recursive (Top-Down)               | Iterative (Bottom-Up)               |
|----------------------|-------------------------------------|-------------------------------------|
| Auxiliary space      | \(O(n)\) for temp array + \(O(\log n)\) stack = \(O(n)\) | \(O(n)\) for temp array + \(O(1)\) loop vars = \(O(n)\) |
| Stack usage          | \(O(\log n)\) due to recursion      | None                                |
| Constant factors     | Slightly higher due to function calls | Slightly lower, no call overhead   |
| Implementation       | Elegant, easy to understand         | More code, but avoids recursion depth limits |
| Memory fragmentation | May create many small arrays if not optimized | Uses a single auxiliary array |

Both have the same asymptotic space complexity \(O(n)\), but the iterative version may be preferable in environments with limited stack size or when function call overhead is significant.

---

### 8. Can Merge Sort Be In-Place?

There are **in-place merge sort** variants that attempt to merge without extra space, but they are complex and often sacrifice time (e.g., become \(O(n^2)\) in worst case) or stability. The classic, simple, and efficient merge sort uses \(O(n)\) auxiliary space. That's why its space complexity is universally quoted as \(O(n)\).

---

### 9. Summary

- **Merge sort requires \(O(n)\) auxiliary space** because it needs a temporary array of size \(n\) to perform merges efficiently.
- **Recursive version** adds an extra \(O(\log n)\) for the call stack, but this is negligible compared to \(n\).
- **Iterative version** avoids the stack, using only the auxiliary array and a few variables.
- The term \(n + \log n\) simplifies to \(O(n)\) for large \(n\) because the linear term dominates.
- Understanding this helps in choosing algorithms when memory is constrained: if you cannot afford \(O(n)\) extra space, you might prefer an in-place sort like heapsort or quicksort (which can be in-place).
