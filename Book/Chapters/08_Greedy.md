# Pattern 8 — Greedy

**Category:** Optimization · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Make a **sequence of choices** to reach a global optimum
- Each choice is **locally best** and doesn't need reconsideration
- Intervals, scheduling, jumps, or selection problems
- Problem says: *"minimum number of"*, *"maximum number of"*, *"can you reach"*, *"minimum cost to cover"*, *"earliest deadline"*

**The critical distinction — Greedy vs DP:**
- Greedy: local optimal = global optimal (no "undo" needed)
- DP: need to consider future consequences, choices interact

---

## 🧠 Intuition in Plain English

Greedy is the algorithm that always orders the cheapest coffee, then the cheapest sandwich, assuming that minimizing each item independently minimizes the total. It works when the problem has **greedy choice property** — the best local decision is always part of some global optimum. When in doubt between Greedy and DP, ask: "If I make the locally best choice, will I ever need to undo it?" If yes → DP. If no → Greedy.

---

## 🔮 Pattern Prediction Checklist

- [ ] Does the problem have **scheduling**, **intervals**, or **jumps**? → Greedy likely works
- [ ] Can I sort the input and make decisions in order? → Greedy
- [ ] Making the locally optimal choice **never needs undoing**? → Greedy (not DP)
- [ ] Is the answer about **existence** (can we reach X?) not count? → Often Greedy

---

## 📐 Core Template

```python
# ─── JUMP GAME (can we reach the end?) ───────────────────────────────────────
def can_jump(nums):
    max_reach = 0                       # Farthest index we can currently reach

    for i, jump in enumerate(nums):
        if i > max_reach:
            return False                # We're stuck, can't reach index i
        max_reach = max(max_reach, i + jump)   # Greedily extend reach

    return True

# ─── INTERVAL SCHEDULING (maximum non-overlapping) ───────────────────────────
def max_non_overlapping(intervals):
    # Greedy: always pick the interval that ends earliest
    intervals.sort(key=lambda x: x[1])   # Sort by END time
    count = 0
    last_end = float('-inf')

    for start, end in intervals:
        if start >= last_end:            # No overlap with last chosen
            count += 1
            last_end = end               # Greedily commit to this interval

    return count

# ─── GAS STATION (circular greedy) ───────────────────────────────────────────
def can_complete_circuit(gas, cost):
    total_surplus = 0
    current_surplus = 0
    start = 0

    for i in range(len(gas)):
        net = gas[i] - cost[i]
        total_surplus += net
        current_surplus += net

        if current_surplus < 0:          # Can't reach next station from 'start'
            start = i + 1               # Restart: try starting after this station
            current_surplus = 0

    return start if total_surplus >= 0 else -1
```

---

## ⏱️ Complexity

Usually O(n log n) if sorting is needed, O(n) if input already provides a natural ordering.

---

## ❌ Anti-patterns & Traps

- **Applying greedy when DP is needed:** Classic mistake on Coin Change (greedy fails with arbitrary denominations — use DP).
- **Wrong sorting key:** For interval problems, sort by **end time** (not start time) for maximum non-overlapping.

---

## 📝 Problem Set

| # | Problem | Difficulty | Greedy Choice |
|---|---------|------------|---------------|
| 1 | [Assign Cookies](https://leetcode.com/problems/assign-cookies/) | 🟢 Easy | Match smallest satisfying cookie |
| 2 | [Jump Game](https://leetcode.com/problems/jump-game/) | 🟡 Medium | Track maximum reach |
| 3 | [Jump Game II](https://leetcode.com/problems/jump-game-ii/) | 🟡 Medium | Greedy BFS levels |
| 4 | [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) | 🟡 Medium | Always keep earliest-ending interval |
| 5 | [Gas Station](https://leetcode.com/problems/gas-station/) | 🟡 Medium | Restart after negative surplus |
| 6 | [Candy](https://leetcode.com/problems/candy/) | 🔴 Hard | Two-pass greedy (left then right) |
| 7 | [Task Scheduler](https://leetcode.com/problems/task-scheduler/) | 🟡 Medium | Schedule most-frequent first |

---
---
