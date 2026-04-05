# 🔢 Catalan Numbers

## 📌 Formula

C(n) = Σ C(i-1) * C(n-i)

---

## 💻 Code
```java
public int nthcatalan(int n) {
    int[] dp = new int[n + 1];
    dp[0] = 1;

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= i; j++) {
            dp[i] += dp[j - 1] * dp[i - j];
        }
    }

    return dp[n];
}
```

---

## 🧠 Use Cases
- BST count
- Parenthesis problems
- Tree structures
