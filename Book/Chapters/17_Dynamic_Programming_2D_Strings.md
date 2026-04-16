# Pattern 17 — Dynamic Programming: 2D / Strings

**Category:** DP · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Problems on **two strings**: comparing, matching, transforming
- Problems on **grids**: paths, minimum cost, unique ways
- **Subsequence** or **substring** matching
- Problem says: *"longest common subsequence"*, *"edit distance"*, *"distinct subsequences"*, *"unique paths"*, *"minimum path sum"*

---

## 🧠 Intuition in Plain English

When you have two sequences (two strings or a grid with two dimensions), the subproblem involves choosing a prefix of each. `dp[i][j]` represents the answer for the first `i` characters of string 1 and the first `j` characters of string 2. The table fills left-to-right, top-to-bottom, each cell building on its neighbors.

---

## 📐 Core Template

```python
# ─── LONGEST COMMON SUBSEQUENCE ───────────────────────────────────────────────
def lcs(text1, text2):
    m, n = len(text1), len(text2)
    # dp[i][j] = LCS of text1[0..i-1] and text2[0..j-1]
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1      # Characters match: extend LCS
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])  # Take best of skipping one

    return dp[m][n]


# ─── EDIT DISTANCE ────────────────────────────────────────────────────────────
def edit_distance(word1, word2):
    m, n = len(word1), len(word2)
    # dp[i][j] = min operations to convert word1[0..i-1] to word2[0..j-1]
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    # Base cases: convert to/from empty string
    for i in range(m + 1): dp[i][0] = i   # Delete all chars
    for j in range(n + 1): dp[0][j] = j   # Insert all chars

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i-1] == word2[j-1]:
                dp[i][j] = dp[i-1][j-1]           # No operation needed
            else:
                dp[i][j] = 1 + min(
                    dp[i-1][j],                    # Delete from word1
                    dp[i][j-1],                    # Insert into word1
                    dp[i-1][j-1]                   # Replace
                )

    return dp[m][n]


# ─── UNIQUE PATHS (grid DP) ───────────────────────────────────────────────────
def unique_paths(m, n):
    # dp[i][j] = number of ways to reach cell (i, j)
    dp = [[1] * n for _ in range(m)]   # First row and col = 1 (only one way)

    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i-1][j] + dp[i][j-1]   # From above + from left

    return dp[m-1][n-1]
```

---

## 📝 Problem Set

| # | Problem | Difficulty | dp[i][j] Meaning |
|---|---------|------------|-----------------|
| 1 | [Unique Paths](https://leetcode.com/problems/unique-paths/) | 🟡 Medium | Ways to reach cell (i,j) |
| 2 | [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/) | 🟡 Medium | Min cost to reach cell (i,j) |
| 3 | [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) | 🟡 Medium | LCS of first i, j chars |
| 4 | [Edit Distance](https://leetcode.com/problems/edit-distance/) | 🟡 Medium | Min edits to convert prefixes |
| 5 | [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/) | 🟡 Medium | `lcs(s, reverse(s))` |
| 6 | [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/) | 🔴 Hard | Count ways to form t from s |
| 7 | [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/) | 🔴 Hard | Pattern match with `*` and `.` |

---
---
