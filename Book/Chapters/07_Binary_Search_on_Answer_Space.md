# Pattern 7 — Binary Search on Answer Space

**Category:** Search/Optimization · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Find the **minimum** value that satisfies a condition
- Find the **maximum** value that still satisfies a constraint
- The answer is a number in a **range** and you can check if a candidate answer is valid
- Problem says: *"minimum speed"*, *"maximum capacity"*, *"split into K parts"*, *"minimize the maximum"*, *"can you finish in D days"*

**THIS IS THE MOST UNDERUSED PATTERN.** Many hard problems become medium when you recognize that you can binary search on the *answer*.

---

## 🧠 Intuition in Plain English

Instead of searching for the answer in the input data, you binary search over the *answer values* themselves. The key insight: **the validity of answers is monotonic** — if speed X lets Koko eat all bananas, then any speed > X also works. This creates a sorted true/false boundary, and you binary search for the boundary.

```
Can answer = 1 work? → NO
Can answer = 2 work? → NO
Can answer = 5 work? → YES  ← Find this leftmost YES
Can answer = 6 work? → YES
```

---

## 🔮 Pattern Prediction Checklist

This pattern applies when ALL of the following are true:
- [ ] The answer is a **number** (not a path, not an array)
- [ ] You can determine **valid bounds** for the answer (min possible, max possible)
- [ ] The validity check is **monotonic**: if X works, all values > X also work (or vice versa)
- [ ] Checking if a given answer is valid takes **O(n) or less**

If all 4 hold → Binary Search on Answer, giving O(n log n) total.

---

## 📐 Core Template

```python
# ─── BINARY SEARCH ON ANSWER (find minimum valid) ────────────────────────────
def binary_search_answer(arr, constraint):

    # Step 1: Define the search space
    left = min_possible_answer          # E.g., 1, min(arr), or 0
    right = max_possible_answer         # E.g., sum(arr), max(arr)

    # Step 2: Binary search the boundary
    while left < right:                 # NOT <=, we want the boundary
        mid = left + (right - left) // 2

        if is_feasible(arr, mid, constraint):
            right = mid                 # Valid! Try to find something smaller
        else:
            left = mid + 1              # Invalid, need to go bigger

    return left                         # left == right == the minimum valid answer


# ─── EXAMPLE: KOKO EATING BANANAS ────────────────────────────────────────────
def min_eating_speed(piles, h):
    import math

    def can_finish(speed):
        # Can Koko finish all piles at this speed in h hours?
        hours_needed = sum(math.ceil(pile / speed) for pile in piles)
        return hours_needed <= h

    left, right = 1, max(piles)         # Speed between 1 and max pile size

    while left < right:
        mid = left + (right - left) // 2
        if can_finish(mid):
            right = mid                 # Try slower
        else:
            left = mid + 1             # Need to go faster

    return left


# ─── EXAMPLE: SPLIT ARRAY LARGEST SUM ────────────────────────────────────────
def split_array(nums, k):

    def is_feasible(max_sum):
        # Can we split nums into k parts where each part sum ≤ max_sum?
        parts = 1
        current_sum = 0
        for num in nums:
            if current_sum + num > max_sum:
                parts += 1             # Start new partition
                current_sum = num
                if parts > k:
                    return False
            else:
                current_sum += num
        return True

    left = max(nums)                    # At minimum, max single element
    right = sum(nums)                   # At maximum, entire array in one part

    while left < right:
        mid = left + (right - left) // 2
        if is_feasible(mid):
            right = mid
        else:
            left = mid + 1

    return left
```

---

## ⏱️ Complexity

| | Time | Space |
|-|------|-------|
| Total | O(n log(max - min)) | O(1) |
| Typical | O(n log n) | O(1) |

The `is_feasible` check is O(n), called O(log(range)) times → O(n log range) total.

---

## ❌ Anti-patterns & Traps

- **Wrong search space bounds:** `left` should be the smallest possible answer (often `1` or `max(arr)`), `right` the largest (`sum(arr)`).
- **Wrong termination:** Use `left < right` (not `<=`), and return `left` at the end.
- **Non-monotonic feasibility:** Make sure `is_feasible` is truly monotonic — if X works, X+1 also works. If not, binary search won't apply.

---

## 📝 Problem Set

| # | Problem | Difficulty | Answer Range |
|---|---------|------------|--------------|
| 1 | [First Bad Version](https://leetcode.com/problems/first-bad-version/) | 🟢 Easy | 1 to n |
| 2 | [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) | 🟡 Medium | 1 to max(piles) |
| 3 | [Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) | 🟡 Medium | max(weights) to sum(weights) |
| 4 | [Minimum Number of Days to Make m Bouquets](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/) | 🟡 Medium | 1 to max(bloomDay) |
| 5 | [Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/) | 🔴 Hard | max(nums) to sum(nums) |
| 6 | [Minimize Max Distance to Gas Station](https://leetcode.com/problems/minimize-max-distance-to-gas-station/) | 🔴 Hard | Binary search on floating-point range |

---
---
