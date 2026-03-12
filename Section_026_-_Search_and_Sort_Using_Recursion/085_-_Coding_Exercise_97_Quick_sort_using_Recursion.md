## 1. Classic Lomuto Partition QuickSort

### Explanation
This is the standard recursive QuickSort using Lomuto's partition scheme. The pivot is chosen as the last element. The partition function rearranges the array so that all elements less than the pivot come before it, and all greater come after. It returns the final index of the pivot. Then the algorithm recursively sorts the left and right subarrays.

### Approach
- Choose the last element as pivot.
- Scan from left to right, keeping track of the boundary between smaller and larger elements.
- Swap elements smaller than pivot into the left region.
- Finally place the pivot in its correct position.

### Algorithm
1. If `s >= e`, return.
2. Call `partition(arr, s, e)` to get pivot index `p`.
3. Recursively sort `arr[s..p-1]` and `arr[p+1..e]`.

### Pseudocode
```
partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j = low to high-1:
        if arr[j] < pivot:
            i++
            swap arr[i] with arr[j]
    swap arr[i+1] with arr[high]
    return i+1

quickSort(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)
        quickSort(arr, low, pi-1)
        quickSort(arr, pi+1, high)
```

### Code
```python
def partition(arr, low, high):
    # Pivot is the last element
    pivot = arr[high]
    # Index of smaller element (indicates right position of pivot)
    i = low - 1

    # Traverse through all elements
    for j in range(low, high):
        # If current element is smaller than pivot
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]  # Swap

    # Place pivot in correct position
    arr[i+1], arr[high] = arr[high], arr[i+1]
    return i + 1

def quick_sort(arr, low, high):
    # Base case: if subarray has 0 or 1 element
    if low < high:
        # pi is partitioning index, arr[pi] is now at right place
        pi = partition(arr, low, high)

        # Recursively sort elements before and after partition
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)

# Example usage
arr = [3, 6, 7, 2, 1, 4, 5]
quick_sort(arr, 0, len(arr) - 1)
print("Sorted:", arr)  # Output: [1, 2, 3, 4, 5, 6, 7]
```

---

## 2. Hoare Partition QuickSort

### Explanation
Hoare's partition scheme is more efficient than Lomuto because it does fewer swaps on average. It uses two pointers moving from both ends towards each other, swapping elements that are on the wrong side of the pivot. The pivot is typically chosen as the first element.

### Approach
- Choose the first element as pivot (or any element).
- Initialize `i = low-1`, `j = high+1`.
- Loop infinitely: move `i` right until `arr[i] >= pivot`, move `j` left until `arr[j] <= pivot`. If `i >= j`, return `j`. Otherwise swap `arr[i]` and `arr[j]`.
- Recursively sort left part (`low..p`) and right part (`p+1..high`).

### Algorithm
1. If `low >= high`, return.
2. Perform Hoare partition to get split index `p`.
3. Recursively sort `arr[low..p]` and `arr[p+1..high]`.

### Pseudocode
```
partition(arr, low, high):
    pivot = arr[low]
    i = low - 1
    j = high + 1
    while True:
        do i++ while arr[i] < pivot
        do j-- while arr[j] > pivot
        if i >= j:
            return j
        swap arr[i] with arr[j]

quickSort(arr, low, high):
    if low < high:
        p = partition(arr, low, high)
        quickSort(arr, low, p)
        quickSort(arr, p+1, high)
```

### Code
```python
def partition_hoare(arr, low, high):
    pivot = arr[low]  # Choose first element as pivot
    i = low - 1
    j = high + 1

    while True:
        # Move i to right until element >= pivot
        i += 1
        while arr[i] < pivot:
            i += 1

        # Move j to left until element <= pivot
        j -= 1
        while arr[j] > pivot:
            j -= 1

        # If pointers crossed, partition is done
        if i >= j:
            return j

        # Swap elements at i and j
        arr[i], arr[j] = arr[j], arr[i]

def quick_sort_hoare(arr, low, high):
    if low < high:
        # p is the partition index
        p = partition_hoare(arr, low, high)
        # Recursively sort left and right parts
        quick_sort_hoare(arr, low, p)
        quick_sort_hoare(arr, p + 1, high)

# Example usage
arr = [3, 6, 7, 2, 1, 4, 5]
quick_sort_hoare(arr, 0, len(arr) - 1)
print("Sorted:", arr)  # Output: [1, 2, 3, 4, 5, 6, 7]
```

