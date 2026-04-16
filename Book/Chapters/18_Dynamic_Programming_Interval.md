# Pattern 18 — Dynamic Programming: Interval

**Category:** DP · **Difficulty Band:** Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Problems on **contiguous segments** of an array where you make a decision about the whole segment
- Merging or splitting arrays for minimum/maximum cost
- Matrix chain multiplication style problems
- Problem says: *"burst balloons"*, *"remove boxes"*, *"strange printer"*, *"minimum cost to merge stones"*

---

## 🧠 Intuition in Plain English

Interval DP solves problems where the answer for a range `[i, j]` depends on splitting it at some pivot `k` and combining the results of `[i, k]` and `[k+1, j]`. You build up from smallest intervals (length 1) to the full range. The key insight: unlike 1D DP which fills a line, interval DP fills a triangle of the 2D table.

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

int burstBalloons(vector<int> nums) {
    int n = nums.size();
    nums.insert(nums.begin(), 1);
    nums.push_back(1);
    vector<vector<int>> dp(n + 2, vector<int>(n + 2, 0));

    for (int len = 2; len < n + 2; ++len) {
        for (int left = 0; left + len < n + 2; ++left) {
            int right = left + len;
            for (int last = left + 1; last < right; ++last) {
                dp[left][right] = max(dp[left][right], dp[left][last] + nums[left] * nums[last] * nums[right] + dp[last][right]);
            }
        }
    }
    return dp[0][n + 1];
}
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Interval Insight |
|---|---------|------------|-----------------|
| 1 | [Minimum Cost Tree from Leaf Values](https://leetcode.com/problems/minimum-cost-tree-from-leaf-values/) | 🟡 Medium | Split leaf arrays |
| 2 | [Burst Balloons](https://leetcode.com/problems/burst-balloons/) | 🔴 Hard | Last balloon to burst, not first |
| 3 | [Strange Printer](https://leetcode.com/problems/strange-printer/) | 🔴 Hard | When s[i] == s[j], extend interval |
| 4 | [Minimum Cost to Merge Stones](https://leetcode.com/problems/minimum-cost-to-merge-stones/) | 🔴 Hard | Only merge k consecutive piles |
| 5 | [Remove Boxes](https://leetcode.com/problems/remove-boxes/) | 🔴 Hard | 3D DP variant |

---
---
