## Omega Notation (Ω) – Lower Bound

### What is Omega Notation?
Omega notation describes the **asymptotic lower bound** of an algorithm's time complexity. It gives us a guarantee that the algorithm will take **at least** a certain amount of time (or operations) for large inputs. In other words, it represents the **best-case** scenario (or the minimum time the algorithm could possibly take), but more formally it's a lower bound on the growth rate.

### Mathematical Definition
We say that **`f(n) = Ω(g(n))`** if there exist positive constants **`c`** and **`n₀`** such that:

> **`0 ≤ c·g(n) ≤ f(n)` for all `n ≥ n₀`**

Here:
- `f(n)` is the exact number of operations (or runtime function).
- `g(n)` is a simple function we compare it to.
- The constant `c` scales `g(n)` downward.
- `n₀` is the threshold beyond which the inequality holds.

In simple terms: For all sufficiently large inputs, `f(n)` is **at least** some constant multiple of `g(n)`. This makes `g(n)` an asymptotic **lower bound**.

### Graph Representation
Imagine we plot `f(n)` (the actual operation count) and `c·g(n)` (the scaled lower bound). After some point `n₀`, `f(n)` always stays **above** `c·g(n)`.

```
Operations
  ^
  |          ................ f(n)
  |        .
  |      .
  |    .  c·g(n)
  |  .
  |.
  |_________________________________> n
         ↑
        n₀
```

The curve of `f(n)` lies above the curve of `c·g(n)` for all `n ≥ n₀`. This shows that `g(n)` is a valid lower bound.

### Example
Suppose an algorithm performs exactly `f(n) = 2n² + 3n + 5` operations. We claim `f(n) = Ω(n²)`. To prove it, we need a constant `c` and a threshold `n₀`.

For `n ≥ 1`, we have:
```
2n² + 3n + 5 ≥ 2n²
```
So we can choose `c = 2` and `n₀ = 1`. Then for all `n ≥ 1`, `f(n) ≥ 2·n²`. Hence `f(n) = Ω(n²)`.

We could also pick a smaller `c` (e.g., `c = 1`) with a larger `n₀` (since eventually `2n²+3n+5 ≥ n²` for `n ≥ 1` as well). The existence of any such pair is sufficient.

### Important Note
Omega notation is often used to describe the **best-case** performance of an algorithm (e.g., linear search is Ω(1) because the first element could be the target). However, it can also be used to give a lower bound on any case – for instance, any comparison-based sorting algorithm must be Ω(n log n) in the worst case. So Omega is not exclusively about best case, but it's the notation for a lower bound.

---

## Theta Notation (Θ) – Tight Bound

### What is Theta Notation?
Theta notation provides an **asymptotic tight bound** – it means the algorithm's time complexity grows **exactly** like a given function, within constant factors. When we say an algorithm is Θ(g(n)), we are saying that its runtime is both **upper bounded** and **lower bounded** by g(n). This represents the **average-case** scenario (or the exact order of growth).

### Mathematical Definition
We say that **`f(n) = Θ(g(n))`** if there exist positive constants **`c₁`**, **`c₂`**, and **`n₀`** such that:

> **`0 ≤ c₁·g(n) ≤ f(n) ≤ c₂·g(n)` for all `n ≥ n₀`**

In other words, `f(n)` is sandwiched between two constant multiples of `g(n)` for all sufficiently large `n`. This means `f(n)` and `g(n)` have the same asymptotic growth rate.

### Graph Representation
Plot `f(n)` along with two curves: `c₁·g(n)` (the lower bound) and `c₂·g(n)` (the upper bound). After `n₀`, `f(n)` stays between them.

```
Operations
  ^
  |          ............ c₂·g(n)
  |        .           .
  |      .               . f(n)
  |    .                   .
  |  .                       . c₁·g(n)
  |.                           .
  |_________________________________> n
         ↑
        n₀
```

The shaded band between the two bounding lines contains `f(n)` for all `n ≥ n₀`. This shows that `f(n)` grows exactly like `g(n)` up to constant factors.

### Example
Consider again `f(n) = 2n² + 3n + 5`. We already saw it is O(n²) and Ω(n²). To show it is Θ(n²), we need both an upper bound and a lower bound.

- Upper bound: We earlier found `2n² + 3n + 5 ≤ 9n²` for `n ≥ 1` (choose `c₂ = 9`, `n₀ = 1`).
- Lower bound: We found `2n² + 3n + 5 ≥ 2n²` for `n ≥ 1` (choose `c₁ = 2`, `n₀ = 1`).

Thus, with `c₁ = 2`, `c₂ = 9`, and `n₀ = 1`, we have `2·n² ≤ f(n) ≤ 9·n²` for all `n ≥ 1`. Hence `f(n) = Θ(n²)`.

### Important Note
Theta notation gives the most precise description of an algorithm's growth. If we can prove both an upper bound (Big O) and a lower bound (Omega) with the same function, we can conclude the algorithm is Theta of that function. For example, merge sort is Θ(n log n) because its worst‑case, best‑case, and average‑case are all proportional to n log n. In contrast, quick sort is O(n²) in the worst case, but its average case is Θ(n log n); so we cannot say quick sort is Θ(n log n) without specifying the case.

---

## Summary Comparison

| Notation | Meaning | Mathematical Condition | Typical Use |
|----------|---------|------------------------|-------------|
| **Big O (O)** | Upper bound (worst-case) | `f(n) ≤ c·g(n)` for `n ≥ n₀` | Guarantees algorithm won't be worse than this |
| **Omega (Ω)** | Lower bound (best-case) | `f(n) ≥ c·g(n)` for `n ≥ n₀` | Guarantees algorithm can't be better than this |
| **Theta (Θ)** | Tight bound (average-case) | `c₁·g(n) ≤ f(n) ≤ c₂·g(n)` for `n ≥ n₀` | Precisely describes growth rate |
