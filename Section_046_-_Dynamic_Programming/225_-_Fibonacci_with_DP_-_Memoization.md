```py
calls = {}

diary = [-1] * (50+1)

def fib_with_dp(n):

    if(n<=1):
        return 1 

    if(diary[n] != -1):
        return diary[n]
    
    if(n not in calls):
        calls[n] = 1
    else:
        calls[n]+=1

    fib1 = fib_with_dp(n-1)
    fib2 = fib_with_dp(n-2)

    diary[n-1] = fib1
    diary[n-2] = fib2

    ans = fib1 + fib2
    diary[n] = ans
    return ans


def fibonicci_number_recursive(n):
    if(n<=1):
        return 1
    
    if(n not in calls):
        calls[n] = 1
    else:
        calls[n]+=1
    fib1 = fibonicci_number_recursive(n-1)
    fib2 = fibonicci_number_recursive(n-2)

    return fib1 + fib2

print(fib_with_dp(50))
print(calls)
calls={}
print(fibonicci_number_recursive(50))
print(calls)


```

# Mastering Fibonacci with Memoization: A Step-by-Step Guide to Dynamic Programming

## Table of Contents

1. **Introduction** – What is Fibonacci and why care?
2. **Naïve Recursive Fibonacci** – The elegant but expensive solution.
3. **The Problem: Overlapping Subproblems** – Why naïve recursion is a performance disaster.
4. **Dynamic Programming Concept** – Remember past work (memoization).
5. **Memoized Fibonacci Implementation** – Clean code with a `memo` list.
6. **Step-by-Step Walkthrough** – Tracing the execution for `n=5`.
7. **Recursion Tree Comparison** – ASCII diagrams showing the difference.
8. **Complexity Analysis** – From exponential to linear.
9. **The Power of Memoization** – Dramatic reduction in function calls.
10. **Key Takeaways** – When and how to apply memoization.

---

## 1. Introduction

The Fibonacci sequence is a classic example used to illustrate recursion.  
Definition (as used in the code):  
- `fib(0) = 1`  
- `fib(1) = 1`  
- `fib(n) = fib(n-1) + fib(n-2)` for `n > 1`

While the recursive definition is mathematically elegant, a direct implementation leads to repeated calculations of the same subproblems – exactly the issue dynamic programming (DP) solves.

---

## 2. Naïve Recursive Fibonacci

Here is the straightforward recursive version:

```python
def fib_naive(n):
    if n <= 1:
        return 1
    return fib_naive(n-1) + fib_naive(n-2)
```

This code works, but it is extremely inefficient for moderate `n`.  
To see the redundancy, a counter can be added:

```python
calls = {}
def fib_naive_with_counter(n):
    if n <= 1:
        return 1
    calls[n] = calls.get(n, 0) + 1
    return fib_naive_with_counter(n-1) + fib_naive_with_counter(n-2)
```

Running `fib_naive_with_counter(15)` and printing `calls` shows that `fib(1)` is computed **610 times** – a clear sign of redundancy.

---

## 3. The Problem: Overlapping Subproblems

The recursion tree for `n=5` reveals the issue:

```
                    fib(5)
                   /      \
              fib(4)       fib(3)
             /    \       /    \
        fib(3)   fib(2) fib(2) fib(1)
        /   \    /   \   /   \
   fib(2) fib(1) … … … … … …
   /   \
fib(1) fib(0)
```

Notice how `fib(3)`, `fib(2)`, `fib(1)`, and `fib(0)` appear multiple times. The number of calls grows exponentially: roughly **O(2ⁿ)**. For `n=15`, this is thousands of calls; for `n=50`, it is astronomically large.

---

## 4. Dynamic Programming Concept – Memoization

The idea of **memoization** is simple:  
**Store results of subproblems the first time they are computed, and reuse them whenever the same subproblem appears again.**

A `memo` array (or dictionary) can be used to cache results.  
- Initialize `memo` with `-1` (or `None`) to indicate “not yet computed”.
- Before recursing, check the memo. If the result is already there, return it immediately.
- Otherwise, compute it recursively, store the result in `memo`, and then return it.

This technique transforms the exponential recursion into a linear one because each unique `n` is computed only once.

---

## 5. Memoized Fibonacci Implementation

Here is a clean version using a list as a memo:

```python
def fib_memo(n, memo):
    if n <= 1:
        return 1
    if memo[n] != -1:          # already computed?
        return memo[n]
    # Compute and store
    result = fib_memo(n-1, memo) + fib_memo(n-2, memo)
    memo[n] = result
    return result
```

To use it:

```python
N = 50
memo = [-1] * (N + 1)   # index 0..N
print(fib_memo(N, memo))
```

**Important:**  
- The memo is shared across recursive calls (passed as an argument).  
- Only `fib(n)` is computed once; all subsequent calls to `fib(n)` return instantly from the memo.

---

## 6. Step-by-Step Walkthrough (n = 5)

Let’s trace `fib_memo(5, memo)` with `memo` initially `[-1]*6`.

