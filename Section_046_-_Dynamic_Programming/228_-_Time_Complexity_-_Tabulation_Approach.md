```py
def fibonicci_bottom_up(n):
    if(n<=1):
        return 1
    dp=[0]*(n+1)
    dp[0]=1
    dp[1]=1
    for i in range(2,n+1):
        dp[i]=dp[i-1]+dp[i-2]
    return dp[n]

n=6
ans=fibonicci_bottom_up(n)
print(ans)
```

# Time Complexity Analysis: Bottom-Up Fibonacci

The function `fibonicci_bottom_up(n)` implements an iterative dynamic programming solution. The time complexity is determined by the loop that computes each Fibonacci number from 2 up to `n`.

## Step-by-Step Analysis

1. **Base case check**  
   `if n <= 1: return 1`  
   This is a constant-time operation, `O(1)`.

2. **Array creation**  
   `dp = [0] * (n+1)`  
   Creating a list of size `n+1` takes `O(n)` time. However, this is dominated by the subsequent loop.

3. **Initialization**  
   `dp[0] = 1` and `dp[1] = 1` are constant-time assignments.

4. **Loop**  
   `for i in range(2, n+1):`  
   This loop executes `n-1` iterations.  
   Inside each iteration:  
   - `dp[i] = dp[i-1] + dp[i-2]`  
   This involves two array lookups, an addition, and an assignment – all `O(1)` operations.

   Therefore, the total work for the loop is `(n-1) * O(1) = O(n)`.

5. **Return**  
   `return dp[n]` is `O(1)`.

## Overall Time Complexity

Summing the contributions:
- Base case: `O(1)`
- Array creation: `O(n)`
- Loop: `O(n)`
- Return: `O(1)`

The dominant term is `O(n)`. Hence, the time complexity of the bottom-up Fibonacci function is **O(n)**.

## Why O(n) is Optimal

The Fibonacci problem requires computing at least `n-1` distinct values (from 2 up to `n`). Any correct algorithm must process each of these values at least once, so a lower bound of Ω(n) exists. The bottom-up approach achieves this bound, making it asymptotically optimal.