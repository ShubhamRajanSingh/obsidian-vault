# 📈 Longest Increasing Subsequence (LIS)

#tags: #dsa #dp #binarysearch

## 📌 Problem
Find length of longest increasing subsequence.

---

## 🧠 Approach 1: DP O(n^2)
- dp[i] = LIS ending at i

---

## 🧠 Approach 2: Binary Search (Optimal)
- Maintain `sub` array
- Replace using binary search

---

## 💻 Code
```java
public int lengthOfLIS(int[] nums) {
    ArrayList<Integer> sub = new ArrayList<>();

    for (int num : nums) {
        int pos = Collections.binarySearch(sub, num);
        if (pos < 0) pos = -(pos + 1);

        if (pos == sub.size()) sub.add(num);
        else sub.set(pos, num);
    }

    return sub.size();
}
```

---

## ⚡ Pattern
- Greedy + Binary Search
