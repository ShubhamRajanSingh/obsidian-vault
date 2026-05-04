# Longest Common Subsequence & Longest Palindromic Subsequence

**Category:** Dynamic Programming **Topics:** String, DP, Subsequence

---

# Part 1 — Longest Common Subsequence (LCS)

**LeetCode:** https://leetcode.com/problems/longest-common-subsequence/ (LC 1143) **Difficulty:** Medium

---

## Problem Statement

Given two strings `text1` and `text2`, return the **length of their longest common subsequence**. If there is no common subsequence, return `0`.

A **subsequence** is a sequence derived from a string by deleting some (or no) characters without changing the relative order of the remaining characters.

A **common subsequence** is a subsequence that appears in both strings.

```
text1 = "abcde"
text2 = "ace"

LCS = "ace" → length 3
```

---

## Examples

### Example 1

```
Input:  text1 = "abcde", text2 = "ace"
Output: 3   ("ace")
```

### Example 2

```
Input:  text1 = "abc", text2 = "abc"
Output: 3   ("abc")
```

### Example 3

```
Input:  text1 = "abc", text2 = "def"
Output: 0   (no common subsequence)
```

---

## Constraints

- `1 <= text1.length, text2.length <= 1000`
- Strings consist of only lowercase English characters.

---

## Intuition

Let `dp[i][j]` = length of LCS of `text1[0..i-1]` and `text2[0..j-1]`.

### Recurrence

```
If text1[i-1] == text2[j-1]:
    dp[i][j] = dp[i-1][j-1] + 1      ← characters match, extend LCS

Else:
    dp[i][j] = max(dp[i-1][j], dp[i][j-1])   ← skip one character from either string
```

### Base Case

```
dp[i][0] = 0  for all i   (empty text2 → LCS = 0)
dp[0][j] = 0  for all j   (empty text1 → LCS = 0)
```

---

## DP Table Visualized

**text1 = `"abcde"`**, **text2 = `"ace"`**

```
      ""  a  c  e
  ""   0  0  0  0
   a   0  1  1  1
   b   0  1  1  1
   c   0  1  2  2
   d   0  1  2  2
   e   0  1  2  3   ← answer
```

**Reading the table:**

- `dp[1][1]`: `text1[0]='a'` == `text2[0]='a'` → `dp[0][0]+1 = 1`
- `dp[2][1]`: `text1[1]='b'` ≠ `text2[0]='a'` → `max(dp[1][1], dp[2][0]) = max(1,0) = 1`
- `dp[3][2]`: `text1[2]='c'` == `text2[1]='c'` → `dp[2][1]+1 = 2`
- `dp[5][3]`: `text1[4]='e'` == `text2[2]='e'` → `dp[4][2]+1 = 3` ✅

---

## Java Solution

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m + 1][n + 1];

        // Base cases are already 0 (default int array values)
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;   // characters match
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]); // skip one
                }
            }
        }

        return dp[m][n];
    }
}
```

---

## Dry Run

**text1 = `"ace"`**, **text2 = `"abcde"`**

|i\j|""|a|b|c|d|e|
|---|---|---|---|---|---|---|
|""|0|0|0|0|0|0|
|a|0|1|1|1|1|1|
|c|0|1|1|2|2|2|
|e|0|1|1|2|2|3|

**Answer: 3** ✅

---

## Reconstructing the Actual LCS

To recover the actual subsequence (not just the length), trace back through the DP table:

```java
public String reconstructLCS(String text1, String text2, int[][] dp) {
    int i = text1.length(), j = text2.length();
    StringBuilder sb = new StringBuilder();

    while (i > 0 && j > 0) {
        if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
            sb.append(text1.charAt(i - 1)); // part of LCS
            i--;
            j--;
        } else if (dp[i - 1][j] > dp[i][j - 1]) {
            i--; // came from top
        } else {
            j--; // came from left
        }
    }

    return sb.reverse().toString();
}
```

**Trace on `"abcde"` / `"ace"`:**

```
i=5,j=3: text1[4]='e'==text2[2]='e' → append 'e', i=4,j=2
i=4,j=2: text1[3]='d'≠text2[1]='c' → dp[3][2]=2 == dp[4][1]=1 → j-- → j=1
i=4,j=1: text1[3]='d'≠text2[0]='a' → dp[3][1]=1 == dp[4][0]=0 → j-- → j=0
loop ends

Wait — let's redo:
i=4,j=2: dp[i-1][j]=dp[3][2]=2, dp[i][j-1]=dp[4][1]=1 → i-- → i=3
i=3,j=2: text1[2]='c'==text2[1]='c' → append 'c', i=2,j=1
i=2,j=1: text1[1]='b'≠text2[0]='a' → dp[1][1]=1, dp[2][0]=0 → i-- → i=1
i=1,j=1: text1[0]='a'==text2[0]='a' → append 'a', i=0,j=0

