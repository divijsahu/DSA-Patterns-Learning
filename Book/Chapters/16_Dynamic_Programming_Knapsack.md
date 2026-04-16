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

```python
# ─── 0/1 KNAPSACK ─────────────────────────────────────────────────────────────
def knapsack_01(weights, values, capacity):
    n = len(weights)
    # dp[i][w] = max value using first i items with capacity w
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        w, v = weights[i-1], values[i-1]
        for cap in range(capacity + 1):
            # Option 1: Don't take item i
            dp[i][cap] = dp[i-1][cap]
            # Option 2: Take item i (if it fits)
            if cap >= w:
                dp[i][cap] = max(dp[i][cap], dp[i-1][cap - w] + v)

    return dp[n][capacity]


# ─── PARTITION EQUAL SUBSET SUM (0/1 Knapsack as boolean) ────────────────────
def can_partition(nums):
    total = sum(nums)
    if total % 2 != 0:
        return False

    target = total // 2
    # dp[j] = can we achieve sum j using a subset of nums?
    dp = [False] * (target + 1)
    dp[0] = True                       # Sum 0 is always achievable (empty subset)

    for num in nums:
        # Iterate BACKWARDS to prevent using same item twice
        for j in range(target, num - 1, -1):
            dp[j] = dp[j] or dp[j - num]

    return dp[target]


# ─── UNBOUNDED KNAPSACK (Coin Change) ─────────────────────────────────────────
def coin_change(coins, amount):
    # dp[i] = minimum coins to make amount i
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0                          # 0 coins needed to make 0

    for coin in coins:
        # Iterate FORWARD: items can be reused
        for i in range(coin, amount + 1):
            dp[i] = min(dp[i], dp[i - coin] + 1)

    return dp[amount] if dp[amount] != float('inf') else -1
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
