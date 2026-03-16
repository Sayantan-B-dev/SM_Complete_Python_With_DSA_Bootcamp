### Find Winner

```python
from collections import deque

def find_winner(n, k):
    q = deque(range(1, n + 1))
    while len(q) > 1:
        for _ in range(k - 1):
            q.append(q.popleft())
        q.popleft()
    return q[0]
```


## Josephus Circle Game Using Queue Rotation

### Problem Structure

`n` friends stand in a circular arrangement numbered from `1` to `n`.
A counting process eliminates every `k`-th friend until only one participant remains.

This is a classical **Josephus problem**, which can be simulated using a **queue** because the circle continuously rotates.

### Queue Simulation Logic

1. Insert all friends into a queue in sequential order.
2. Rotate the queue `k-1` times by moving the front element to the rear.
3. The `k`-th friend becomes the front element and is removed.
4. Continue the process until only one element remains.
5. The last remaining friend is the winner.

Queue rotation simulates circular counting without explicitly building a circular structure.

### Complexity Characteristics

| Metric           | Value                                                      |
| ---------------- | ---------------------------------------------------------- |
| Time Complexity  | `O(n × k)` because each elimination involves `k` rotations |
| Space Complexity | `O(n)` due to queue storage                                |

---

## Implementation

```python
from collections import deque

def find_winner(n, k):
    """
    Simulates the Josephus elimination process using a queue.

    Parameters
    ----------
    n : int
        Total number of friends in the circle.
    k : int
        Every k-th friend gets eliminated.

    Returns
    -------
    int
        The label of the last remaining friend.
    """

    # Initialize a queue containing friends numbered from 1 to n
    friends_queue = deque(range(1, n + 1))

    # Continue the elimination process until only one friend remains
    while len(friends_queue) > 1:

        # Rotate the queue k-1 times
        # Each rotation moves the front friend to the back
        for _ in range(k - 1):
            friends_queue.append(friends_queue.popleft())

        # Remove the k-th friend from the circle
        friends_queue.popleft()

    # The remaining friend is the winner
    return friends_queue[0]
```

---

## Expected Output Demonstration

```python
print(find_winner(5, 2))
```

### Expected Output

```
3
```

### Elimination Sequence Illustration

| Step    | Circle State  | Eliminated |
| ------- | ------------- | ---------- |
| Initial | `[1,2,3,4,5]` | —          |
| 1       | `[2,3,4,5,1]` | `2`        |
| 2       | `[4,5,1,3]`   | `4`        |
| 3       | `[1,3,5]`     | `1`        |
| 4       | `[3,5]`       | `5`        |
| Final   | `[3]`         | Winner     |