---

## 3. Three-Way Partition (Dutch National Flag) QuickSort

### Explanation
When an array contains many duplicate keys, traditional QuickSort can be inefficient. Three-way partition divides the array into three parts: elements less than pivot, equal to pivot, and greater than pivot. This is also known as Dutch national flag partitioning.

### Approach
- Use two pointers: `lt` for less-than region, `gt` for greater-than region, and `i` for current element.
- While `i <= gt`:
  - If `arr[i] < pivot`: swap with `lt`, increment `lt` and `i`.
  - If `arr[i] > pivot`: swap with `gt`, decrement `gt`.
  - If `arr[i] == pivot`: increment `i`.
- Recursively sort left part (`low..lt-1`) and right part (`gt+1..high`).

### Algorithm
1. If `low >= high`, return.
2. Perform three-way partition to get `lt` and `gt`.
3. Recursively sort `arr[low..lt-1]` and `arr[gt+1..high]`.

### Pseudocode
```
partition3way(arr, low, high):
    pivot = arr[low]  # can also choose random
    lt = low
    i = low
    gt = high
    while i <= gt:
        if arr[i] < pivot:
            swap arr[lt] with arr[i]
            lt++; i++
        elif arr[i] > pivot:
            swap arr[i] with arr[gt]
            gt--
        else:
            i++
    return lt, gt

quickSort3way(arr, low, high):
    if low < high:
        lt, gt = partition3way(arr, low, high)
        quickSort3way(arr, low, lt-1)
        quickSort3way(arr, gt+1, high)
```

### Code
```python
def partition_3way(arr, low, high):
    pivot = arr[low]  # Choose first as pivot
    lt = low      # arr[low..lt-1] < pivot
    i = low       # arr[lt..i-1] == pivot
    gt = high     # arr[gt+1..high] > pivot

    while i <= gt:
        if arr[i] < pivot:
            arr[lt], arr[i] = arr[i], arr[lt]
            lt += 1
            i += 1
        elif arr[i] > pivot:
            arr[i], arr[gt] = arr[gt], arr[i]
            gt -= 1
        else:  # arr[i] == pivot
            i += 1
    # Return boundaries of equal region
    return lt, gt

def quick_sort_3way(arr, low, high):
    if low < high:
        lt, gt = partition_3way(arr, low, high)
        # Recursively sort elements less than pivot and greater than pivot
        quick_sort_3way(arr, low, lt - 1)
        quick_sort_3way(arr, gt + 1, high)

# Example usage with duplicates
arr = [4, 9, 4, 4, 1, 9, 4, 4, 9, 4, 4, 1, 4]
quick_sort_3way(arr, 0, len(arr) - 1)
print("Sorted:", arr)  # Output: [1, 1, 4, 4, 4, 4, 4, 4, 4, 4, 9, 9, 9]
```

---

## 4. Randomized Pivot QuickSort

### Explanation
To avoid the worst-case O(n²) time complexity on already sorted arrays, we can choose the pivot randomly. This makes the algorithm expected O(n log n) for all inputs.

### Approach
- In the partition function, randomly select an index between `low` and `high` and swap its value with the last element (or first) before partitioning.
- Then proceed with standard Lomuto or Hoare partition.

### Algorithm
1. If `low < high`:
   - Generate random index `rand_idx` in [low, high].
   - Swap `arr[rand_idx]` with `arr[high]` (if using Lomuto with last pivot).
   - Call partition to get pivot index.
   - Recursively sort left and right.

