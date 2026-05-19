---
tags:
  - dsa
  - bfs
  - graph
  - hard
  - leetcode
  - striver
aliases:
  - Word Ladder II
  - LeetCode 126
difficulty: Hard
topic: BFS / Graph
pattern: BFS for shortest paths + DFS to reconstruct
leetcode_id: 126
date: 2026-05-19
---

# Word Ladder II

## 📌 Problem Statement

> Given `beginWord`, `endWord`, and a `wordList`, return all shortest transformation sequences from `beginWord` to `endWord`, changing one letter at a time, using only words from the list.

**LeetCode #126** | Difficulty: 🔴 Hard | Striver SDE Sheet ✅

---

## 🧠 Intuition

BFS layer by layer to build a "parent map" of how each word was reached (shortest path). Then DFS/backtrack from `endWord` back to `beginWord` to reconstruct all paths.

---

## ✅ Java Solution

```java
class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        Set<String> wordSet = new HashSet<>(wordList);
        List<List<String>> res = new ArrayList<>();
        Map<String, List<String>> parents = new HashMap<>();
        Map<String, Integer> dist = new HashMap<>();

        Queue<String> q = new LinkedList<>();
        q.offer(beginWord);
        dist.put(beginWord, 0);

        while (!q.isEmpty()) {
            String word = q.poll();
            int d = dist.get(word);
            if (word.equals(endWord)) break;
            char[] arr = word.toCharArray();
            for (int i = 0; i < arr.length; i++) {
                char orig = arr[i];
                for (char c = 'a'; c <= 'z'; c++) {
                    arr[i] = c;
                    String next = new String(arr);
                    if (!wordSet.contains(next)) { arr[i] = orig; continue; }
                    if (!dist.containsKey(next)) {
                        dist.put(next, d + 1);
                        q.offer(next);
                    }
                    if (dist.get(next) == d + 1) {
                        parents.computeIfAbsent(next, k -> new ArrayList<>()).add(word);
                    }
                    arr[i] = orig;
                }
            }
        }

        if (dist.containsKey(endWord)) {
            dfs(endWord, beginWord, parents, new LinkedList<>(Arrays.asList(endWord)), res);
        }
        return res;
    }

    private void dfs(String word, String begin, Map<String, List<String>> parents,
                     LinkedList<String> path, List<List<String>> res) {
        if (word.equals(begin)) { res.add(new ArrayList<>(path)); return; }
        if (!parents.containsKey(word)) return;
        for (String parent : parents.get(word)) {
            path.addFirst(parent);
            dfs(parent, begin, parents, path, res);
            path.removeFirst();
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(N × L × 26) — N words, L length |
| Space | O(N × L) |
