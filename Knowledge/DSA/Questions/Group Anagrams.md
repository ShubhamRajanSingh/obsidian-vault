---
tags:
  - dsa
  - hashmap
  - string
  - medium
  - leetcode
  - striver
aliases:
  - Group Anagrams
  - LeetCode 49
difficulty: Medium
topic: HashMap / String
pattern: Sort key hashing
leetcode_id: 49
date: 2026-05-19
---

# Group Anagrams

## 📌 Problem Statement

> Given an array of strings `strs`, group the anagrams together.

**LeetCode #49** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();
        for (String s : strs) {
            char[] ca = s.toCharArray();
            Arrays.sort(ca);
            String key = new String(ca);
            map.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(map.values());
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n × k log k) where k = max string length |
| Space | O(n × k) |
