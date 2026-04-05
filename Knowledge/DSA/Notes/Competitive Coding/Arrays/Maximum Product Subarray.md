# 📊 Maximum Product Subarray

#tags: #dsa #arrays #kadane #important

## 📌 Problem
Find contiguous subarray with maximum product.

---

## 🧠 Key Insight
- Track both max and min (due to negatives)

---

## 💻 Code
```java
public int maxProduct(int[] nums) {
    int maxSoFar = nums[0];
    int minSoFar = nums[0];
    int result = nums[0];

    for (int i = 1; i < nums.length; i++) {
        int curr = nums[i];

        int tempMax = Math.max(curr, Math.max(maxSoFar * curr, minSoFar * curr));
        minSoFar = Math.min(curr, Math.min(maxSoFar * curr, minSoFar * curr));
        maxSoFar = tempMax;

        result = Math.max(result, maxSoFar);
    }

    return result;
}
```

---

## ⚡ Pattern
- Variation of Kadane’s Algorithm
