# Best Time to Buy and Sell Stock II – Detailed Explanation

## Table of Contents

1. [Full Commented Code](#full-commented-code)
2. [Algorithm Description](#algorithm-description)
3. [Step‑by‑Step Walkthrough with ASCII Representation](#step-by-step-walkthrough-with-ascii-representation)
4. [Recursion Tree for Naive Approach](#recursion-tree-for-naive-approach)
5. [From Lower Layer to Higher Layer – Greedy Reasoning](#from-lower-layer-to-higher-layer-greedy-reasoning)
6. [Complexity Analysis](#complexity-analysis)
7. [Example Usage and Output](#example-usage-and-output)
8. [Additional Notes](#additional-notes)

---

## Full Commented Code

```python
def maxProfit(prices):
    """
    Function to return the maximum profit you can achieve by buying and selling stocks.
    You may complete as many transactions as you like (i.e., buy one and sell one share
    of the stock multiple times). However, you must sell before buying again.

    :param prices: List[int] -> List of stock prices per day
    :return: int -> Maximum profit
    """
    # Handle edge case where prices list is empty
    if not prices:
        return 0

    # Initialize total profit
    total_profit = 0

    # Iterate through prices and capture every increasing transaction
    for day_index in range(1, len(prices)):
        # If price increased from previous day, add profit
        if prices[day_index] > prices[day_index - 1]:
            total_profit += prices[day_index] - prices[day_index - 1]

    return total_profit
```

---

## Algorithm Description

The problem is known as **Best Time to Buy and Sell Stock II**. You are given an array `prices` where `prices[i]` is the stock price on day `i`. You may complete **any number of transactions** (buy one, sell one, then buy again, etc.), but you must sell before buying again. The goal is to maximize the total profit.

**Greedy Approach**  
The optimal strategy is to buy and sell on **every consecutive price increase**.  
- If `prices[i] > prices[i-1]`, you can consider that you bought at `prices[i-1]` and sold at `prices[i]`, capturing the difference as profit.  
- Summing all such positive differences yields the maximum total profit.  

This works because any profit obtained by holding a stock over a longer period can be broken down into a sum of smaller daily profits, and the greedy accumulation captures all profitable increments.

---

## Step‑by‑Step Walkthrough with ASCII Representation

Take the example: `prices = [7, 1, 5, 3, 6, 4]`

We iterate from day 1 to day 5 (index 1 to 5) and add any positive difference.

### Initial State
```
Day:         0   1   2   3   4   5
Price:       7   1   5   3   6   4
total_profit = 0
```

### Iteration

**Day 1 (i = 1)**: `prices[1] = 1`, `prices[0] = 7` → `1 - 7 = -6` (not > 0) → no addition.
```
total_profit = 0
```

**Day 2 (i = 2)**: `prices[2] = 5`, `prices[1] = 1` → `5 - 1 = 4` (positive) → add 4.
```
total_profit = 0 + 4 = 4
```

**Day 3 (i = 3)**: `prices[3] = 3`, `prices[2] = 5` → `3 - 5 = -2` → no addition.
```
total_profit = 4
```

**Day 4 (i = 4)**: `prices[4] = 6`, `prices[3] = 3` → `6 - 3 = 3` (positive) → add 3.
```
total_profit = 4 + 3 = 7
```

**Day 5 (i = 5)**: `prices[5] = 4`, `prices[4] = 6` → `4 - 6 = -2` → no addition.
```
total_profit = 7
```

### ASCII Workflow

```
Day:       0   1   2   3   4   5
Price:     7   1   5   3   6   4
Diff:        -6   4  -2   3  -2
Add?           n   y   n   y   n
Profit: 0 → 0 → 4 → 4 → 7 → 7
```

**Final profit**: 7.

---

## Recursion Tree for Naive Approach

A naive recursive solution would try all possible combinations of buy/sell decisions. For a sequence of `n` days, each day you can either buy, sell, or do nothing, but transactions must be valid (sell after buy). The number of possibilities grows exponentially (O(2ⁿ)).

Consider a small example `prices = [1, 2, 3]`. A recursion tree might look like this (simplified, showing only decisions that affect profit):

```
                                 maxProfit(0, holding=False)
                 /                       |                       \
          buy at 0                     skip 0                   (invalid)
          /       \                    /       \
     sell at 1   skip 1          buy at 1   skip 1
        |          |                |          |
      ...        ...              ...        ...
```

Each path represents a sequence of actions. The tree has overlapping subproblems (e.g., the state at day `i` with `holding` flag) and leads to exponential time. The greedy algorithm avoids this by exploiting the fact that we can break down the optimal profit into a sum of positive daily differences, which can be computed in linear time without recursion.

---

## From Lower Layer to Higher Layer – Greedy Reasoning

**Lower Layer (Daily Differences)**  
Consider the price change between consecutive days:  
`delta_i = prices[i] - prices[i-1]` for i = 1..n-1.

**Building Up**  
If you hold a stock from day `i-1` to day `i`, you gain exactly `delta_i`. Any longer transaction that covers multiple days can be decomposed into a sum of consecutive deltas:

```
profit from day a to day b = (prices[a+1]-prices[a]) + (prices[a+2]-prices[a+1]) + ... + (prices[b]-prices[b-1])
                         = Σ_{k=a+1}^{b} delta_k
```

Thus, **total profit from any sequence of transactions is simply the sum of selected deltas**. Since we are free to choose any set of non‑overlapping intervals, the maximum profit is obtained by taking **all positive deltas** (because taking a negative delta would reduce profit). This is a greedy choice that yields the global optimum.

**Higher Layer (Global Optimal)**  
By summing all positive deltas, we achieve the maximum possible total profit. This is equivalent to capturing every price increase, no matter how small, because they are all additive.

**Visualization** (price graph):

```
Price
  ^
  |    5
  |   / \     6
  |  /   \   / \
  | /     \ /   \
  |/       X     \
  |1       3      \
  |                4
  +-------------------> Day
    0  1  2  3  4  5
```

The greedy algorithm sums the rises (from 1→5, from 3→6), which is the area under the increasing segments.

---

## Complexity Analysis

- **Time Complexity**: O(n) – single pass through the array.
- **Space Complexity**: O(1) – only a few variables used.

---

## Example Usage and Output

### Example 1: Provided example
```python
print(maxProfit([7,1,5,3,6,4]))
# Output: 7
```

### Example 2: Empty list
```python
print(maxProfit([]))
# Output: 0
```

### Example 3: Single element
```python
print(maxProfit([42]))
# Output: 0
```

### Example 4: Strictly increasing
```python
print(maxProfit([1,2,3,4,5]))
# Output: 4   (1→2, 2→3, 3→4, 4→5: sum = 4)
```

### Example 5: Strictly decreasing
```python
print(maxProfit([5,4,3,2,1]))
# Output: 0
```

### Example 6: Multiple ups and downs
```python
print(maxProfit([3,3,5,0,0,3,1,4]))
# Output: 8   (3→5, 0→3, 1→4: 2+3+3 = 8)
```

---

## Additional Notes

- This solution works for the problem where you can buy and sell multiple times **as long as you sell before buying again**. It does **not** hold for the version where only one transaction is allowed (Best Time to Buy and Sell Stock I).
- The greedy approach is optimal because the problem exhibits the property of **optimal substructure** and **greedy choice property** – taking all positive deltas cannot be improved by skipping any positive delta or including any negative one.
- The algorithm does not actually simulate buying and selling; it simply sums the increments, which mathematically yields the same result.