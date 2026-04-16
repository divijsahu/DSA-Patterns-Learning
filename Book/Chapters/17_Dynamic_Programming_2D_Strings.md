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

```cpp
#include <bits/stdc++.h>
using namespace std;

int longestCommonSubsequence(const string& text1, const string& text2) {
    int m = text1.size(), n = text2.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    for (int i = 1; i <= m; ++i) {
        for (int j = 1; j <= n; ++j) {
            if (text1[i - 1] == text2[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
            else dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
        }
    }
    return dp[m][n];
}

int editDistance(const string& word1, const string& word2) {
    int m = word1.size(), n = word2.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    for (int i = 0; i <= m; ++i) dp[i][0] = i;
    for (int j = 0; j <= n; ++j) dp[0][j] = j;
    for (int i = 1; i <= m; ++i) {
        for (int j = 1; j <= n; ++j) {
            if (word1[i - 1] == word2[j - 1]) dp[i][j] = dp[i - 1][j - 1];
            else dp[i][j] = 1 + min({dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]});
        }
    }
    return dp[m][n];
}

bool isInterleave(const string& s1, const string& s2, const string& s3) {
    int m = s1.size(), n = s2.size();
    if (m + n != (int)s3.size()) return false;
    vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
    dp[0][0] = true;
    for (int i = 0; i <= m; ++i) {
        for (int j = 0; j <= n; ++j) {
            if (i > 0) dp[i][j] = dp[i][j] || (dp[i - 1][j] && s1[i - 1] == s3[i + j - 1]);
            if (j > 0) dp[i][j] = dp[i][j] || (dp[i][j - 1] && s2[j - 1] == s3[i + j - 1]);
        }
    }
    return dp[m][n];
}
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