### Pseudocode
```
randomPartition(arr, low, high):
    r = random(low, high)
    swap arr[r] with arr[high]
    return partition(arr, low, high)  # using Lomuto

quickSortRandom(arr, low, high):
    if low < high:
        pi = randomPartition(arr, low, high)
        quickSortRandom(arr, low, pi-1)
        quickSortRandom(arr, pi+1, high)
```

### Code
```python
import random

def partition_lomuto(arr, low, high):
    # Standard Lomuto partition (last element as pivot)
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i+1], arr[high] = arr[high], arr[i+1]
    return i + 1

def random_partition(arr, low, high):
    # Choose random index and swap with last element
    rand_idx = random.randint(low, high)
    arr[rand_idx], arr[high] = arr[high], arr[rand_idx]
    # Now partition using last element as pivot
    return partition_lomuto(arr, low, high)

def quick_sort_random(arr, low, high):
    if low < high:
        pi = random_partition(arr, low, high)
        quick_sort_random(arr, low, pi - 1)
        quick_sort_random(arr, pi + 1, high)

# Example usage
arr = [3, 6, 7, 2, 1, 4, 5]
quick_sort_random(arr, 0, len(arr) - 1)
print("Sorted:", arr)  # Output: [1, 2, 3, 4, 5, 6, 7]
```

---

## 5. Iterative QuickSort (Using Stack)

### Explanation
Recursive QuickSort can cause stack overflow for large arrays. An iterative version uses an explicit stack to store subarray boundaries, simulating the recursion.

### Approach
- Push initial `(low, high)` onto a stack.
- While stack not empty, pop boundaries.
- Partition the subarray.
- Push left and right subarray boundaries if they have more than one element.

### Algorithm
1. Initialize stack with `(0, n-1)`.
2. While stack not empty:
   - Pop `(low, high)`.
   - If `low < high`:
     - Get pivot index `p` from partition.
     - Push `(low, p-1)` if `low < p-1`.
     - Push `(p+1, high)` if `p+1 < high`.

### Pseudocode
```
iterativeQuickSort(arr):
    stack = [(0, len(arr)-1)]
    while stack:
        low, high = stack.pop()
        if low < high:
            p = partition(arr, low, high)
            if low < p-1: stack.append((low, p-1))
            if p+1 < high: stack.append((p+1, high))
```

### Code
```python
def partition(arr, low, high):
    # Lomuto partition
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i+1], arr[high] = arr[high], arr[i+1]
    return i + 1

def quick_sort_iterative(arr):
    # Stack stores tuples (low, high)
    stack = [(0, len(arr) - 1)]

    while stack:
        low, high = stack.pop()
        if low < high:
            # Partition the current subarray
            p = partition(arr, low, high)

            # Push left subarray if it has more than one element
            if low < p - 1:
                stack.append((low, p - 1))
            # Push right subarray if it has more than one element
            if p + 1 < high:
                stack.append((p + 1, high))

# Example usage
arr = [3, 6, 7, 2, 1, 4, 5]
quick_sort_iterative(arr)
print("Sorted:", arr)  # Output: [1, 2, 3, 4, 5, 6, 7]
```

---

## 6. Tail Recursive QuickSort

### Explanation
QuickSort typically makes two recursive calls. To reduce stack depth, we can optimize the second call by making it a tail call. In many languages, tail recursion can be optimized, but Python doesn't do that. However, we can manually convert it to iterative for the larger subarray.

### Approach
- After partitioning, identify the smaller and larger subarrays.
- Recursively sort the smaller one first.
- Then update parameters to sort the larger one iteratively (by reassigning `low` or `high` and looping).

### Algorithm
1. While `low < high`:
   - Partition to get `p`.
   - If `p - low < high - p` (left smaller):
     - Recursively sort left.
     - Set `low = p + 1` to sort right iteratively.
   - Else:
     - Recursively sort right.
     - Set `high = p - 1` to sort left iteratively.

