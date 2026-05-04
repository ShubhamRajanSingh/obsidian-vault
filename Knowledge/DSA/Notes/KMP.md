# KMP — Knuth-Morris-Pratt Algorithm

**Category:** String Matching **Time Complexity:** O(n + m) **Space Complexity:** O(m) **Topics:** String, Pattern Matching, Failure Function

---

## Problem Statement

Given a text string `text` of length `n` and a pattern string `pattern` of length `m`, find **all occurrences** of `pattern` in `text`.

```
text    = "AABAACAADAABAABA"
pattern = "AABA"

Output: Pattern found at index 0, 9, 12
```

---

## Why Not Naive?

The naive approach compares the pattern at every position in text. On a mismatch, it resets the pattern pointer back to `0` and moves the text pointer forward by just `1`.

```
text:    A A B A A C ...
pattern: A A B A
               ↑ mismatch at index 3 (text[4]='A', pattern[3]='A' actually matches... )

Worst case — text = "AAAAAAB", pattern = "AAAAB"
Every position almost matches → O(n·m)
```

The problem: **we throw away information**. When a mismatch happens after matching several characters, we already know what those matched characters were. KMP uses this to avoid redundant comparisons.

---

## Core Idea

> When a mismatch occurs at position `j` in the pattern, we don't need to restart from `j = 0`. We can restart from a position we already know is safe — because a **prefix of the pattern** also matches a **suffix of what we've already matched**.

This is encoded in the **LPS (Longest Proper Prefix which is also a Suffix)** array, also called the **failure function**.

---

## LPS Array (Failure Function)

For each position `i` in the pattern, `lps[i]` stores the **length of the longest proper prefix of `pattern[0..i]` that is also a suffix**.

- **Proper prefix** means the prefix cannot be the entire string itself.

### Example

```
pattern = "AABAABAAB"
index:     0 1 2 3 4 5 6 7 8

lps[0] = 0   "A"        → no proper prefix = suffix
lps[1] = 1   "AA"       → "A" is both prefix and suffix
lps[2] = 0   "AAB"      → no match
lps[3] = 1   "AABA"     → "A" matches
lps[4] = 2   "AABAA"    → "AA" matches
lps[5] = 3   "AABAAB"   → "AAB" matches
lps[6] = 4   "AABAABA"  → "AABA" matches
lps[7] = 5   "AABAABAA" → "AABAA" matches
lps[8] = 6   "AABAABAAB"→ "AABAAB" matches

lps = [0, 1, 0, 1, 2, 3, 4, 5, 6]
```

### What LPS Tells Us on Mismatch

If we matched `j` characters and then hit a mismatch, `lps[j-1]` tells us:

> "The first `lps[j-1]` characters of the pattern are already matched — restart matching from there."

We skip re-comparing those characters entirely.

---

## Building the LPS Array

```
i = 1 (current position being filled)
len = 0 (length of previous longest prefix-suffix)

For each i from 1 to m-1:
  - If pattern[i] == pattern[len]:
      len++, lps[i] = len, i++
  - Else if len != 0:
      len = lps[len - 1]   ← fall back (don't increment i)
  - Else:
      lps[i] = 0, i++
```

### Trace: `pattern = "AABAABAAB"`

|i|len|pattern[i]|pattern[len]|Match?|lps[i]|Action|
|---|---|---|---|---|---|---|
|1|0|A|A|✅|1|len=1, i=2|
|2|1|B|A|❌|—|len=lps[0]=0|
|2|0|B|A|❌|0|i=3|
|3|0|A|A|✅|1|len=1, i=4|
|4|1|A|A|✅|2|len=2, i=5|
|5|2|B|B|✅|3|len=3, i=6|
|6|3|A|A|✅|4|len=4, i=7|
|7|4|A|A|✅|5|len=5, i=8|
|8|5|B|B|✅|6|len=6, i=9|

**LPS = [0, 1, 0, 1, 2, 3, 4, 5, 6]** ✅

---

## KMP Search

With the LPS array built, the search phase uses two pointers:

