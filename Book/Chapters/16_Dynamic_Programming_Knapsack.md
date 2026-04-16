# Pattern 16 — Dynamic Programming: Knapsack

**Category:** DP · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Choose a **subset** of items subject to a **capacity/budget constraint**
- Maximize value or determine if a **target sum is achievable**
- **Include or exclude** each element is the fundamental decision
- Problem says: *"partition equal subset"*, *"target sum"*, *"can you achieve sum W"*, *"bounded items"*, *"unbounded coins"*

**Two main subtypes:**
- **0/1 Knapsack:** Each item used at most once
- **Unbounded Knapsack:** Each item can be used unlimited times (Coin Change)

---

## 🧠 Intuition in Plain English

You're packing a hiking bag with a weight limit. For each item, you face a binary decision: pack it or leave it. The total packing strategy can't be decided greedily — a cheap item now might block a much more valuable item later. DP solves this by recording the optimal packing for every possible remaining weight, building up from weight 0 to the limit.

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

int knapsack_01(const vector<int>& weights, const vector<int>& values, int capacity) {
    int n = weights.size();
    vector<vector<int>> dp(n + 1, vector<int>(capacity + 1, 0));
    for (int i = 1; i <= n; ++i) {
        int w = weights[i - 1], v = values[i - 1];
        for (int cap = 0; cap <= capacity; ++cap) {
            dp[i][cap] = dp[i - 1][cap];
            if (cap >= w) dp[i][cap] = max(dp[i][cap], dp[i - 1][cap - w] + v);
        }
    }
    return dp[n][capacity];
}

bool canPartition(const vector<int>& nums) {
    int total = accumulate(nums.begin(), nums.end(), 0);
    if (total % 2 != 0) return false;
    int target = total / 2;
    vector<bool> dp(target + 1, false);
    dp[0] = true;
    for (int num : nums) {
        for (int j = target; j >= num; --j) {
            dp[j] = dp[j] || dp[j - num];
        }
    }
    return dp[target];
}

int coinChange(const vector<int>& coins, int amount) {
    const int INF = 1e9;
    vector<int> dp(amount + 1, INF);
    dp[0] = 0;
    for (int coin : coins) {
        for (int i = coin; i <= amount; ++i) {
            dp[i] = min(dp[i], dp[i - coin] + 1);
        }
    }
    return dp[amount] == INF ? -1 : dp[amount];
}
```

---

## 🔑 The Key Loop Direction Rule

```
0/1 Knapsack (each item once):    iterate j from HIGH → LOW (backward)
Unbounded Knapsack (reuse ok):    iterate j from LOW  → HIGH (forward)
```

This single rule prevents the most common knapsack bugs.

---

## 📝 Problem Set

| # | Problem | Difficulty | Knapsack Type |
|---|---------|------------|---------------|
| 1 | [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/) | 🟡 Medium | 0/1 boolean |
| 2 | [Coin Change](https://leetcode.com/problems/coin-change/) | 🟡 Medium | Unbounded, minimize count |
| 3 | [Coin Change II](https://leetcode.com/problems/coin-change-ii/) | 🟡 Medium | Unbounded, count ways |
| 4 | [Target Sum](https://leetcode.com/problems/target-sum/) | 🟡 Medium | 0/1 count ways |
| 5 | [Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/) | 🟡 Medium | Partition into equal sums |
| 6 | [Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/) | 🟡 Medium | 2D knapsack (m zeros, n ones budget) |
| 7 | [Perfect Squares](https://leetcode.com/problems/perfect-squares/) | 🟡 Medium | Unbounded: min squares to sum to n |

---
---