Result reversed: "ace" ✅
```

---

## Space Optimized Solution — O(n) Space

Since `dp[i][j]` only depends on `dp[i-1][j-1]`, `dp[i-1][j]`, and `dp[i][j-1]`, we can use a single 1D array:

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[] dp = new int[n + 1];

        for (int i = 1; i <= m; i++) {
            int prev = 0; // represents dp[i-1][j-1]
            for (int j = 1; j <= n; j++) {
                int temp = dp[j]; // save before overwrite (will become prev for next j)
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[j] = prev + 1;
                } else {
                    dp[j] = Math.max(dp[j], dp[j - 1]);
                }
                prev = temp;
            }
        }

        return dp[n];
    }
}
```

---

## Complexity Analysis

|Approach|Time|Space|
|---|---|---|
|2D DP|O(m × n)|O(m × n)|
|1D DP|O(m × n)|O(n)|

---

## Common Mistakes — LCS

1. **Confusing subsequence with substring** — A subsequence does not need to be contiguous. `"ace"` is a subsequence of `"abcde"` but not a substring.
2. **Wrong index mapping** — `dp[i][j]` represents `text1[0..i-1]` and `text2[0..j-1]`. Character access is `text1.charAt(i-1)`, not `text1.charAt(i)`.
3. **Forgetting the +1 offset** — The DP table is `(m+1) × (n+1)` to accommodate base cases at row 0 and column 0.

---

---

---

# Part 2 — Longest Palindromic Subsequence (LPS)

**LeetCode:** https://leetcode.com/problems/longest-palindromic-subsequence/ (LC 516) **Difficulty:** Medium

---

## Problem Statement

Given a string `s`, find the **length of the longest subsequence** of `s` that is a palindrome.

```
s = "bbbab"
Output: 4   ("bbbb")

s = "cbbd"
Output: 2   ("bb")
```

---

## Examples

### Example 1

```
Input:  s = "bbbab"
Output: 4   (subsequence "bbbb")
```

### Example 2

```
Input:  s = "cbbd"
Output: 2   (subsequence "bb")
```

---

## Constraints

- `1 <= s.length <= 1000`
- `s` consists only of lowercase English characters.

---

## Approach 1 — Reduce to LCS

### Key Insight

> The longest palindromic subsequence of `s` is the **LCS of `s` and its reverse `s'`**.

**Why?** A palindrome reads the same forwards and backwards. A subsequence common to both `s` and `reverse(s)` must be a palindrome — matching characters from both ends simultaneously.

```
s  = "bbbab"
s' = "babbb"

LCS("bbbab", "babbb") = "bbbb" → length 4 ✅
```

### Java Solution (LCS approach)

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        String reversed = new StringBuilder(s).reverse().toString();
        return lcs(s, reversed);
    }

    private int lcs(String a, String b) {
        int m = a.length(), n = b.length();
        int[][] dp = new int[m + 1][n + 1];

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (a.charAt(i - 1) == b.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        return dp[m][n];
    }
}
```

---

## Approach 2 — Direct Interval DP

### Recurrence

Let `dp[i][j]` = length of longest palindromic subsequence in `s[i..j]`.

```
If s[i] == s[j]:
    dp[i][j] = dp[i+1][j-1] + 2     ← both ends match, include them

Else:
    dp[i][j] = max(dp[i+1][j], dp[i][j-1])  ← skip one end
```

### Base Cases

```
dp[i][i] = 1     for all i  (single character is a palindrome of length 1)
dp[i][j] = 0     for i > j  (empty substring)
```

### Fill Order

Since `dp[i][j]` depends on `dp[i+1][j-1]` (a smaller subproblem), we must fill the table by **increasing substring length**:

```
Length 1: all dp[i][i] = 1
Length 2: dp[i][i+1]
Length 3: dp[i][i+2]
...
Length n: dp[0][n-1]  ← answer
```

---

## DP Table Visualized

**s = `"bbbab"`**

Indices: b=0, b=1, b=2, a=3, b=4

```
Fill by increasing length (len = 1 to 5):

      0(b) 1(b) 2(b) 3(a) 4(b)
0(b) [ 1    2    3    3    4  ]
1(b) [ 0    1    2    2    3  ]
2(b) [ 0    0    1    1    3  ]
3(a) [ 0    0    0    1    1  ]
4(b) [ 0    0    0    0    1  ]

