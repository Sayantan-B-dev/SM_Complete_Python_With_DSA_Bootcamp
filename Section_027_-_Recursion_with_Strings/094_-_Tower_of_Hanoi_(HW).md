## 1. Introduction
The Tower of Hanoi puzzle consists of three pegs (rods) and a number of disks of different sizes. Initially, all disks are stacked on one peg (the source) in decreasing size order – the largest at the bottom, smallest at the top.  
**Rules**:
- Only one disk can be moved at a time.
- A larger disk may never be placed on top of a smaller disk.
- The goal is to move the entire stack to another peg (the destination), using the third peg (auxiliary) as a temporary holder.

The puzzle has a simple recursive solution, which is exactly what the given code implements. It prints each move with a step number.

## 2. TODO Explanation
The function `tower_of_hanoi(n, source, destination, auxiliary)` is supposed to print the sequence of moves required to move `n` disks from the `source` peg to the `destination` peg, using the `auxiliary` peg as helper. It numbers each move using a global variable `step`.

## 3. Code Skeleton
```python
step = 1   # global counter for move numbers

def tower_of_hanoi(n, source, destination, auxiliary):
    # Base cases: n == 0 (nothing to do) or n == 1 (single disk move)
    # Recursive case:
    #   1. Move n-1 disks from source to auxiliary
    #   2. Move the largest disk from source to destination
    #   3. Move n-1 disks from auxiliary to destination
```

## 4. Base Cases
```python
if n == 0:
    return
if n == 1:
    print(step, source, '-->', destination)
    step += 1
    return
```
- **`n == 0`**: No disks to move – simply return (do nothing). This handles the case when the function is called with zero disks, though typically we start with n≥1.
- **`n == 1`**: If there's only one disk, we can move it directly from `source` to `destination`. We print the move with the current step number, then increment `step` for the next move.

These base cases stop the recursion.

## 5. Recursive Call (The Heart)
```python
tower_of_hanoi(n-1, source, auxiliary, destination)   # Step A
print(step, source, '-->', destination)                # Step B
step += 1
tower_of_hanoi(n-1, auxiliary, destination, source)    # Step C
```
This is the classic recursive strategy:
- **Step A**: Move the top `n-1` disks from `source` to `auxiliary` (using `destination` as temporary). This is a smaller Tower of Hanoi problem itself.
- **Step B**: Move the largest remaining disk (the bottom one) directly from `source` to `destination`. Print this move.
- **Step C**: Move the `n-1` disks from `auxiliary` to `destination` (using `source` as temporary).

Notice how the roles of the pegs change in the recursive calls – that's the key.

## 6. Recursion Tree (for n = 3)
Let's draw the tree of function calls for `n=3`. We'll label each call with its parameters `(n, source, dest, aux)`. Moves are printed when a call handles a single disk (either as base case or after recursive calls). The tree shows the order of execution.

```
tower_of_hanoi(3, 'source', 'dest', 'aux')
├── tower_of_hanoi(2, 'source', 'aux', 'dest')
│   ├── tower_of_hanoi(1, 'source', 'dest', 'aux')
│   │   └── print: 1 source --> dest
│   ├── print: 2 source --> aux
│   └── tower_of_hanoi(1, 'dest', 'aux', 'source')
│       └── print: 3 dest --> aux
├── print: 4 source --> dest
└── tower_of_hanoi(2, 'aux', 'dest', 'source')
    ├── tower_of_hanoi(1, 'aux', 'source', 'dest')
    │   └── print: 5 aux --> source
    ├── print: 6 aux --> dest
    └── tower_of_hanoi(1, 'source', 'dest', 'aux')
        └── print: 7 source --> dest
```

The moves are printed in the order they occur – that's the exact sequence needed to solve the puzzle.

## 7. Step‑by‑Step Working (Trace for n = 3)
Let's simulate the execution for `n=3`, `source = 'A'`, `destination = 'C'`, `auxiliary = 'B'` (using letters for brevity). The global `step` starts at 1.

1. **Call** `tower_of_hanoi(3, A, C, B)`
   - n=3, so not base case.
   - Calls `tower_of_hanoi(2, A, B, C)` and waits.

2. **Call** `tower_of_hanoi(2, A, B, C)`
   - n=2, not base.
   - Calls `tower_of_hanoi(1, A, C, B)`.

3. **Call** `tower_of_hanoi(1, A, C, B)`
   - n=1 → base case: print `"1 A --> C"`, step becomes 2. Return.