- `i` — moves through `text` (never goes back)
- `j` — moves through `pattern` (falls back using LPS on mismatch)

```
i=0, j=0

While i < n:
  If text[i] == pattern[j]:
    i++, j++
  If j == m:
    → Match found at index (i - m)
    → j = lps[j-1]   (continue searching for more matches)
  Else if text[i] != pattern[j]:
    If j != 0:
      j = lps[j-1]   ← fall back using LPS (don't move i)
    Else:
      i++             ← no fallback possible, advance text
```

The key insight: **`i` never moves backward**. This is what guarantees O(n) for the search phase.

---

## Java Implementation

```java
public class KMP {

    // Phase 1: Build the LPS (failure function) array — O(m)
    private static int[] buildLPS(String pattern) {
        int m = pattern.length();
        int[] lps = new int[m];
        int len = 0; // length of previous longest prefix-suffix
        int i = 1;

        lps[0] = 0; // always 0 by definition

        while (i < m) {
            if (pattern.charAt(i) == pattern.charAt(len)) {
                len++;
                lps[i] = len;
                i++;
            } else {
                if (len != 0) {
                    // Fall back — don't increment i
                    len = lps[len - 1];
                } else {
                    lps[i] = 0;
                    i++;
                }
            }
        }

        return lps;
    }

    // Phase 2: Search using LPS — O(n)
    public static void search(String text, String pattern) {
        int n = text.length();
        int m = pattern.length();

        int[] lps = buildLPS(pattern);

        int i = 0; // pointer for text
        int j = 0; // pointer for pattern

        while (i < n) {
            if (text.charAt(i) == pattern.charAt(j)) {
                i++;
                j++;
            }

            if (j == m) {
                // Full match found
                System.out.println("Pattern found at index " + (i - m));
                j = lps[j - 1]; // continue to find overlapping matches
            } else if (i < n && text.charAt(i) != pattern.charAt(j)) {
                if (j != 0) {
                    j = lps[j - 1]; // fall back, don't move i
                } else {
                    i++; // no fallback, move text pointer
                }
            }
        }
    }

    public static void main(String[] args) {
        search("AABAACAADAABAABA", "AABA");
        // Pattern found at index 0
        // Pattern found at index 9
        // Pattern found at index 12

        search("AABAABAAB", "AABAABAAB");
        // Pattern found at index 0

        search("AAAAAAB", "AAAB");
        // Pattern found at index 3
    }
}
```

---

## Full Dry Run

**text = `"AABXAABY"`**, **pattern = `"AABY"`**

**LPS for `"AABY"`:**

```
lps = [0, 1, 0, 0]
```

**Search:**

|i|j|text[i]|pattern[j]|Match?|Action|
|---|---|---|---|---|---|
|0|0|A|A|✅|i=1, j=1|
|1|1|A|A|✅|i=2, j=2|
|2|2|B|B|✅|i=3, j=3|
|3|3|X|Y|❌|j=lps[2]=0|
|3|0|X|A|❌|j=0, i=4|
|4|0|A|A|✅|i=5, j=1|
|5|1|A|A|✅|i=6, j=2|
|6|2|B|B|✅|i=7, j=3|
|7|3|Y|Y|✅|i=8, j=4|
|—|4|—|—|j==m|**Match at index 4**, j=lps[3]=0|

**Output:** `Pattern found at index 4` ✅

Notice at step `i=3, j=3`: mismatch at `X` vs `Y`. KMP uses `lps[2] = 0` — the first two matched characters (`AA`) gave us a prefix-suffix of length 0 at position 2, so we restart from `j=0`. Then `text[3]='X'` still doesn't match `pattern[0]='A'`, so `i` advances. **`i` never went back.**

---

## Why `j = lps[j-1]` After a Match?

After finding a full match at position `i - m`, we don't reset `j = 0`. Instead:

```java
j = lps[j - 1];  // j was m, so lps[m-1]
```

This handles **overlapping matches**:

