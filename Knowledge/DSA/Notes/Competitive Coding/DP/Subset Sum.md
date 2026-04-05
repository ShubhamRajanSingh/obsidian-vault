# 🎯 Subset Sum

#tags: #dsa #dp #subset

## 📌 Problem
Given array, check if subset exists with given sum.

---

## 🧠 Approach (1D DP)
- Use boolean dp array
- Traverse backwards to avoid overwrite

---

## 💻 Code
```java
public boolean subsetSum(int[] nums, int target) {
    boolean[] dp = new boolean[target + 1];
    dp[0] = true;

    for (int num : nums) {
        for (int j = target; j >= num; j--) {
            dp[j] = dp[j] || dp[j - num];
        }
    }

    return dp[target];
}
```

---

## ⚡ Pattern
- 0/1 Knapsack variant
