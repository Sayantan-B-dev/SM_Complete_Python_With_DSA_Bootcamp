### Largest Rectangle Area

```python
def largestRectangleArea(heights):
    stack = []
    max_area = 0
    heights.append(0)
    for i in range(len(heights)):
        while stack and heights[i] < heights[stack[-1]]:
            h = heights[stack.pop()]
            w = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, h * w)
        stack.append(i)
    return max_area
```
## Largest Rectangle in Histogram Using Monotonic Stack

### Problem Structure

Given an array `heights`, each element represents the height of a histogram bar with **unit width**.
The objective is to determine the **largest rectangular area** that can be formed within the histogram.

Example histogram:

```
heights = [2,1,5,6,2,3]
```

The largest rectangle area here is **10**, formed by bars `5` and `6`.

### Core Algorithmic Insight

For each bar, the maximum rectangle that can be formed using that bar as the **smallest height** extends:

* **Left** until a smaller bar appears.
* **Right** until a smaller bar appears.

A **monotonic increasing stack** efficiently tracks bars whose rectangles are still expandable.

Stack stores **indices** of bars so width can be calculated.

### Key Computation

When encountering a bar shorter than the stack top:

1. Pop the stack.
2. Treat the popped bar as the **height of a rectangle**.
3. Determine width using current index and previous smaller index.

Width formula:

```
width = current_index - previous_smaller_index - 1
```

If the stack becomes empty:

```
width = current_index
```

### Complexity Analysis

| Metric           | Value                                  |
| ---------------- | -------------------------------------- |
| Time Complexity  | `O(n)` each bar pushed and popped once |
| Space Complexity | `O(n)` stack storage                   |

---

## Implementation

```python
def largestRectangleArea(heights):
    """
    Computes the maximum rectangle area that can be formed
    within a histogram using a monotonic stack approach.

    Parameters
    ----------
    heights : List[int]
        List representing heights of histogram bars.

    Returns
    -------
    int
        Maximum rectangular area within the histogram.
    """

    # Stack will store indices of bars
    stack = []

    # Maximum rectangle area encountered so far
    max_area = 0

    # Append a sentinel bar of height zero
    # This guarantees all remaining bars in stack are processed
    heights.append(0)

    # Iterate through every bar index
    for current_index in range(len(heights)):

        # While current bar is smaller than stack top bar
        # rectangles for taller bars must be finalized
        while stack and heights[current_index] < heights[stack[-1]]:

            # Height of the rectangle determined by popped bar
            height_of_rectangle = heights[stack.pop()]

            # Determine the width of rectangle
            if not stack:
                width_of_rectangle = current_index
            else:
                width_of_rectangle = current_index - stack[-1] - 1

            # Compute rectangle area
            area = height_of_rectangle * width_of_rectangle

            # Update maximum area if necessary
            max_area = max(max_area, area)

        # Push current index to stack
        stack.append(current_index)

    return max_area
```

---

## Expected Output Demonstration

```python
heights = [2,1,5,6,2,3]

print(largestRectangleArea(heights))
```

### Expected Output

```
10
```

### Histogram Area Interpretation

| Bar Heights | Possible Rectangle    | Area |
| ----------- | --------------------- | ---- |
| `[5,6]`     | width `2`, height `5` | `10` |
| `[2,1,5,6]` | width `4`, height `1` | `4`  |
| `[2]`       | width `1`, height `2` | `2`  |

The rectangle formed by bars **5 and 6** yields the **maximum area of 10**.
