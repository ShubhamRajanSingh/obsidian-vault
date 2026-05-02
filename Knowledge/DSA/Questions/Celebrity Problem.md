# Celebrity Problem

**Difficulty:** Medium **Topics:** Array, Stack, Graph, Two Pointer **Link:** https://www.geeksforgeeks.org/the-celebrity-problem/ | https://leetcode.com/problems/find-the-celebrity/ (LC 277)

---

## Problem Statement

In a party of `n` people, a **celebrity** is defined as someone who:

- Is **known by everyone** else at the party
- **Knows nobody** else at the party

You are given a helper function:

```java
boolean knows(int a, int b); // returns true if person a knows person b
```

Find the celebrity if one exists, otherwise return `-1`.

> You may call `knows()` as few times as possible — each call has a cost.

---

## Examples

### Example 1

```
n = 4, celebrity = person 2

knows matrix (row knows col):
     0  1  2  3
  0 [0, 1, 1, 0]   person 0 knows 1 and 2
  1 [0, 0, 1, 0]   person 1 knows 2
  2 [0, 0, 0, 0]   person 2 knows nobody ✅
  3 [0, 0, 1, 0]   person 3 knows 2

Output: 2
```

### Example 2

```
n = 3

knows matrix:
     0  1  2
  0 [0, 0, 0]
  1 [0, 0, 0]
  2 [0, 0, 0]

Output: -1  (nobody is known by everyone)
```

---

## Constraints

- `1 <= n <= 100`
- `knows(a, b)` is defined externally
- There is **at most one** celebrity

---

## Intuition

### Brute Force Thinking

For every person `i`, check:

- Does every other person `j` know `i`? (n-1 checks)
- Does `i` know nobody? (n-1 checks)

This is O(n²) calls to `knows()`.

### The Key Elimination Insight

> A single call to `knows(a, b)` can **eliminate one person** as a candidate:
> 
> - If `knows(a, b) == true` → `a` cannot be the celebrity (celebrity knows nobody)
> - If `knows(a, b) == false` → `b` cannot be the celebrity (celebrity is known by everyone)

So every call eliminates exactly one person. After `n-1` calls, only **one candidate** remains. We then verify that candidate in O(n).

This gives us **O(n)** total calls.

---

## Algorithm: Stack-Based Elimination

### Phase 1 — Find the Candidate

1. Push all `n` people onto a stack.
2. While stack has more than 1 person:
    - Pop two people: `a` and `b`
    - Call `knows(a, b)`:
        - If `true` → `a` is eliminated, push `b` back
        - If `false` → `b` is eliminated, push `a` back
3. The last remaining person is the **candidate**.

### Phase 2 — Verify the Candidate

The candidate could still be a false positive. Verify:

1. The candidate knows nobody (check all others)
2. Everyone knows the candidate (check all others)

If both hold → return candidate. Else → return `-1`.

---

## Visual Trace

**n = 4, celebrity = 2**

**Phase 1 — Stack Elimination:**

```
Stack: [0, 1, 2, 3]

Pop a=2, b=3 → knows(2,3)? No → eliminate 3, push 2
Stack: [0, 1, 2]

Pop a=1, b=2 → knows(1,2)? Yes → eliminate 1, push 2
Stack: [0, 2]

Pop a=0, b=2 → knows(0,2)? Yes → eliminate 0, push 2
Stack: [2]

Candidate = 2
```

**Phase 2 — Verification of candidate 2:**

```
Does 2 know anyone?
  knows(2,0)? No ✅
  knows(2,1)? No ✅
  knows(2,3)? No ✅

Does everyone know 2?
  knows(0,2)? Yes ✅
  knows(1,2)? Yes ✅
  knows(3,2)? Yes ✅

Candidate 2 is verified → return 2 ✅
```

---

## Java Solution (Stack-Based)