1. **`fib(5)`**: `memo[5] == -1` → compute  
   - `fib(4)`: `memo[4] == -1` → compute  
     - `fib(3)`: `memo[3] == -1` → compute  
       - `fib(2)`: `memo[2] == -1` → compute  
         - `fib(1)`: base case → return 1  
         - `fib(0)`: base case → return 1  
         - `fib(2) = 1+1 = 2`, store `memo[2]=2`  
       - `fib(1)`: base case → return 1 (already in memo, but base cases are not stored)  
       - `fib(3) = 2+1 = 3`, store `memo[3]=3`  
     - `fib(2)`: now `memo[2] != -1` → **return 2 directly** (no recursion)  
     - `fib(4) = 3+2 = 5`, store `memo[4]=5`  
   - `fib(3)`: `memo[3] != -1` → return 3 directly  
   - `fib(5) = 5+3 = 8`, store `memo[5]=8`  

Result: `8`.  
The memo now holds all computed values: `[1, 1, 2, 3, 5, 8]`. Notice that `fib(2)` and `fib(3)` were computed only once each.

---

## 7. Recursion Tree Comparison

### Naïve Recursion Tree (n=5)
Each node is a function call.

```
                            fib(5)
                          /        \
                     fib(4)        fib(3)
                    /     \        /    \
               fib(3)   fib(2)  fib(2) fib(1)
              /    \    /    \   /    \
         fib(2) fib(1) … … … … … … … …
         /   \
    fib(1) fib(0)
```

- Total calls: **15** (including base cases).  
- Many duplicates (e.g., `fib(2)` called 3 times).

### Memoized Recursion Tree (n=5)
Only unique subproblems are computed once; subsequent calls are replaced by memo lookups (dashed arrows).

```
                         fib(5)
                       /        \
                  fib(4)        fib(3)   ← memo[3] already computed, no further recursion
                 /     \
            fib(3)     fib(2)   ← memo[2] already computed, no further recursion
           /     \
      fib(2)     fib(1)   ← base, no recursion
     /     \
fib(1)    fib(0)
```

- Total calls: **9** (only the leftmost path computes new values; right branches are trivial).  
- No duplicated computation – each `fib(k)` is computed exactly once.

---

## 8. Complexity Analysis

| Approach       | Time Complexity | Space Complexity |
|----------------|-----------------|------------------|
| Naïve Recursion| **O(2ⁿ)**       | O(n) (call stack) |
| Memoized       | **O(n)**        | O(n) (memo + stack) |

- **Naïve**: Exponential – infeasible for n > 40.  
- **Memoized**: Linear – can compute up to n = 10⁶ (with iterative DP) easily.

---

## 9. The Power of Memoization

Let’s count the actual calls for n = 15 using a counter.

**Naïve version**:

```python
calls = {}
def fib_naive_counter(n):
    if n <= 1:
        return 1
    calls[n] = calls.get(n, 0) + 1
    return fib_naive_counter(n-1) + fib_naive_counter(n-2)

fib_naive_counter(15)
print(calls)  # shows huge counts, e.g., fib(1): 610
```

**Memoized version**:

```python
calls_memo = {}
def fib_memo_counter(n, memo):
    if n <= 1:
        return 1
    if memo[n] != -1:
        return memo[n]
    calls_memo[n] = calls_memo.get(n, 0) + 1
    result = fib_memo_counter(n-1, memo) + fib_memo_counter(n-2, memo)
    memo[n] = result
    return result

memo = [-1] * 16
fib_memo_counter(15, memo)
print(calls_memo)  # each n from 2 to 15 appears exactly once
```

- Naïve: `fib(1)` called 610 times.  
- Memoized: `fib(1)` called once (as base case).  

That is the **power**: millions of redundant computations are avoided, reducing the number of recursive calls from exponential to linear.

---

## 10. Key Takeaways

1. **Memoization** is a top-down dynamic programming technique that caches results of expensive function calls.
2. It works when the problem has **overlapping subproblems** – the same subproblem appears many times.
3. The memo (often an array or dictionary) stores results indexed by the function arguments.
4. Before performing recursion, the memo is checked; if the result exists, it is returned immediately.
5. After computing a result, it is stored in the memo before returning.
6. Memoization transforms exponential time into linear time for problems like Fibonacci.
7. **Space complexity** increases because results are stored, but this is a worthwhile trade-off for massive speed gains.
8. Memoization can be applied to many recursive problems (e.g., factorial, edit distance, knapsack).
9. For Fibonacci, an even more efficient **iterative DP** (bottom-up) exists, but memoization illustrates the core DP idea.
10. Always ask: “Are the same subproblems being recomputed?” If yes, memoization can help.

---

## Final Note

The original DP attempt with `diary` was on the right track. The refined version presented here is simpler and more common. The key is to use a `memo` list (or dictionary) that is passed down or accessible globally. Base cases are often not stored in the memo (though they can be). With this approach, the performance improvement is dramatic.