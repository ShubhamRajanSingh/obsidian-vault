# 139. Word Break

**Difficulty:** Medium **Topics:** Array, Hash Table, String, Dynamic Programming, Trie, Memoization **Link:** https://leetcode.com/problems/word-break/

---

## Problem Statement

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

The same word in the dictionary may be reused multiple times.

```
s = "leetcode"
wordDict = ["leet", "code"]
Output: true  ("leet" + "code")

s = "applepenapple"
wordDict = ["apple", "pen"]
Output: true  ("apple" + "pen" + "apple")

s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

---

## Examples

### Example 1

```
Input:  s = "leetcode", wordDict = ["leet", "code"]
Output: true
```

### Example 2

```
Input:  s = "applepenapple", wordDict = ["apple", "pen"]
Output: true   (word "apple" is reused)
```

### Example 3

```
Input:  s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

---

## Constraints

- `1 <= s.length <= 300`
- `1 <= wordDict.length <= 1000`
- `1 <= wordDict[i].length <= 20`
- `s` and `wordDict[i]` consist of only lowercase English letters
- All strings in `wordDict` are **unique**

---

## Intuition

Think of the problem recursively:

> Can we segment `s[0..n-1]`?

For every possible first word ending at index `i`, check:

- Is `s[0..i-1]` a word in the dictionary?
- Can the remaining `s[i..n-1]` also be segmented?

```
solve(s, start) = can s[start..n-1] be segmented?

For each end from start+1 to n:
    if s[start..end-1] is in wordDict AND solve(s, end):
        return true

return false
```

**Base Case:**

```
solve(s, n) = true   (empty string — nothing left to segment)
```

---

## Step 1 — Pure Recursion

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> dict = new HashSet<>(wordDict);
        return solve(s, dict, 0);
    }

    private boolean solve(String s, Set<String> dict, int start) {
        // Base case: reached end of string — valid segmentation
        if (start == s.length()) return true;

        // Try every possible end index for the next word
        for (int end = start + 1; end <= s.length(); end++) {
            String word = s.substring(start, end);
            if (dict.contains(word) && solve(s, dict, end)) {
                return true;
            }
        }

        return false;
    }
}
```

### Recursion Tree — `s = "leetcode"`, `dict = ["leet", "code"]`

```
solve(0)
├── "l" not in dict
├── "le" not in dict
├── "lee" not in dict
├── "leet" ✅ in dict → solve(4)
│     ├── "c" not in dict
│     ├── "co" not in dict
│     ├── "cod" not in dict
│     └── "code" ✅ in dict → solve(8)
│           └── start==length → return true ✅
├── "leetc" not in dict
...
```

### Problem: Overlapping Subproblems

For a string like `"aaaaab"` with dict `["a","aa","aaa","aaaa"]`, `solve(3)` may be called from `solve(0)`, `solve(1)`, and `solve(2)` — all recomputing the same suffix.

Worst case: **O(2^n)** without caching.

---

## Step 2 — Memoization (Top-Down DP)

Cache the result of `solve(start)`. Only `n` unique starting positions exist.

```java
class Solution {
    Boolean[] memo;

    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> dict = new HashSet<>(wordDict);
        memo = new Boolean[s.length()]; // null = not computed
        return solve(s, dict, 0);
    }

    private boolean solve(String s, Set<String> dict, int start) {
        // Base case
        if (start == s.length()) return true;

        // Return cached result
        if (memo[start] != null) return memo[start];

        for (int end = start + 1; end <= s.length(); end++) {
            String word = s.substring(start, end);
            if (dict.contains(word) && solve(s, dict, end)) {
                return memo[start] = true;
            }
        }

        return memo[start] = false;
    }
}
```

### What Changes from Recursion?

- Added `Boolean[] memo` (using `Boolean` not `boolean` so `null` can represent "not computed")
- Added cache check: `if (memo[start] != null) return memo[start]`
- Added cache store: `return memo[start] = true/false`
- **Logic and base cases are identical**

> **Why `Boolean[]` instead of `boolean[]`?** `boolean[]` defaults to `false` which is a valid result — can't distinguish "not computed" from "computed false". `Boolean[]` defaults to `null`, a safe sentinel.

### Complexity After Memoization

||Time|Space|
|---|---|---|
|Pure Recursion|O(2^n)|O(n) stack|
|Memoization|O(n² × m)|O(n)|

`n` = length of `s`, `m` = average word length (for substring + hash check)

---

## Step 3 — Tabulation (Bottom-Up DP)

`dp[i]` = can `s[0..i-1]` be segmented using words from `wordDict`?

### Recurrence

```
dp[i] = true  if there exists some j < i such that:
            dp[j] == true   AND
            s[j..i-1] is in wordDict
```

### Base Case

```
dp[0] = true   (empty prefix — trivially segmentable)
```

### Fill Order

Left to right — `dp[i]` depends on all `dp[j]` where `j < i`.

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> dict = new HashSet<>(wordDict);
        int n = s.length();
        boolean[] dp = new boolean[n + 1];
        dp[0] = true; // empty string is always valid

        for (int i = 1; i <= n; i++) {
            for (int j = 0; j < i; j++) {
                // If s[0..j-1] is segmentable AND s[j..i-1] is a word
                if (dp[j] && dict.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break; // no need to check further j values
                }
            }
        }

        return dp[n];
    }
}
```

---