```java
import java.util.*;

class Solution {
    // Assume knows(a, b) is provided
    boolean knows(int a, int b) { /* external */ return false; }

    public int findCelebrity(int n) {
        // Phase 1: Find the candidate via elimination
        Deque<Integer> stack = new ArrayDeque<>();
        for (int i = 0; i < n; i++) stack.push(i);

        while (stack.size() > 1) {
            int a = stack.pop();
            int b = stack.pop();

            if (knows(a, b)) {
                // a knows b → a is not the celebrity
                stack.push(b);
            } else {
                // a doesn't know b → b is not the celebrity
                stack.push(a);
            }
        }

        int candidate = stack.pop();

        // Phase 2: Verify the candidate
        for (int i = 0; i < n; i++) {
            if (i == candidate) continue;

            // Celebrity must not know anyone, and must be known by everyone
            if (knows(candidate, i) || !knows(i, candidate)) {
                return -1;
            }
        }

        return candidate;
    }
}
```

---

## Alternative: Two-Pointer Elimination (No Stack)

Instead of a stack, use two pointers starting at opposite ends:

```java
class Solution {
    boolean knows(int a, int b) { return false; }

    public int findCelebrity(int n) {
        // Phase 1: Find candidate with two pointers
        int left = 0, right = n - 1;

        while (left < right) {
            if (knows(left, right)) {
                left++;        // left is not the celebrity
            } else {
                right--;       // right is not the celebrity
            }
        }

        int candidate = left; // left == right at this point

        // Phase 2: Verify
        for (int i = 0; i < n; i++) {
            if (i == candidate) continue;
            if (knows(candidate, i) || !knows(i, candidate)) {
                return -1;
            }
        }

        return candidate;
    }
}
```

Both approaches are equivalent — the two-pointer version avoids the overhead of a stack data structure and is slightly more concise.

---

## Why Verification is Necessary

The elimination phase guarantees:

> "All others have been eliminated — only this candidate _could_ be the celebrity."

But it does **not** guarantee the candidate actually _is_ the celebrity. Consider:

```
n = 2
knows(0, 1) = false → candidate = 0
knows(0, 1) = false ✅ (0 knows nobody)
knows(1, 0) = false ❌ (1 doesn't know 0)
→ No celebrity exists, return -1
```

Without verification, we'd wrongly return `0`.

---

## Dry Run — Two Pointer

**n = 4, celebrity = 2**

```
left=0, right=3
knows(0,3)? No  → right-- → right=2

left=0, right=2
knows(0,2)? Yes → left++  → left=1

left=1, right=2
knows(1,2)? Yes → left++  → left=2

left == right == 2 → candidate = 2

Verify:
  knows(2,0)? No, knows(0,2)? Yes ✅
  knows(2,1)? No, knows(1,2)? Yes ✅
  knows(2,3)? No, knows(3,2)? Yes ✅

Return 2 ✅
```

---

## Complexity Analysis

|Phase|Time|Space|
|---|---|---|
|Elimination (Stack)|O(n) calls|O(n) stack|
|Elimination (Two Pointer)|O(n) calls|O(1)|
|Verification|O(n) calls|O(1)|
|**Total (Stack)**|**O(n)**|**O(n)**|
|**Total (Two Pointer)**|**O(n)**|**O(1)**|

- **Brute force:** O(n²) calls to `knows()`
- **Elimination + Verification:** at most `(n-1) + 2(n-1)` = `3n - 3` calls → **O(n)**

---

## Common Mistakes to Avoid

1. **Skipping verification** — The candidate from elimination is not guaranteed to be a celebrity. Always verify in a second pass.
2. **Calling `knows()` redundantly** — In the verification loop, check both conditions (`knows(candidate, i)` and `knows(i, candidate)`) — don't re-check what the elimination already told you if you cache results.
3. **Not handling n = 1** — A single person is trivially a celebrity (no one else to know or be known by). The stack/two-pointer naturally handles this — the loop doesn't execute and the single person is returned as candidate, verified correctly.
4. **Assuming there is always a celebrity** — The problem says at most one celebrity exists. Always return `-1` if verification fails.
5. **Confusing rows and columns** in the matrix version — `knows[a][b] == 1` means `a` knows `b`. Row = the knower, column = the known.

---

## Related Problems

|Problem|Relationship|
|---|---|
|LC 277 — Find the Celebrity|Exact problem with `knows()` API|
|LC 997 — Find the Town Judge|Same concept on a directed graph (in-degree = n-1, out-degree = 0)|
|LC 1557 — Minimum Number of Vertices|Graph reachability|