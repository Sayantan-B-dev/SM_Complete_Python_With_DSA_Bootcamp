## Strings, Substrings, and Subsequences

- **Substring**: A contiguous sequence of characters within a string. For a string of length `n`, the number of non‑empty substrings is `n(n+1)/2` (including all possible starting and ending positions).  
- **Subsequence**: A sequence that can be derived by deleting some (or none) of the characters without changing the order of the remaining characters. For a string of length `n`, the total number of subsequences (including the empty subsequence) is `2ⁿ`.

---

## Algorithms to Generate All Subsequences of a String

Both algorithms aim to produce every possible combination of characters that preserves the original order. We describe them **without code**, focusing on the logical steps.

### 1. Recursive Algorithm

The recursive approach is based on the simple idea: for each character, we have two choices – include it in the subsequence or exclude it.

**Algorithm (conceptual):**

1. **Base case**: If the string is empty, the only subsequence is the empty string. Return a set/list containing `""`.
2. **Recursive step**:
   - Let the first character be `first` and the rest of the string be `rest`.
   - Recursively find all subsequences of `rest`. Call this set `subseq_rest`.
   - To build all subsequences of the full string:
     - Keep all subsequences from `subseq_rest` (these are the ones that **exclude** `first`).
     - For each subsequence in `subseq_rest`, **prepend** `first` to it. These are the subsequences that **include** `first`.
   - Combine (union) these two groups and return the result.

**Example walkthrough** for `"abc"`:
- `"abc"` → first = `'a'`, rest = `"bc"`.
  - Recursively find subsequences of `"bc"`:
    - `"bc"` → first = `'b'`, rest = `"c"`.
      - `"c"` → first = `'c'`, rest = `""`.
        - `""` → returns `{""}`.
      - From `{""}`: exclude `'c'` gives `""`; include `'c'` gives `"c"`. So `{"", "c"}`.
    - From `{"", "c"}`: exclude `'b'` gives `{"", "c"}`; include `'b'` gives `{"b", "bc"}`. Combined: `{"", "b", "c", "bc"}`.
  - From `{"", "b", "c", "bc"}`: exclude `'a'` gives that set; include `'a'` gives `{"a", "ab", "ac", "abc"}`. Combined: all 8 subsequences.

**Properties**:
- Time complexity: O(2ⁿ) – each subsequence is generated once.
- Space complexity: O(2ⁿ) to store all results (if we store them) plus recursion stack depth O(n).

---

### 2. Iterative Algorithm (using the “power set” idea)

The iterative method builds subsequences incrementally. It mimics the binary counting approach: for a string of length `n`, there are exactly `2ⁿ` subsequences, each corresponding to a binary number from `0` to `2ⁿ‑1`, where a `1` at bit position `i` indicates that the character at index `i` is included.

**Algorithm (bitmasking style):**

1. Let `n` be the length of the string.
2. Initialize an empty list `result` to hold all subsequences.
3. For each integer `mask` from `0` to `2ⁿ‑1`:
   - Create an empty string `subseq`.
   - For each position `i` from `0` to `n‑1`:
     - If the bit at position `i` in `mask` is set (i.e., `mask & (1 << i) != 0`), then append the character at index `i` to `subseq`.
   - Add `subseq` to `result`.
4. After the loop, `result` contains all possible subsequences.

**Alternative iterative construction (without bitmasking):**

- Start with a list containing the empty string: `[""]`.
- For each character `ch` in the original string:
  - Create a new list `new_subseqs` by taking every existing subsequence in the current list and appending `ch` to it.
  - Merge (extend) the original list with `new_subseqs`.
- At the end, the list contains all subsequences.

**Example** with `"abc"` using the second method:
- Start: `[""]`
- Process `'a'`: new = `["a"]`; list becomes `["", "a"]`
- Process `'b'`: new = `["b", "ab"]`; list becomes `["", "a", "b", "ab"]`
- Process `'c'`: new = `["c", "ac", "bc", "abc"]`; list becomes `["", "a", "b", "ab", "c", "ac", "bc", "abc"]`

**Properties**:
- Time complexity: O(n·2ⁿ) – each of the 2ⁿ subsequences is built by appending characters, and each character may be processed multiple times.
- Space complexity: O(2ⁿ) to store the results.

---

## Summary

- **Recursive algorithm** follows a natural “include/exclude” decision tree and is elegant but can be limited by recursion depth for very long strings.
- **Iterative algorithms** (especially the incremental construction) are straightforward and avoid recursion overhead; they are easy to implement iteratively in any language.

Both methods correctly generate all subsequences and demonstrate fundamental combinatorial generation techniques.