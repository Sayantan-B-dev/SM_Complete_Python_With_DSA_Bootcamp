## Time Complexity of Fibonacci Number Computation

The Fibonacci sequence is defined as:
- \( F(0) = 0 \)
- \( F(1) = 1 \)
- \( F(n) = F(n-1) + F(n-2) \) for \( n \ge 2 \)

We will analyze the time complexity of two common approaches: the naive recursive algorithm and the efficient iterative algorithm.

---

### 1. Recursive Approach (Naive)

```python
def fib_recursive(n):
    if n <= 1:
        return n
    return fib_recursive(n-1) + fib_recursive(n-2)
```

This implementation makes two recursive calls for each \( n \ge 2 \). Let's analyze its time complexity.

#### 1.1 Recurrence Relation

Let \( T(n) \) be the number of operations (or time) required to compute \( F(n) \). For \( n \le 1 \), the function does constant work: \( T(0) = T(1) = c \) (some constant). For \( n \ge 2 \), we have:

\[
T(n) = T(n-1) + T(n-2) + c
\]

where \( c \) accounts for the addition and the conditional check.

#### 1.2 Solving the Recurrence

This recurrence is similar to the Fibonacci recurrence itself. We can show that \( T(n) \) is proportional to \( F(n) \) (or maybe \( F(n+1) \)). In fact, the number of recursive calls to compute \( F(n) \) is exactly \( 2F(n+1) - 1 \). Since \( F(n) \) grows exponentially ( \( F(n) \approx \phi^n / \sqrt{5} \) where \( \phi \approx 1.618 \) ), the time complexity is exponential.

**Proof by induction**: Let’s define \( R(n) \) as the number of recursive calls (excluding the initial call). Then \( R(0)=R(1)=0 \), and for \( n\ge2 \), \( R(n) = 1 + R(n-1) + R(n-2) \) (the current call plus the calls from the two subcalls). One can show that \( R(n) = 2F(n+1) - 2 \). So total calls including the initial is \( 2F(n+1)-1 \). Since \( F(n+1) = O(\phi^n) \), we have \( T(n) = O(\phi^n) \). For simplicity, we often say it is \( O(2^n) \) because \( \phi^n < 2^n \) and it's easier to see the exponential growth.

#### 1.3 Recursion Tree Visualization

The recursion tree for \( F(5) \) helps understand the exponential growth.

**Tree for n = 5**:

```
                        F(5)
                       /    \
                  F(4)        F(3)
                 /    \       /    \
              F(3)    F(2)   F(2)   F(1)
             /   \    /  \   /  \     |
          F(2)  F(1) F(1)F(0)F(1)F(0) F(1)
         /   \    |   |   |   |   |    |
      F(1)  F(0) (1) (0) (1) (0) (1)  (1)
       |     |
      (1)   (0)
```

Each node represents a recursive call. The number of nodes in the tree for \( F(n) \) is approximately \( 2^n \) (actually a bit less, but still exponential). For \( n=5 \), count the nodes: we have 1 (F5) + 2 (F4,F3) + 4 + ...? Let's list all distinct calls: The tree is not full because leaves appear at different depths. But we can count: The total number of calls for F(5) is 15 (which is 2F(6)-1 = 2*8-1=15). That's less than 2^5=32, but still exponential.

**Key observation**: Each level of the tree roughly doubles the number of nodes, but the tree depth is about n, so total nodes is exponential.

#### 1.4 Mathematical Derivation of Exponential Bound

We can bound \( T(n) \) from above. Since \( T(n) = T(n-1) + T(n-2) + c \), and \( T(n-1) \ge T(n-2) \), we have \( T(n) \le 2T(n-1) + c \). Unfolding this gives \( T(n) \le 2^n T(0) + c(2^{n-1} + ...) = O(2^n) \). Similarly, a lower bound can be obtained by noting \( T(n) \ge 2T(n-2) \), leading to \( T(n) \ge 2^{n/2} \) (also exponential). So indeed it is exponential.

**Conclusion**: The naive recursive Fibonacci has time complexity \( O(\phi^n) \) or \( O(2^n) \) – exponential, which makes it impractical for even moderate n (e.g., n=50 takes billions of calls).

---

### 2. Iterative Approach (Efficient)

```python
def fib_iterative(n):
    if n <= 1:
        return n
    a, b = 0, 1
    for i in range(2, n+1):
        a, b = b, a + b
    return b
```

This algorithm uses a simple loop that runs from 2 to n.

- For each iteration, it does constant work (one addition and two assignments).
- The loop runs exactly \( n-1 \) times (for n≥2).
- Therefore, the time complexity is \( O(n) \), linear.
- Space complexity is \( O(1) \) (only a few variables).

**Comparison**: For n=100, iterative does about 100 operations, while recursive would do about \( \phi^{100} \approx 10^{20} \) operations – completely infeasible.

---

### 3. Recursion with Memoization (Top-Down Dynamic Programming)

We can improve the recursive approach by storing already computed values (memoization). This reduces the complexity to \( O(n) \) because each Fibonacci number is computed only once.

```python
def fib_memo(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fib_memo(n-1, memo) + fib_memo(n-2, memo)
    return memo[n]
```

Now each recursive call is made only once per unique n, so the recursion tree becomes a chain (like a depth-first traversal). The time complexity becomes \( O(n) \) and space \( O(n) \) for the memo dictionary.

---

### 4. Summary Table

| Approach          | Time Complexity | Space Complexity | Notes                               |
|-------------------|-----------------|------------------|-------------------------------------|
| Naive Recursive   | \( O(\phi^n) \) (exponential) | \( O(n) \) (call stack depth) | Impractical for large n |
| Iterative         | \( O(n) \)      | \( O(1) \)       | Best for most purposes              |
| Memoized Recursive| \( O(n) \)      | \( O(n) \)       | Good if recursion is preferred      |

---

### 5. Why the Recursion Tree Gives Exponential Growth

In the recursion tree for \( F(n) \), each node (except leaves) has two children. The number of nodes in a full binary tree of height \( n \) would be \( 2^{n+1}-1 \). Our tree is not full because some branches terminate earlier (when reaching base cases). However, the number of leaves is \( F(n+1) \) (the number of base calls), which is exponential. Since each internal node also contributes, the total number of nodes is roughly proportional to the number of leaves, hence also exponential. More precisely, the total number of calls is \( 2F(n+1)-1 \), which is \( \Theta(\phi^n) \).

**Visual proof**: For \( n=5 \), we had 15 calls. For \( n=6 \), it would be 25? Actually F(7)=13, so 2*13-1=25. So it grows rapidly.

---

### 6. Key Takeaway

The naive recursive Fibonacci is a classic example of how an innocent-looking recurrence can lead to exponential time due to massive recomputation. The iterative (or memoized) version eliminates this redundancy, achieving linear time. This illustrates the importance of algorithm design and complexity analysis.