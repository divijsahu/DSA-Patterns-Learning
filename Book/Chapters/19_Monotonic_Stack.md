# Pattern 19 — Monotonic Stack

**Category:** Stack · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- For each element, find the **next/previous greater or smaller element**
- **Histogram** area problems
- Problems involving **spans** (stock span, daily temperatures)
- Sliding window minimum/maximum (→ use Monotonic Queue, Pattern 20)
- Problem says: *"next greater element"*, *"daily temperatures"*, *"stock span"*, *"largest rectangle"*, *"trap water"*

---

## 🧠 Intuition in Plain English

A monotonic stack maintains elements in sorted order (increasing or decreasing). When a new element breaks the sort order, it becomes the "answer" for every element it pops off. Imagine a queue of people waiting at a coffee shop — whenever a taller person arrives, everyone shorter in front of them can finally see who's ahead of them (the taller person is their "next greater element"). The stack holds the "still waiting" elements.

---

## 📐 Core Template

```python
# ─── NEXT GREATER ELEMENT (decreasing stack) ─────────────────────────────────
def next_greater(nums):
    n = len(nums)
    result = [-1] * n                  # Default: no greater element exists
    stack = []                         # Stack of indices, values decrease bottom→top

    for i in range(n):
        # Current element is GREATER than stack top → it's the answer for top
        while stack and nums[i] > nums[stack[-1]]:
            idx = stack.pop()
            result[idx] = nums[i]      # nums[i] is next greater for nums[idx]
        stack.append(i)

    return result

# Monotonic INCREASING stack → finds next SMALLER element
# Monotonic DECREASING stack → finds next GREATER element


# ─── LARGEST RECTANGLE IN HISTOGRAM ──────────────────────────────────────────
def largest_rectangle(heights):
    stack = []                         # Monotonic increasing stack of indices
    max_area = 0
    heights = heights + [0]            # Sentinel 0 flushes all remaining bars

    for i, h in enumerate(heights):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            # Width: from current position back to previous smaller bar
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)

    return max_area


# ─── DAILY TEMPERATURES ───────────────────────────────────────────────────────
def daily_temperatures(temps):
    n = len(temps)
    result = [0] * n
    stack = []                         # Stack of indices

    for i in range(n):
        while stack and temps[i] > temps[stack[-1]]:
            idx = stack.pop()
            result[idx] = i - idx      # Days until warmer = index difference
        stack.append(i)

    return result
```

---

## 🔑 Key Insight

```
When you POP from the stack, the current element is the answer for the popped element.
What "answer" means depends on the problem:
  - Next greater: result[popped_idx] = current_value
  - Days until warmer: result[popped_idx] = current_idx - popped_idx
  - Largest rectangle: area = height[popped] × width_calculation
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Stack Type |
|---|---------|------------|------------|
| 1 | [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/) | 🟢 Easy | Decreasing stack |
| 2 | [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) | 🟡 Medium | Days until warmer |
| 3 | [Online Stock Span](https://leetcode.com/problems/online-stock-span/) | 🟡 Medium | Previous greater (reverse) |
| 4 | [Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums/) | 🟡 Medium | Next/prev smaller + contribution |
| 5 | [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) | 🔴 Hard | Increasing stack + area calc |
| 6 | [Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/) | 🔴 Hard | Histogram per row of matrix |
| 7 | [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) | 🔴 Hard | Stack or two-pointer both work |

---
---
