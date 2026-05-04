
**Category:** String Matching / Rolling Hash **Time Complexity:** O(n + m) average, O(n·m) worst case **Space Complexity:** O(1) **Topics:** String, Hashing, Sliding Window

---

## Problem Statement

Given a text string `text` of length `n` and a pattern string `pattern` of length `m`, find **all occurrences** of `pattern` in `text`.

```
text    = "AABAACAADAABAABA"
pattern = "AABA"

Output: Pattern found at index 0, 9, 12
```

---

## Why Not Naive String Matching?

The naive approach checks the pattern against every position in the text:

```
For each position i in text (0 to n-m):
    Compare text[i..i+m-1] with pattern character by character
```

- **Worst case: O(n·m)** — e.g. `text = "AAAAAAA"`, `pattern = "AAAB"` — every position almost matches before failing at the last character.

Rabin-Karp reduces the inner comparison to **O(1) on average** using hashing.

---

## Core Idea: Rolling Hash

Instead of comparing characters one by one, compute a **hash** of the pattern and compare it with the hash of each window of size `m` in the text.

```
If hash(text[i..i+m-1]) == hash(pattern)
    → Likely a match → Verify character by character
Else
    → Definitely not a match → Skip
```

Character comparison only happens on hash matches, which are rare → average O(1) per window.

The trick is computing the hash of the next window **in O(1)** using the previous window's hash — this is the **rolling hash**.

---

## Rolling Hash Formula

Use a polynomial hash with base `b` and modulus `MOD`:

```
hash("abc") = (a × b² + b × b¹ + c × b⁰) % MOD
```

More generally for a window of size `m`:

```
hash = (text[i] × b^(m-1) + text[i+1] × b^(m-2) + ... + text[i+m-1] × b^0) % MOD
```

### Sliding the Window (Rolling)

When the window moves from `i` to `i+1`:

```
Remove contribution of text[i]    (leftmost character)
Shift remaining characters left   (multiply by b)
Add contribution of text[i+m]     (new rightmost character)
```

```
newHash = (oldHash - text[i] × b^(m-1)) × b + text[i+m]
newHash = newHash % MOD
```

This gives the next window's hash in **O(1)** — no need to rehash from scratch.

---

## Step-by-Step Example

**text = "ABCDABCD"**, **pattern = "BCD"**, `b = 26`, `MOD = 101`

Let's use simple char values: A=1, B=2, C=3, D=4

```
m = 3
b^(m-1) = b^2 = 676

Pattern hash:
  hash("BCD") = (2×676 + 3×26 + 4×1) % 101
              = (1352 + 78 + 4) % 101
              = 1434 % 101
              = 18

Window hashes in text:
  "ABC" = (1×676 + 2×26 + 3) % 101 = 731 % 101 = 24   ≠ 18 → skip
  "BCD" = (24 - 1×676) × 26 + 4... rolling → = 18      == 18 → verify → MATCH at index 1
  "CDA" = rolling → ≠ 18 → skip
  "DAB" = rolling → ≠ 18 → skip
  "ABC" = rolling → ≠ 18 → skip
  "BCD" = rolling → = 18 → verify → MATCH at index 5
```

---

## Java Implementation

```java
public class RabinKarp {

    static final int BASE = 26;
    static final int MOD  = 1_000_000_007;

    public static void search(String text, String pattern) {
        int n = text.length();
        int m = pattern.length();

        if (m > n) {
            System.out.println("Pattern longer than text.");
            return;
        }

        // Compute BASE^(m-1) % MOD  — used to remove the leading character
        long highPow = 1;
        for (int i = 0; i < m - 1; i++) {
            highPow = (highPow * BASE) % MOD;
        }

        // Compute initial hash of pattern and first window of text
        long patternHash = 0, windowHash = 0;
        for (int i = 0; i < m; i++) {
            patternHash = (patternHash * BASE + pattern.charAt(i)) % MOD;
            windowHash  = (windowHash  * BASE + text.charAt(i))    % MOD;
        }

        // Slide the window across the text
        for (int i = 0; i <= n - m; i++) {
            // Hash match → verify character by character (handle spurious hits)
            if (windowHash == patternHash) {
                if (text.substring(i, i + m).equals(pattern)) {
                    System.out.println("Pattern found at index " + i);
                }
            }

            // Roll the hash to the next window
            if (i < n - m) {
                windowHash = (windowHash - text.charAt(i) * highPow % MOD + MOD) % MOD;
                windowHash = (windowHash * BASE + text.charAt(i + m)) % MOD;
            }
        }
    }

    public static void main(String[] args) {
        search("AABAACAADAABAABA", "AABA");
        // Output:
        // Pattern found at index 0
        // Pattern found at index 9
        // Pattern found at index 12
    }
}
```

