# 🧮 Combinatorics - Unique Paths

#tags: #dsa #math #combinatorics

## 📌 Problem
Count number of ways to reach bottom-right in grid.

---

## 🧠 Idea
- Total moves = m+n-2
- Choose (m-1) or (n-1)

---

## 💻 Code
```java
public static int uniquePaths(int m, int n) {
    int N = m + n - 2;
    int R = Math.min(m - 1, n - 1);

    long result = 1;
    for (int i = 1; i <= R; i++) {
        result = result * (N - R + i) / i;
    }

    return (int) result;
}
```

---

## ⚡ Pattern
- nCr computation
