# Number of Balanced Binary Trees of Given Height

## Problem Definition

A **binary tree** is *balanced* if for every node, the heights of its left and right subtrees differ by at most 1.  
Given a height `h` (where height is defined as the number of nodes on the longest path from root to leaf, with a single node tree having height 1), we want to count the number of **distinct balanced binary trees** that have exactly height `h`.

An empty tree (height 0) is considered a valid balanced tree, and it is counted as 1.

---

## Recurrence Relation – The Heart of the Solution

Let `count[h]` denote the number of balanced binary trees of height `h`.

### Base Cases
- `count[0] = 1` – the empty tree.
- `count[1] = 1` – a single node.

### For Height `h` (`h ≥ 2`)

A balanced tree of height `h` must have a root with left and right subtrees that are themselves balanced. The heights of these subtrees can only be:

- **Both `h-1`** – because the tallest subtree must be `h-1` (since the root adds one level).  
- **One of them `h-1` and the other `h-2`** – this still keeps the height difference ≤ 1.

No other combination is possible: if both subtrees were `h-2`, the overall tree height would be `h-1`; if one were `h-1` and the other `h-1`, that’s the first case; if one were `h-1` and the other `h-2`, that’s the second case; if one were `h-2` and the other `h-3`, the height would be at most `h-1` (since the taller subtree would be `h-2`, adding root gives `h-1`), so it cannot reach height `h`.

Thus we have:

```
count[h] = count[h-1] * count[h-1]   [both subtrees of height h-1]
         + count[h-1] * count[h-2]   [left = h-1, right = h-2]
         + count[h-2] * count[h-1]   [left = h-2, right = h-1]
```

This simplifies to:

```
count[h] = count[h-1]² + 2 * count[h-1] * count[h-2]
```

---

## Visual Explanation – Building the Formula

Consider a root. Its left and right subtrees are independent balanced trees. Let `A = count[h-1]` and `B = count[h-2]`.

### Case 1: Both subtrees have height `h-1`
- Choose any left tree from the `A` possibilities.
- Choose any right tree from the `A` possibilities.
- Total combinations: `A × A = A²`.

### Case 2: Left subtree of height `h-1`, right of height `h-2`
- Left: any of the `A` trees.
- Right: any of the `B` trees.
- Total: `A × B`.

### Case 3: Left subtree of height `h-2`, right of height `h-1`
- Left: `B` possibilities.
- Right: `A` possibilities.
- Total: `B × A = A × B`.

Adding the three disjoint cases gives the formula.

---

## Diagram (for h = 3)

```
Height 3 balanced trees:
Root must have a subtree of height 2 (to reach overall height 3) and the other subtree either height 2 or height 1.

Left=2, Right=2
  count[2]² = 2² = 4

Left=2, Right=1
  count[2] * count[1] = 2 * 1 = 2

Left=1, Right=2
  count[1] * count[2] = 1 * 2 = 2

Total = 4 + 2 + 2 = 8
```

Indeed, the sequence for `h = 0,1,2,3,4` is: 1, 1, 3, 15, 105, … (matching the code’s output).

---

## Three Implementations – A Deep Dive

### 1. Naïve Recursion

This directly translates the recurrence into a recursive function. It is the most straightforward but extremely inefficient because it recomputes the same `count[h]` many times.

#### Algorithm
- Base cases: if `h ≤ 1` return 1.
- Compute `h1 = count(h-1)`, `h2 = count(h-2)`.
- Return `h1*h1 + 2*h1*h2`.

#### Time Complexity
`O(2^h)` – each call spawns two recursive calls, leading to exponential growth.  
For `h=30`, this becomes impractical.

#### Space Complexity
`O(h)` due to the recursion stack.

#### Pros & Cons
- **Pros:** Simple, follows mathematical definition.
- **Cons:** Unusable for moderate `h`.

---

### 2. Memoization (Top‑Down Dynamic Programming)

We store already computed results in a memo array (or dictionary). Before recursing, we check if the value is already available; if yes, we return it immediately.

#### Algorithm
- Create a memo array of size `h+1` initialized with `-1` (or `None`).
- In the recursive function:
  - Base cases as before.
  - If `memo[h] != -1`, return it.
  - Compute `h1` and `h2` by calling the memoized function recursively.
  - Store the result in `memo[h]` and return it.

#### Time Complexity
`O(h)` – each distinct height is computed exactly once; each computation involves a few arithmetic operations and constant‑time recursive calls (which hit the memo).

#### Space Complexity
`O(h)` for the memo array, plus `O(h)` recursion stack in the worst case.

#### Pros & Cons
- **Pros:** Dramatically faster than naïve recursion; still preserves the recursive structure.
- **Cons:** Recursion depth may become an issue for very large `h` (though `h` is typically small here, as the numbers grow super‑exponentially). Also requires explicit memo management.

---

### 3. Tabulation (Bottom‑Up Dynamic Programming)

We build the table iteratively from the smallest height up to the target. This eliminates recursion entirely.

#### Algorithm
- Create a `dp` array of length `h+1`.
- Initialize `dp[0] = 1`, `dp[1] = 1`.
- For `i` from `2` to `h`:
  - `dp[i] = dp[i-1] * dp[i-1] + 2 * dp[i-1] * dp[i-2]`
- Return `dp[h]`.

#### Time Complexity
`O(h)` – a single loop.

#### Space Complexity
`O(h)` for the table. (It can be reduced to `O(1)` by keeping only the last two values, because the recurrence only depends on `dp[i-1]` and `dp[i-2]`.)

#### Pros & Cons
- **Pros:** No recursion, very fast, easy to optimise space, no risk of stack overflow.
- **Cons:** Computes all intermediate values even if only one height is needed (which is unavoidable here because they are all needed for the recurrence).

---

## Comparison of the Three Approaches

| Approach          | Time Complexity | Space Complexity | Recursion Depth | Suitable for Large `h` |
|-------------------|-----------------|------------------|-----------------|------------------------|
| Naïve Recursion   | O(2^h)          | O(h)             | O(h)            | No                     |
| Memoization       | O(h)            | O(h)             | O(h)            | Yes, but recursion may limit depth |
| Tabulation        | O(h)            | O(h) / O(1)      | None            | Yes                    |

All three produce the same results (e.g., for `h=5`, the count is 315). The dynamic programming versions turn an exponential problem into a linear one by exploiting overlapping subproblems.

---

## Why the Formula Works – The Human Intuition

Think of building a balanced tree of height `h`:

- The root must have a left child and a right child.
- For the tree to have height exactly `h`, at least one of the subtrees must have height `h-1`. The other subtree can be either `h-1` or `h-2` (because if it were smaller, the height would still be `h-1`, and if it were larger, the tree would be taller than `h`).
- So we have three cases:
  1. Both subtrees height `h-1`.
  2. Left subtree height `h-1`, right `h-2`.
  3. Left `h-2`, right `h-1`.
- The number of ways to choose a subtree of a given height is already known (by induction). Multiplying the independent choices gives the count for each case.
- Adding the three cases yields the recurrence.

This recurrence is the cornerstone of all three implementations.

---

## Conclusion

The problem of counting balanced binary trees of a given height is a classic example of how dynamic programming can be applied to combinatorial counting. The naive recursive solution is elegant but impractical; memoization and tabulation provide efficient linear‑time solutions, each with its own trade‑offs. The recurrence `count[h] = count[h-1]² + 2·count[h-1]·count[h-2]` captures the combinatorial structure and can be visualised through simple case analysis. Understanding this recurrence is key to mastering the problem and serves as a foundation for more complex tree‑counting problems.