### Pseudocode
```
tailRecursiveQuickSort(arr, low, high):
    while low < high:
        p = partition(arr, low, high)
        if p - low < high - p:
            tailRecursiveQuickSort(arr, low, p-1)
            low = p+1
        else:
            tailRecursiveQuickSort(arr, p+1, high)
            high = p-1
```

### Code
```python
def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i+1], arr[high] = arr[high], arr[i+1]
    return i + 1

def quick_sort_tail_recursive(arr, low, high):
    while low < high:
        p = partition(arr, low, high)

        # Sort the smaller subarray recursively
        if p - low < high - p:
            # Left subarray is smaller
            quick_sort_tail_recursive(arr, low, p - 1)
            low = p + 1  # Now sort right subarray iteratively
        else:
            # Right subarray is smaller
            quick_sort_tail_recursive(arr, p + 1, high)
            high = p - 1  # Now sort left subarray iteratively

# Example usage
arr = [3, 6, 7, 2, 1, 4, 5]
quick_sort_tail_recursive(arr, 0, len(arr) - 1)
print("Sorted:", arr)  # Output: [1, 2, 3, 4, 5, 6, 7]
```

---

## 7. QuickSort for Linked Lists

### Explanation
QuickSort can be applied to singly linked lists. Since we don't have random access, we need to partition by rearranging nodes. This variant uses the first node as pivot and builds three lists: less, equal, greater, then recursively sorts and concatenates.

### Approach
- Base case: if list is empty or has one node, return it.
- Choose head as pivot.
- Traverse the rest, appending nodes to less, equal, or greater lists based on value.
- Recursively sort less and greater.
- Concatenate: less + equal + greater.

### Algorithm
1. If head is None or head.next is None: return head.
2. pivot = head.val.
3. Create three dummy heads for less, equal, greater lists.
4. Traverse list, compare each node's value with pivot, and attach to appropriate list.
5. Recursively sort less and greater.
6. Concatenate sorted less, equal, and sorted greater.

### Pseudocode
```
quickSortList(head):
    if not head or not head.next: return head
    pivot = head.val
    lessHead = lessTail = ListNode(0)
    equalHead = equalTail = ListNode(0)
    greaterHead = greaterTail = ListNode(0)
    curr = head
    while curr:
        if curr.val < pivot:
            lessTail.next = curr; lessTail = curr
        elif curr.val > pivot:
            greaterTail.next = curr; greaterTail = curr
        else:
            equalTail.next = curr; equalTail = curr
        curr = curr.next
    lessTail.next = greaterTail.next = equalTail.next = None
    sortedLess = quickSortList(lessHead.next)
    sortedGreater = quickSortList(greaterHead.next)
    return concatenate(sortedLess, equalHead.next, sortedGreater)
```

### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def quick_sort_list(head):
    # Base case
    if not head or not head.next:
        return head

    pivot = head.val
    # Dummy heads for three partitions
    less_head = ListNode(0)
    less_tail = less_head
    equal_head = ListNode(0)
    equal_tail = equal_head
    greater_head = ListNode(0)
    greater_tail = greater_head

    # Traverse and partition
    curr = head
    while curr:
        if curr.val < pivot:
            less_tail.next = curr
            less_tail = curr
        elif curr.val > pivot:
            greater_tail.next = curr
            greater_tail = curr
        else:
            equal_tail.next = curr
            equal_tail = curr
        curr = curr.next

    # Terminate each list
    less_tail.next = None
    equal_tail.next = None
    greater_tail.next = None

    # Recursively sort less and greater
    sorted_less = quick_sort_list(less_head.next)
    sorted_greater = quick_sort_list(greater_head.next)

    # Concatenate: sorted_less + equal + sorted_greater
    return concatenate(sorted_less, equal_head.next, sorted_greater)