## DP Array Visualized

**`s = "leetcode"`, `dict = ["leet", "code"]`**

```
Index:  0    1    2    3    4    5    6    7    8
Char:        l    e    e    t    c    o    d    e
dp:    [T    F    F    F    F    F    F    F    F]  ← initial

i=1: j=0: dp[0]=T, s[0..0]="l" ∉ dict → dp[1]=F
i=2: j=0: "le" ∉ dict; j=1: dp[1]=F → dp[2]=F
i=3: j=0: "lee" ∉ dict → dp[3]=F
i=4: j=0: dp[0]=T, s[0..3]="leet" ✅ → dp[4]=T
i=5: j=0..3: no match; j=4: dp[4]=T, "c" ∉ dict → dp[5]=F
i=6: j=4: dp[4]=T, "co" ∉ dict → dp[6]=F
i=7: j=4: dp[4]=T, "cod" ∉ dict → dp[7]=F
i=8: j=4: dp[4]=T, "code" ✅ → dp[8]=T

dp = [T, F, F, F, T, F, F, F, T]

Answer: dp[8] = true  ✅
```

---

## Dry Run

**`s = "applepenapple"`, `dict = ["apple", "pen"]`**

```
n = 13
dp[0] = true

i=5:  j=0: dp[0]=T, "apple" ✅ → dp[5]=T
i=8:  j=5: dp[5]=T, "pen" ✅   → dp[8]=T
i=13: j=8: dp[8]=T, "apple" ✅ → dp[13]=T

Answer: dp[13] = true  ✅
("apple" + "pen" + "apple")
```

---

## Progression Summary

|Approach|Time|Space|Notes|
|---|---|---|---|
|Recursion|O(2^n)|O(n)|Exponential — TLE|
|Memoization|O(n² × m)|O(n)|Top-down; `Boolean[]` for null sentinel|
|Tabulation|O(n² × m)|O(n)|Bottom-up; cleanest solution|

`m` = cost of substring + hash lookup (proportional to word length)

---

## Optimization — Limit Inner Loop by Max Word Length

If the longest word in `wordDict` has length `maxLen`, there's no point checking substrings longer than `maxLen`:

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> dict = new HashSet<>(wordDict);
        int n = s.length();
        int maxLen = 0;
        for (String w : wordDict) maxLen = Math.max(maxLen, w.length());

        boolean[] dp = new boolean[n + 1];
        dp[0] = true;

        for (int i = 1; i <= n; i++) {
            // Only check substrings up to maxLen in length
            for (int j = Math.max(0, i - maxLen); j < i; j++) {
                if (dp[j] && dict.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break;
                }
            }
        }

        return dp[n];
    }
}
```

This reduces the inner loop from O(n) to O(maxLen) — significant when `maxLen << n`.

---

## Word Break II — Return All Valid Sentences (LC 140)

A follow-up problem: return **all possible** segmentations.

```java
class Solution {
    Map<Integer, List<String>> memo = new HashMap<>();

    public List<String> wordBreak(String s, List<String> wordDict) {
        Set<String> dict = new HashSet<>(wordDict);
        return solve(s, dict, 0);
    }

    private List<String> solve(String s, Set<String> dict, int start) {
        if (memo.containsKey(start)) return memo.get(start);

        List<String> result = new ArrayList<>();

        if (start == s.length()) {
            result.add(""); // base: empty string for clean concatenation
            return result;
        }

        for (int end = start + 1; end <= s.length(); end++) {
            String word = s.substring(start, end);
            if (dict.contains(word)) {
                List<String> suffixes = solve(s, dict, end);
                for (String suffix : suffixes) {
                    result.add(word + (suffix.isEmpty() ? "" : " " + suffix));
                }
            }
        }

        memo.put(start, result);
        return result;
    }
}
```

```
s = "catsanddog", dict = ["cat", "cats", "and", "sand", "dog"]
Output: ["cat sand dog", "cats and dog"]
```

---

## Common Mistakes to Avoid

1. **Using `boolean[]` instead of `Boolean[]` for memoization** — `boolean[]` defaults to `false`, which is a valid answer. Use `Boolean[]` so `null` clearly means "not yet computed".
    
2. **Wrong base case** — `dp[0] = true` means the empty prefix is always valid. Without this, `dp[i]` is never set to `true` since every check starts from `dp[j]`.
    
3. **Off-by-one in substring** — `s.substring(j, i)` extracts characters from index `j` to `i-1` (exclusive end). Ensure your indices correctly represent `s[j..i-1]`.
    
4. **Not using a HashSet for dictionary** — Using a `List` for `dict.contains()` makes each lookup O(m × wordDict.size()) instead of O(m). Always convert to `HashSet` first.
    
5. **Forgetting `break` after `dp[i] = true`** — Once we find one valid split for position `i`, we can stop checking other `j` values. Not breaking wastes time but doesn't affect correctness.
    

---

## Related Problems

| #      | Problem                        | Relationship                            |
| ------ | ------------------------------ | --------------------------------------- |
| LC 139 | **Word Break** ← You are here  | Can s be segmented?                     |
| LC 140 | Word Break II                  | Return all valid segmentations          |
| LC 472 | Concatenated Words             | Word Break applied to list of words     |
| LC 322 | Coin Change                    | Same DP structure — unbounded selection |
| LC 300 | Longest Increasing Subsequence | Similar 1D DP pattern                   |