```
text    = "AABABAB"
pattern = "ABAB"
lps     = [0, 0, 1, 2]

Match at index 0: "ABAB"
  j = lps[3] = 2  → we already know "AB" is matched
  Continue from j=2, i=4

Match at index 2: "ABAB"  ← overlapping!
```

If we reset `j = 0`, we'd miss overlapping occurrences.

---

## Complexity Analysis

|Phase|Time|Space|
|---|---|---|
|Build LPS|O(m)|O(m)|
|Search|O(n)|O(1)|
|**Total**|**O(n + m)**|**O(m)**|

### Why O(n) for search?

`i` strictly increases — it moves forward on every match and on every dead-end mismatch (j=0). `j` can decrease via `lps`, but `j` can only decrease as many times as it has increased, and it increases at most `n` times (once per `i` increment). So total operations ≤ 2n → **O(n)**.

---

## KMP vs Rabin-Karp vs Z-Algorithm

|Algorithm|Preprocessing|Search|Worst Case|Best For|
|---|---|---|---|---|
|Naive|None|O(n·m)|O(n·m)|Very short patterns|
|KMP|O(m) — LPS|O(n)|O(n+m)|Single pattern, guaranteed linear|
|Rabin-Karp|O(m) — hash|O(n) avg|O(n·m)|Multiple patterns simultaneously|
|Z-Algorithm|O(m) — Z array|O(n)|O(n+m)|Conceptually simpler, same power|
|Boyer-Moore|O(m+σ)|O(n/m) best|O(n·m)|Large alphabets, long patterns|

KMP is the go-to when you need a **guaranteed O(n+m)** with no worst-case degradation (unlike Rabin-Karp's hash collisions).

---

## Common Mistakes to Avoid

1. **Confusing `lps[j-1]` vs `lps[j]`** — On mismatch at pattern position `j`, use `lps[j-1]` (the LPS of the already-matched prefix `pattern[0..j-1]`). Using `lps[j]` is wrong because `pattern[j]` hasn't been matched.
    
2. **Resetting `j = 0` after a full match** — This misses overlapping occurrences. Always use `j = lps[j-1]` (i.e. `lps[m-1]`) after a match.
    
3. **Incrementing `i` on every mismatch** — When `j != 0` on a mismatch, `i` must NOT move. Only `j` falls back. Moving `i` would skip characters in text.
    
4. **Setting `lps[0] = something other than 0`** — `lps[0]` is always `0` by definition — there is no proper prefix of a single character.
    
5. **Off-by-one in match index** — When `j == m`, the match starts at `i - m` (not `i - m + 1`), because `i` was already incremented past the last matched character.
    

---

## LPS Intuition — One More Way to Think About It

```
pattern = "A B A B C A B A B"
index   =  0 1 2 3 4 5 6 7 8
lps     =  0 0 1 2 0 1 2 3 4
```

At `lps[7] = 3`: the substring `"ABAB"` (indices 0–3) is a prefix, and `"ABA"` (indices 5–7) is a suffix of `"ABABCABA"`.

```
A B A B C A B A B
↑ ↑ ↑         ↑ ↑ ↑
prefix         suffix
"ABA" == "ABA" → length 3
```

When mismatch at index 8 (`pattern[8]`): → We know the last 3 characters already matched `"ABA"`, which is also the first 3 characters of the pattern. → Jump to `j = 3` — skip re-comparing those 3 characters.

---

## Related Problems

| #       | Problem                                | How KMP Applies                                       |
| ------- | -------------------------------------- | ----------------------------------------------------- |
| LC 28   | Find the Index of the First Occurrence | Direct KMP application                                |
| LC 214  | Shortest Palindrome                    | Build KMP on `s + "#" + reverse(s)`                   |
| LC 459  | Repeated Substring Pattern             | Check if `lps[m-1] > 0` and `m % (m - lps[m-1]) == 0` |
| LC 686  | Repeated String Match                  | KMP search on repeated string                         |
| LC 1392 | Longest Happy Prefix                   | Directly returns `lps[m-1]`                           |