def concatenate(less, equal, greater):
    # Helper to concatenate three lists
    dummy = ListNode(0)
    tail = dummy

    # Append less
    tail.next = less
    if less:
        while tail.next:
            tail = tail.next
    # Append equal
    tail.next = equal
    if equal:
        while tail.next:
            tail = tail.next
    # Append greater
    tail.next = greater
    return dummy.next

# Helper functions for testing
def list_to_array(head):
    arr = []
    while head:
        arr.append(head.val)
        head = head.next
    return arr

def array_to_list(arr):
    dummy = ListNode(0)
    tail = dummy
    for val in arr:
        tail.next = ListNode(val)
        tail = tail.next
    return dummy.next

# Example usage
arr = [3, 6, 7, 2, 1, 4, 5]
head = array_to_list(arr)
sorted_head = quick_sort_list(head)
print("Sorted linked list:", list_to_array(sorted_head))  # Output: [1, 2, 3, 4, 5, 6, 7]
```

---

## 8. Median-of-Three Pivot Selection

### Explanation
Choosing the median of the first, middle, and last elements as pivot improves the chance of balanced partitions, especially for partially sorted arrays. This reduces the probability of worst-case behavior.

### Approach
- Find median of `arr[low]`, `arr[mid]`, `arr[high]`.
- Swap the median value with the last element (or first) before partitioning.
- Then proceed with standard partition.

### Algorithm
1. In partition function, compute `mid = (low + high)//2`.
2. Compare `arr[low]`, `arr[mid]`, `arr[high]` and choose the median.
3. Swap median with `arr[high]`.
4. Perform Lomuto partition.

### Pseudocode
```
medianOfThree(arr, low, high):
    mid = (low + high)//2
    if arr[low] > arr[mid]: swap arr[low] with arr[mid]
    if arr[low] > arr[high]: swap arr[low] with arr[high]
    if arr[mid] > arr[high]: swap arr[mid] with arr[high]
    # Now arr[mid] is median, swap with high
    swap arr[mid] with arr[high]
    return arr[high]  # pivot

partitionMedian(arr, low, high):
    pivot = medianOfThree(arr, low, high)
    # proceed with Lomuto
```

### Code
```python
def median_of_three(arr, low, high):
    mid = (low + high) // 2
    # Sort low, mid, high so that arr[mid] becomes median
    if arr[low] > arr[mid]:
        arr[low], arr[mid] = arr[mid], arr[low]
    if arr[low] > arr[high]:
        arr[low], arr[high] = arr[high], arr[low]
    if arr[mid] > arr[high]:
        arr[mid], arr[high] = arr[high], arr[mid]
    # Now median is at mid, swap with high for Lomuto
    arr[mid], arr[high] = arr[high], arr[mid]
    return arr[high]  # pivot

def partition_median(arr, low, high):
    pivot = median_of_three(arr, low, high)
    i = low - 1
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i+1], arr[high] = arr[high], arr[i+1]
    return i + 1

def quick_sort_median(arr, low, high):
    if low < high:
        pi = partition_median(arr, low, high)
        quick_sort_median(arr, low, pi - 1)
        quick_sort_median(arr, pi + 1, high)

# Example usage
arr = [3, 6, 7, 2, 1, 4, 5]
quick_sort_median(arr, 0, len(arr) - 1)
print("Sorted:", arr)  # Output: [1, 2, 3, 4, 5, 6, 7]
```

---

## 9. Hybrid QuickSort with Insertion Sort for Small Subarrays

### Explanation
For small subarrays (e.g., size ≤ 15), insertion sort is faster due to lower overhead. This hybrid approach switches to insertion sort when the subarray size falls below a threshold, improving practical performance.

### Approach
- In the recursive function, if `high - low + 1 <= THRESHOLD`, run insertion sort on that subarray.
- Otherwise, perform partition and recurse.

### Algorithm
1. If `low >= high`, return.
2. If `high - low + 1 <= THRESHOLD`, call `insertion_sort(arr, low, high)`.
3. Else:
   - Partition to get `p`.
   - Recursively sort left and right.

