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

```python
# ─── INTERVAL DP TEMPLATE ─────────────────────────────────────────────────────
def interval_dp(arr):
    n = len(arr)
    # dp[i][j] = answer for subarray arr[i..j]
    dp = [[0] * n for _ in range(n)]

    # Fill by increasing interval LENGTH
    for length in range(2, n + 1):              # Length from 2 to n
        for i in range(n - length + 1):
            j = i + length - 1                  # Right endpoint

            dp[i][j] = float('inf')             # Or -inf, depending on problem

            for k in range(i, j):               # Try every split point
                # Combine dp[i][k] and dp[k+1][j]
                cost = dp[i][k] + dp[k+1][j] + merge_cost(arr, i, j, k)
                dp[i][j] = min(dp[i][j], cost)

    return dp[0][n-1]


# ─── BURST BALLOONS ───────────────────────────────────────────────────────────
def max_coins(nums):
    # Add boundary balloons with value 1
    nums = [1] + nums + [1]
    n = len(nums)
    dp = [[0] * n for _ in range(n)]

    # dp[i][j] = max coins from bursting all balloons between i and j (exclusive)
    for length in range(2, n):
        for left in range(0, n - length):
            right = left + length
            for k in range(left + 1, right):   # k = last balloon to burst in range
                coins = nums[left] * nums[k] * nums[right]
                dp[left][right] = max(dp[left][right],
                                      dp[left][k] + coins + dp[k][right])

    return dp[0][n-1]
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
