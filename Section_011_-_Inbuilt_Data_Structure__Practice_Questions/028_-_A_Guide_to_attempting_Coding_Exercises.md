# Problem Solving Guide for Coding Platforms

## Before Starting
```
Read the problem completely - twice
Identify input constraints
Understand expected output format
Check sample test cases
```

## Common Platform Types
```
LeetCode - Focus on optimization
HackerRank - Strict input/output format
CodeChef - Handle multiple test cases
Codeforces - Fast I/O important
```

## Input/Output Handling

### Fast I/O for Large Inputs
```python
import sys
def solve():
    # For multiple lines
    data = sys.stdin.read().strip().split()
    
    # For single line
    n = int(sys.stdin.readline())
    
    # For multiple test cases
    t = int(sys.stdin.readline())
    for _ in range(t):
        n = int(sys.stdin.readline())
        arr = list(map(int, sys.stdin.readline().split()))

solve()
```

### Multiple Test Cases Pattern
```python
def solve_test_case():
    # Your logic here
    pass

def main():
    t = int(input())
    for _ in range(t):
        result = solve_test_case()
        print(result)

if __name__ == "__main__":
    main()
```

## Common Problem Patterns

### Array Problems
```python
# Two pointers technique
def two_sum(arr, target):
    left, right = 0, len(arr) - 1
    while left < right:
        current = arr[left] + arr[right]
        if current == target:
            return [left, right]
        elif current < target:
            left += 1
        else:
            right -= 1
    return []

# Sliding window
def max_sum_subarray(arr, k):
    window_sum = sum(arr[:k])
    max_sum = window_sum
    for i in range(k, len(arr)):
        window_sum = window_sum - arr[i-k] + arr[i]
        max_sum = max(max_sum, window_sum)
    return max_sum
```

### String Problems
```python
# Palindrome check
def is_palindrome(s):
    return s == s[::-1]

# Character frequency
from collections import Counter
freq = Counter(s)

# String to list of words
words = s.split()
```

### Number Theory
```python
# Prime check
def is_prime(n):
    if n < 2:
        return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True

# GCD
import math
gcd = math.gcd(a, b)

# Modulo operations
result = (a + b) % MOD
result = (a * b) % MOD
```

## Optimization Tricks

### Precomputation
```python
# Prefix sums
def prefix_sum(arr):
    prefix = [0] * (len(arr) + 1)
    for i in range(len(arr)):
        prefix[i+1] = prefix[i] + arr[i]
    return prefix

# Factorials precomputation
MOD = 10**9 + 7
fact = [1] * (n+1)
for i in range(2, n+1):
    fact[i] = (fact[i-1] * i) % MOD
```

### Memoization
```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n <= 1:
        return n
    return fib(n-1) + fib(n-2)
```

### Bit Manipulation
```python
# Check if number is power of two
def is_power_of_two(n):
    return n > 0 and (n & (n-1)) == 0

# Count set bits
def count_set_bits(n):
    count = 0
    while n:
        count += n & 1
        n >>= 1
    return count
```

## Debugging Strategies

### Print Debugging
```python
def solve():
    arr = [1, 2, 3, 4]
    # Debug print
    print(f"DEBUG: arr = {arr}")
    
    # Check intermediate values
    for i in range(len(arr)):
        print(f"DEBUG: i={i}, arr[i]={arr[i]}")
```

### Edge Cases Checklist
```
Empty input
Single element
All same values
Already sorted
Reverse sorted
Maximum constraints
Minimum constraints
Negative numbers
Zero
Large numbers
```

## Time Complexity Estimation

### Common Complexities
```
O(1)      - Constant time
O(log n)  - Binary search
O(n)      - Single loop
O(n log n)- Sorting
O(n²)     - Nested loops
O(2ⁿ)     - Recursive without memoization
```

### Quick Calculator
```python
# n = 10^6, O(n) is fine
# n = 10^5, O(n log n) is fine
# n = 5000, O(n²) might work
# n = 20, O(2ⁿ) might work
```

## Input Validation
```python
def solve():
    try:
        n = int(input())
        if n < 1 or n > 10**5:
            return
        arr = list(map(int, input().split()))
        if len(arr) != n:
            return
        # Main logic
    except:
        return
```

## Output Formatting
```python
# Space-separated
print(*result_list)

# New line for each
for item in result_list:
    print(item)

# Specific decimal places
print(f"{result:.6f}")

# Join with space
print(" ".join(map(str, result_list)))
```

## Common Mistakes to Avoid

```
1. Not reading input correctly
2. Off-by-one errors
3. Integer overflow
4. Not resetting variables for multiple test cases
5. Ignoring constraints
6. Wrong data type selection
7. Not handling empty input
8. Infinite loops
9. Stack overflow in recursion
10. Not using modulo when required
```

## Template for Competitions
```python
import sys
from collections import defaultdict, Counter, deque
from math import gcd, sqrt
from functools import lru_cache
import heapq

def solve():
    # Read input
    data = sys.stdin.read().strip().split()
    if not data:
        return
    
    # Parse input based on problem
    t = int(data[0])
    idx = 1
    results = []
    
    for _ in range(t):
        n = int(data[idx]); idx += 1
        arr = list(map(int, data[idx:idx+n])); idx += n
        
        # Your solution here
        result = process_test_case(n, arr)
        results.append(str(result))
    
    # Output
    print("\n".join(results))

def process_test_case(n, arr):
    # Main logic for one test case
    return sum(arr)  # Example

if __name__ == "__main__":
    solve()
```

## Platform-Specific Tips

### LeetCode
```
Focus on function implementation
Return value, don't print
Class-based solutions
Check constraints in problem description
```

### HackerRank
```
Stick to exact output format
Multiple test cases handled automatically
Sometimes need if __name__ == "__main__"
Watch for trailing spaces
```

### CodeChef/Codeforces
```
Fast I/O is crucial
Handle multiple test cases yourself
Watch for large input sizes
Sometimes interactive problems
```

## Quick Reference Card

```
Reading:     sys.stdin.read() for speed
Looping:     for i in range(n) not while
Sorting:     arr.sort() or sorted(arr)
Reverse:     arr[::-1] or reversed(arr)
Min/Max:     min(arr), max(arr)
Sum:         sum(arr)
Count:       arr.count(x)
Unique:      set(arr)
Map:         list(map(int, input().split()))
Zip:         list(zip(arr1, arr2))
Enumeration: for i, val in enumerate(arr):
```

Remember: Practice consistently, analyze failed test cases, and learn from editorial solutions.