### Pseudocode
```
insertionSort(arr, low, high):
    for i = low+1 to high:
        key = arr[i]
        j = i-1
        while j >= low and arr[j] > key:
            arr[j+1] = arr[j]
            j--
        arr[j+1] = key

quickSortHybrid(arr, low, high):
    if low < high:
        if high - low + 1 <= THRESHOLD:
            insertionSort(arr, low, high)
        else:
            p = partition(arr, low, high)
            quickSortHybrid(arr, low, p-1)
            quickSortHybrid(arr, p+1, high)
```

### Code
```python
THRESHOLD = 5  # Small for demonstration

def insertion_sort(arr, low, high):
    for i in range(low + 1, high + 1):
        key = arr[i]
        j = i - 1
        while j >= low and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key

def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i+1], arr[high] = arr[high], arr[i+1]
    return i + 1

def quick_sort_hybrid(arr, low, high):
    if low < high:
        # If subarray size is small, use insertion sort
        if high - low + 1 <= THRESHOLD:
            insertion_sort(arr, low, high)
        else:
            pi = partition(arr, low, high)
            quick_sort_hybrid(arr, low, pi - 1)
            quick_sort_hybrid(arr, pi + 1, high)

# Example usage
arr = [3, 6, 7, 2, 1, 4, 5, 9, 8, 0]
quick_sort_hybrid(arr, 0, len(arr) - 1)
print("Sorted:", arr)  # Output: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

---

## 10. Parallel QuickSort (using multiprocessing)

### Explanation
QuickSort's divide-and-conquer nature makes it suitable for parallelization. This variant uses Python's `multiprocessing` to sort subarrays concurrently. However, due to overhead, it's efficient only for large arrays.

### Approach
- Recursively split the array into two halves after partitioning.
- If the subarray size is above a threshold, spawn a process to sort one half while the current process sorts the other.
- Use `multiprocessing.Pool` or `Process` for parallel execution.

### Algorithm
1. If `low < high`:
   - Partition to get `p`.
   - If size is large enough (e.g., > 1000) and we have available cores:
     - Create two processes to sort left and right concurrently.
   - Else, sort recursively in the same process.

### Pseudocode
```
parallelQuickSort(arr, low, high, depth):
    if low < high:
        p = partition(arr, low, high)
        if depth < maxDepth and (high-low) > THRESHOLD:
            with Pool(2) as pool:
                pool.starmap(parallelQuickSort, [(arr, low, p-1, depth+1), (arr, p+1, high, depth+1)])
        else:
            parallelQuickSort(arr, low, p-1, depth+1)
            parallelQuickSort(arr, p+1, high, depth+1)
```

### Code
```python
import multiprocessing as mp
import math

def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i+1], arr[high] = arr[high], arr[i+1]
    return i + 1

def parallel_quick_sort(arr, low, high, depth=0, max_depth=2):
    if low < high:
        pi = partition(arr, low, high)
        # If depth is less than max depth and subarrays are large enough, parallelize
        if depth < max_depth and (high - low) > 10000:
            # Create processes for left and right
            left_proc = mp.Process(target=parallel_quick_sort, args=(arr, low, pi-1, depth+1, max_depth))
            right_proc = mp.Process(target=parallel_quick_sort, args=(arr, pi+1, high, depth+1, max_depth))
            left_proc.start()
            right_proc.start()
            left_proc.join()
            right_proc.join()
        else:
            # Sort sequentially
            parallel_quick_sort(arr, low, pi-1, depth+1, max_depth)
            parallel_quick_sort(arr, pi+1, high, depth+1, max_depth)

def sort_parallel(arr):
    parallel_quick_sort(arr, 0, len(arr)-1, max_depth=mp.cpu_count())

# Example usage (requires large array to see benefit)
if __name__ == "__main__":
    import random
    arr = [random.randint(0, 1000) for _ in range(10000)]
    sort_parallel(arr)
    print("Sorted (first 10):", arr[:10])  # Check first 10 elements
```