---

## Code Walkthrough

### 1. Precompute `highPow`

```java
long highPow = 1;
for (int i = 0; i < m - 1; i++) highPow = (highPow * BASE) % MOD;
```

`highPow = BASE^(m-1) % MOD` — the weight of the leftmost character in the window hash. Needed to subtract it during rolling.

### 2. Initial Hashes

```java
for (int i = 0; i < m; i++) {
    patternHash = (patternHash * BASE + pattern.charAt(i)) % MOD;
    windowHash  = (windowHash  * BASE + text.charAt(i))    % MOD;
}
```

Polynomial hash built left to right:

```
After i=0: hash = text[0]
After i=1: hash = text[0] × BASE + text[1]
After i=2: hash = text[0] × BASE² + text[1] × BASE + text[2]
```

### 3. Sliding and Matching

```java
if (windowHash == patternHash) {
    if (text.substring(i, i + m).equals(pattern)) { ... }
}
```

Hash match is necessary but not sufficient — always verify with character comparison to handle **spurious hits** (hash collisions).

### 4. Rolling the Hash

```java
windowHash = (windowHash - text.charAt(i) * highPow % MOD + MOD) % MOD;
windowHash = (windowHash * BASE + text.charAt(i + m)) % MOD;
```

Line 1 — Remove leftmost character:

```
windowHash = windowHash - text[i] × BASE^(m-1)
```

