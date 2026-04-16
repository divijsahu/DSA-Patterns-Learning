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

```python
# ─── FIBONACCI / CLIMBING STAIRS ──────────────────────────────────────────────
def climb_stairs(n):
    if n <= 2: return n

    # dp[i] = number of ways to reach stair i
    prev2, prev1 = 1, 2

    for i in range(3, n + 1):
        curr = prev1 + prev2           # From stair i-1 (1 step) or i-2 (2 steps)
        prev2, prev1 = prev1, curr

    return prev1                       # Space optimized: only need last 2 values


# ─── HOUSE ROBBER ─────────────────────────────────────────────────────────────
def house_robber(nums):
    if len(nums) == 1: return nums[0]

    # dp[i] = max money from houses 0..i
    # Recurrence: dp[i] = max(dp[i-1], dp[i-2] + nums[i])
    prev2, prev1 = 0, 0

    for num in nums:
        curr = max(prev1, prev2 + num)
        prev2, prev1 = prev1, curr

    return prev1


# ─── WORD BREAK ───────────────────────────────────────────────────────────────
def word_break(s, word_dict):
    word_set = set(word_dict)
    n = len(s)

    # dp[i] = True if s[0..i-1] can be segmented
    dp = [False] * (n + 1)
    dp[0] = True                       # Empty string is always valid

    for i in range(1, n + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_set:   # s[j..i-1] is a valid word
                dp[i] = True
                break

    return dp[n]


# ─── DECODE WAYS ──────────────────────────────────────────────────────────────
def num_decodings(s):
    n = len(s)
    if s[0] == '0': return 0

    dp = [0] * (n + 1)
    dp[0] = 1                          # Empty string: 1 way
    dp[1] = 1                          # Single char (non-zero): 1 way

    for i in range(2, n + 1):
        one_digit = int(s[i-1])
        two_digits = int(s[i-2:i])

        if one_digit != 0:             # s[i-1] alone is valid
            dp[i] += dp[i-1]
        if 10 <= two_digits <= 26:     # s[i-2..i-1] together is valid
            dp[i] += dp[i-2]

    return dp[n]
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
