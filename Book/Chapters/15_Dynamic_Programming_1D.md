# Pattern 15 — Dynamic Programming: 1D

**Category:** DP · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- A sequence of decisions, each dependent on **a fixed number of previous states**
- **"How many ways"**, **"can you reach"**, **"minimum cost"** in a linear structure
- Fibonacci-like recurrences (each state depends on 1–2 previous states)
- Problem says: *"climbing stairs"*, *"house robber"*, *"decode ways"*, *"word break"*, *"minimum jumps"*

---

## 🧠 Intuition in Plain English

1D DP is like tracking your bank balance — each day's balance depends only on yesterday's. You don't re-simulate every transaction from day 1. You store the result of each subproblem (a day's balance) and build forward. The recurrence relation is the mathematical rule that says: "Today's answer = f(yesterday's answer, some current value)."

**The 5-step DP recipe:**
1. Define what `dp[i]` means in plain English
2. Identify the base cases
3. Write the recurrence relation
4. Fill the table (bottom-up) or memoize (top-down)
5. Return the answer (often `dp[n]` or `max(dp)`)

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

int climbStairs(int n) {
    if (n <= 2) return n;
    int prev2 = 1, prev1 = 2;
    for (int i = 3; i <= n; ++i) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}

int houseRobber(const vector<int>& nums) {
    if (nums.size() == 1) return nums[0];
    int prev2 = 0, prev1 = 0;
    for (int num : nums) {
        int curr = max(prev1, prev2 + num);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}

bool wordBreak(const string& s, const vector<string>& wordDict) {
    unordered_set<string> wordSet(wordDict.begin(), wordDict.end());
    int n = s.size();
    vector<bool> dp(n + 1, false);
    dp[0] = true;
    for (int i = 1; i <= n; ++i) {
        for (int j = 0; j < i; ++j) {
            if (dp[j] && wordSet.count(s.substr(j, i - j))) {
                dp[i] = true;
                break;
            }
        }
    }
    return dp[n];
}

int numDecodings(const string& s) {
    if (s.empty() || s[0] == '0') return 0;
    int n = s.size();
    vector<int> dp(n + 1, 0);
    dp[0] = 1;
    dp[1] = 1;
    for (int i = 2; i <= n; ++i) {
        int one_digit = s[i - 1] - '0';
        int two_digits = stoi(s.substr(i - 2, 2));
        if (one_digit != 0) dp[i] += dp[i - 1];
        if (10 <= two_digits && two_digits <= 26) dp[i] += dp[i - 2];
    }
    return dp[n];
}
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Recurrence |
|---|---------|------------|------------|
| 1 | [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) | 🟢 Easy | `dp[i] = dp[i-1] + dp[i-2]` |
| 2 | [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/) | 🟢 Easy | `dp[i] = min(dp[i-1], dp[i-2]) + cost[i]` |
| 3 | [House Robber](https://leetcode.com/problems/house-robber/) | 🟡 Medium | `dp[i] = max(dp[i-1], dp[i-2]+nums[i])` |
| 4 | [House Robber II](https://leetcode.com/problems/house-robber-ii/) | 🟡 Medium | Run robber twice on sliced arrays |
| 5 | [Decode Ways](https://leetcode.com/problems/decode-ways/) | 🟡 Medium | 1-digit + 2-digit transitions |
| 6 | [Word Break](https://leetcode.com/problems/word-break/) | 🟡 Medium | `dp[i] = any(dp[j] and s[j:i] in dict)` |
| 7 | [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) | 🟡 Medium | `dp[i] = max(dp[j]+1) where nums[j]<nums[i]` |
| 8 | [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/) | 🟡 Medium | Track max AND min (negatives flip sign) |

---
---