4. Back to `tower_of_hanoi(2, A, B, C)`
   - After recursive call, print `"2 A --> B"`, step becomes 3.
   - Then calls `tower_of_hanoi(1, C, B, A)`.

5. **Call** `tower_of_hanoi(1, C, B, A)`
   - n=1 → print `"3 C --> B"`, step becomes 4. Return.

6. Back to `tower_of_hanoi(2, A, B, C)` – done, return.

7. Back to `tower_of_hanoi(3, A, C, B)`
   - After first recursive call, print `"4 A --> C"`, step becomes 5.
   - Then calls `tower_of_hanoi(2, B, C, A)`.

8. **Call** `tower_of_hanoi(2, B, C, A)`
   - Calls `tower_of_hanoi(1, B, A, C)`.

9. **Call** `tower_of_hanoi(1, B, A, C)`
   - n=1 → print `"5 B --> A"`, step becomes 6. Return.

10. Back to `tower_of_hanoi(2, B, C, A)`
    - Print `"6 B --> C"`, step becomes 7.
    - Calls `tower_of_hanoi(1, A, C, B)`.

11. **Call** `tower_of_hanoi(1, A, C, B)`
    - n=1 → print `"7 A --> C"`, step becomes 8. Return.

12. Back to `tower_of_hanoi(2, B, C, A)` – done, return.

13. Back to `tower_of_hanoi(3, A, C, B)` – done.

The printed output is exactly the 7 moves required to solve Tower of Hanoi with 3 disks.

## 8. The Algorithm
The recursive algorithm for Tower of Hanoi can be stated as:

```
To move n disks from source to destination using auxiliary:
    if n == 1:
        move the single disk from source to destination
    else:
        move n-1 disks from source to auxiliary (using destination as helper)
        move the largest disk from source to destination
        move n-1 disks from auxiliary to destination (using source as helper)
```

**Number of moves**: 2ⁿ – 1 (for n disks).  
**Time complexity**: O(2ⁿ) – exponential, because we generate all moves.

## 9. Use of Loop
You may have noticed: **there is no loop** in the code. The repetition is handled entirely by recursion. Each recursive call essentially acts as a "loop" that processes a smaller instance of the same problem. The recursion tree itself is the control structure.

## 10. Whole Code Commented Detailed
```python
# Global variable to count the moves (step number)
step = 1

def tower_of_hanoi(n, source, destination, auxiliary):
    """
    Print the sequence of moves to move n disks from source peg to destination peg,
    using auxiliary peg as temporary storage. Each move is numbered.
    """
    global step  # we need to modify the global step variable

    # Base case 1: no disks to move – do nothing
    if n == 0:
        return

    # Base case 2: only one disk – move it directly from source to destination
    if n == 1:
        print(step, source, '-->', destination)
        step += 1          # increment step number for the next move
        return

    # Recursive case: more than one disk

    # Step 1: Move the top n-1 disks from source to auxiliary.
    #         In this subproblem, the destination peg acts as the temporary helper.
    tower_of_hanoi(n-1, source, auxiliary, destination)

    # Step 2: Move the largest disk (the remaining one) from source to destination.
    print(step, source, '-->', destination)
    step += 1

    # Step 3: Move the n-1 disks from auxiliary to destination.
    #         This time, source peg acts as the temporary helper.
    tower_of_hanoi(n-1, auxiliary, destination, source)


# Example: Solve Tower of Hanoi for 5 disks
n = 5
tower_of_hanoi(n, 'source', 'destination', 'auxiliary')
```

## 11. Teach It Like I'm 5 (ELI5)
Imagine you have three plates of different sizes stacked on a table (the smallest on top). You want to move the whole stack to another table, but you can only move one plate at a time, and you can never put a bigger plate on top of a smaller one. There's a spare table you can use for temporary stacking.

The trick is to think:
- If I only have one plate, I can just move it directly. Easy!
- If I have two plates, I move the small one to the spare table, move the big one to the destination, then move the small one on top of the big one. Done.
- For three plates, it's the same idea: first move the top two plates to the spare table (using the same method as for two plates), then move the biggest plate to the destination, then move the two plates from the spare to the destination (again using the two‑plate method).

So the puzzle solves itself: to move many plates, you just need to know how to move one fewer plates. That's exactly what the recursive function does – it keeps asking "how do I move a smaller stack?" until it reaches the single‑plate case.

**Extra fun**: The number of moves for 5 disks is 2⁵ – 1 = 31. You can watch an animation of the solution at the link you provided: [Tower of Hanoi Animation](https://yongdanielliang.github.io/animation/web/TowerOfHanoi.html)

---