`+ MOD` before `% MOD` ensures the result stays positive (Java's `%` can return negative values for negative operands).

Line 2 — Shift left and add new rightmost character:

```
windowHash = windowHash × BASE + text[i+m]
```

---

## Spurious Hits (Hash Collisions)

A **spurious hit** is when `windowHash == patternHash` but the actual characters don't match — a false positive due to hash collision.

```
Example with small MOD:
  hash("AB") % 5 == hash("XY") % 5  → both = 3
  → Hash match, but "AB" ≠ "XY" → spurious hit
```

This is why we always verify on a hash match. Spurious hits are rare with a large prime `MOD` (e.g. `10^9 + 7`) but can degrade worst-case performance to O(n·m) if they occur frequently.

### Reducing Spurious Hits

- Use a **large prime** as `MOD` (e.g. `1_000_000_007`)
- Use **double hashing** — two independent hash functions; a collision in both is astronomically unlikely

```java
// Double hashing sketch
long hash1 = ...; // MOD1 = 1_000_000_007
long hash2 = ...; // MOD2 = 998_244_353
if (hash1 == patternHash1 && hash2 == patternHash2) {
    // Extremely unlikely to be a spurious hit
}
```

---

## Dry Run

**text = `"AABAACAADAABAABA"`**, **pattern = `"AABA"`**, `m = 4`

Using simple char values: A=65, B=66

|i|Window|Hash Match?|Verified?|Output|
|---|---|---|---|---|
|0|AABA|✅ Yes|✅ Yes|Found at index **0**|
|1|ABAA|❌ No|—|—|
|2|BAAC|❌ No|—|—|
|3|AACA|❌ No|—|—|
|4|ACAA|❌ No|—|—|
|5|CAAD|❌ No|—|—|
|6|AADA|❌ No|—|—|
|7|ADAA|❌ No|—|—|
|8|DAAB|❌ No|—|—|
|9|AABA|✅ Yes|✅ Yes|Found at index **9**|
|10|ABAA|❌ No|—|—|
|11|BAAB|❌ No|—|—|
|12|AABA|✅ Yes|✅ Yes|Found at index **12**|

---

## Complexity Analysis

|Case|Time|Reason|
|---|---|---|
|Best / Average|O(n + m)|Few or no spurious hits; each window hash computed in O(1)|
|Worst case|O(n·m)|Every window is a spurious hit (e.g. all same characters)|
|Space|O(1)|Only a few hash variables; no extra array|

- The `O(m)` term comes from computing the initial hash.
- The `O(n)` term comes from sliding the window across all `n - m + 1` positions.

---

## Comparison with Other String Matching Algorithms

|Algorithm|Preprocessing|Search|Worst Case|Key Idea|
|---|---|---|---|---|
|Naive|None|O(n·m)|O(n·m)|Compare every window|
|Rabin-Karp|O(m)|O(n)|O(n·m)|Rolling hash|
|KMP|O(m)|O(n)|O(n+m)|Failure function (no backtrack)|
|Z-Algorithm|O(m)|O(n)|O(n+m)|Z-array prefix matching|
|Boyer-Moore|O(m+alphabet)|O(n/m) best|O(n·m)|Skip via bad char/good suffix|

Rabin-Karp shines in **multiple pattern matching** — hash all patterns first (O(m·k) for k patterns), then slide once through the text O(n), checking all k pattern hashes simultaneously. KMP would require k separate passes.

---

## Multiple Pattern Matching

Rabin-Karp's real power: find all occurrences of **any** of `k` patterns in one pass.

```java
public static void multiSearch(String text, String[] patterns) {
    Set<Long> patternHashes = new HashSet<>();
    Map<Long, String> hashToPattern = new HashMap<>();
    int m = patterns[0].length(); // assume all same length for simplicity

    for (String p : patterns) {
        long h = computeHash(p, m);
        patternHashes.add(h);
        hashToPattern.put(h, p);
    }

    int n = text.length();
    long windowHash = computeHash(text.substring(0, m), m);
    long highPow = computeHighPow(m);

    for (int i = 0; i <= n - m; i++) {
        if (patternHashes.contains(windowHash)) {
            String candidate = hashToPattern.get(windowHash);
            if (text.substring(i, i + m).equals(candidate)) {
                System.out.println("'" + candidate + "' found at index " + i);
            }
        }
        if (i < n - m) {
            windowHash = roll(windowHash, text.charAt(i), text.charAt(i + m), highPow);
        }
    }
}
```

---

## Common Mistakes to Avoid

1. **Negative hash values** — Java's `%` operator returns negative results for negative operands. Always add `MOD` before taking `%` when subtracting: `(hash - val + MOD) % MOD`.
    
2. **Not verifying on hash match** — Hash equality is not character equality. Always do a direct string comparison on a hash hit to rule out spurious hits.
    
3. **Using a small or non-prime MOD** — Small moduli cause frequent collisions. Always use a large prime like `10^9 + 7` or `998_244_353`.
    
4. **Operator precedence in rolling** — `text.charAt(i) * highPow % MOD` — be careful with Java's precedence. Parenthesise explicitly: `(text.charAt(i) * highPow) % MOD`.
    
5. **Off-by-one in the loop bound** — The loop runs `i <= n - m` (inclusive), giving exactly `n - m + 1` windows. Using `i < n - m` would miss the last window.
    

---

## Related Problems

|#|Problem|How Rabin-Karp Applies|
|---|---|---|
|LC 28|Find the Index of the First Occurrence in a String|Direct application|
|LC 187|Repeated DNA Sequences|Rolling hash over fixed-length windows|
|LC 1044|Longest Duplicate Substring|Binary search + rolling hash|
|LC 718|Maximum Length of Repeated Subarray|Rolling hash + binary search|
|LC 1316|Distinct Echo Substrings|Rolling hash|