Answer: dp[0][4] = 4 ✅
```

**Key cells:**

- `dp[0][1]`: s[0]='b'==s[1]='b' → `dp[1][0]+2 = 0+2 = 2`
- `dp[0][4]`: s[0]='b'==s[4]='b' → `dp[1][3]+2 = 2+2 = 4`
- `dp[2][4]`: s[2]='b'==s[4]='b' → `dp[3][3]+2 = 1+2 = 3`

---

## Java Solution (Interval DP)

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        int[][] dp = new int[n][n];

        // Base case: every single character is a palindrome of length 1
        for (int i = 0; i < n; i++) dp[i][i] = 1;

        // Fill by increasing substring length
        for (int len = 2; len <= n; len++) {
            for (int i = 0; i <= n - len; i++) {
                int j = i + len - 1;

                if (s.charAt(i) == s.charAt(j)) {
                    // Both ends match
                    dp[i][j] = (len == 2) ? 2 : dp[i + 1][j - 1] + 2;
                } else {
                    // Skip one end
                    dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }

        return dp[0][n - 1];
    }
}
```

> **Why `len == 2` check?** When `len = 2` and both characters match, `dp[i+1][j-1] = dp[j][i]` which is an invalid cell (i > j). We handle it explicitly as `2`.

---

## Dry Run — Interval DP

**s = `"cbbd"`**

```
n=4, indices: c=0, b=1, b=2, d=3

Base: dp[0][0]=1, dp[1][1]=1, dp[2][2]=1, dp[3][3]=1

len=2:
  i=0,j=1: s[0]='c' ≠ s[1]='b' → max(dp[1][1], dp[0][0]) = max(1,1) = 1
  i=1,j=2: s[1]='b' == s[2]='b' → len==2 → dp[1][2] = 2
  i=2,j=3: s[2]='b' ≠ s[3]='d' → max(dp[3][3], dp[2][2]) = 1

len=3:
  i=0,j=2: s[0]='c' ≠ s[2]='b' → max(dp[1][2], dp[0][1]) = max(2,1) = 2
  i=1,j=3: s[1]='b' ≠ s[3]='d' → max(dp[2][3], dp[1][2]) = max(1,2) = 2

len=4:
  i=0,j=3: s[0]='c' ≠ s[3]='d' → max(dp[1][3], dp[0][2]) = max(2,2) = 2

Answer: dp[0][3] = 2 ✅  ("bb")
```

---

## LCS Approach vs Interval DP

||LCS Approach|Interval DP|
|---|---|---|
|Idea|Reduce to known problem|Solve directly|
|Code|Reuse LCS logic|Standalone|
|Time|O(n²)|O(n²)|
|Space|O(n²)|O(n²)|
|Intuition|Elegant reduction|Direct, self-contained|
|Reconstruction|Via LCS traceback|Via interval DP traceback|

Both are equally valid. The LCS reduction is a great insight to mention in interviews.

---

## Complexity Analysis — LPS

|Approach|Time|Space|
|---|---|---|
|LCS Reduction|O(n²)|O(n²)|
|Interval DP|O(n²)|O(n²)|

---

## Common Mistakes — LPS

1. **Forgetting `len == 2` guard in interval DP** — When `len = 2` and characters match, `dp[i+1][j-1]` accesses `dp[j][i]` where `j > i`, an invalid/zero cell. Handle explicitly.
2. **Wrong fill order** — You must iterate by **increasing length**, not by row or column. `dp[i][j]` depends on strictly smaller subproblems.
3. **Confusing LPS with LCS** — LCS needs two strings; LPS is a property of one string (though it reduces to LCS of `s` and `reverse(s)`).
4. **Thinking contiguous** — Like LCS, a palindromic subsequence is not required to be contiguous. `"bbb"` in `"bbbab"` is valid even though there's an `'a'` in between.

---

## Connection Between LCS and LPS

```
LPS(s) = LCS(s, reverse(s))

s  = "character"
s' = "retcarahc"

LCS = "carac" → length 5
→ Longest palindromic subsequence of "character" has length 5
```

This reduction works because:

- Any palindromic subsequence of `s` is also a subsequence of `reverse(s)` (a palindrome reads the same both ways)
- Any common subsequence of `s` and `reverse(s)` is a palindrome

---

## Related Problems

|#|Problem|Relationship|
|---|---|---|
|LC 1143|Longest Common Subsequence|Foundation for LPS reduction|
|LC 516|Longest Palindromic Subsequence|This problem|
|LC 5|Longest Palindromic Substring|Contiguous version (different DP)|
|LC 1312|Minimum Insertions to Make String Palindrome|`n - LPS(s)`|
|LC 583|Delete Operation for Two Strings|`m + n - 2 × LCS(s1, s2)`|
|LC 72|Edit Distance|Classic interval/2D DP on strings|