---
tags:
  - dsa
  - string
  - hashmap
  - medium
  - leetcode
aliases:
  - Bulls and Cows
  - LeetCode 299
difficulty: Medium
topic: String / HashMap
pattern: Count bulls then cows via frequency arrays
leetcode_id: 299
date: 2026-05-19
---

# Bulls and Cows

## 📌 Problem Statement

> You are playing Bulls and Cows. Given a secret number and a guess (both strings of same length using digits 0-9), return a hint: `xAyB` where A = bulls (right digit, right position), B = cows (right digit, wrong position).

**LeetCode #299** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public String getHint(String secret, String guess) {
        int bulls = 0, cows = 0;
        int[] sCount = new int[10], gCount = new int[10];

        for (int i = 0; i < secret.length(); i++) {
            if (secret.charAt(i) == guess.charAt(i)) bulls++;
            else { sCount[secret.charAt(i) - '0']++; gCount[guess.charAt(i) - '0']++; }
        }
        for (int i = 0; i < 10; i++) cows += Math.min(sCount[i], gCount[i]);

        return bulls + "A" + cows + "B";
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(10) = O(1) |
