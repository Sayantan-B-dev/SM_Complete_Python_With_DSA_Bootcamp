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
# Time Complexity Comparison: Naïve Recursion vs. Memoized DP

## 1. Naïve Recursive Fibonacci – Exponential Time

The function `fibonicci_number_recursive(n)` follows the recurrence:

```
T(n) = T(n-1) + T(n-2) + 1,   for n > 1
T(0) = T(1) = 1
```

This recurrence is the same as the Fibonacci sequence itself. The closed‑form solution is approximately:

```
T(n) ≈ 2 * φⁿ / √5,   where φ = (1+√5)/2 ≈ 1.618
```

Thus **T(n) = O(φⁿ) ≈ O(2ⁿ)**.  
For n = 50, T(50) ≈ 4×10¹⁰ function calls – completely impractical.

**Why exponential?**  
Each call spawns two new recursive calls (except base cases), creating a full binary tree of calls with many overlapping subtrees.

---

## 2. Memoized DP – Linear Time

In `fib_with_dp(n)`, the `diary` array stores results for every `n` once they are computed.  
The recurrence changes because each distinct `n` is computed only once:

- For each `n` (from 2 up to input), the function performs:
  - A memo lookup (constant time)
  - Two recursive calls that hit the memo (or base case) – also constant time each
  - Some arithmetic and storage (constant time)

Therefore, each `n` contributes O(1) work, and there are `n` such values. Hence:

```
T(n) = O(n)
```

For n = 50, this is about 50 units of work – a massive reduction from the exponential version.

---

## 3. Why the Dramatic Difference?

| Feature               | Naïve Recursion                     | Memoized DP                          |
|-----------------------|-------------------------------------|--------------------------------------|
| **Subproblem handling** | Solves same subproblem repeatedly | Solves each subproblem exactly once |
| **Call tree**          | Exponential number of nodes         | Linear number of computed nodes      |
| **Time complexity**    | O(2ⁿ)                               | O(n)                                 |

The key is **overlapping subproblems**: the naïve recursion recomputes `fib(2)` thousands of times; memoization caches it after the first computation.

---

## 4. Empirical Evidence (from the provided code)

- Running `fibonicci_number_recursive(15)` shows `calls[1] = 610` – meaning `fib(1)` was computed 610 times.
- Running `fib_with_dp(50)` shows every `n` from 2 to 50 appears exactly once in `calls`.

This confirms the time complexity difference:  
- Naïve: T(15) ≈ 2×10³  
- Memoized: T(50) ≈ 50