# Pattern 10 — Divide & Conquer

**Category:** Recursion · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Problem can be **split into two independent halves**, solved, and **merged**
- Classic sorting problems (merge sort, quick sort)
- Finding a **peak element** or **majority element**
- Tree problems that compute values by combining left and right subtree results
- Problem says: *"find peak"*, *"majority element"*, *"sort"*, *"closest pair"*

---

## 🧠 Intuition in Plain English

Divide & Conquer is the military strategy of "defeat in detail" — split the enemy force into small groups and defeat each group separately, then the whole army is defeated. For algorithms: break the problem in half, solve each half recursively (base case: size 1 or 2), then combine the results. The magic is that combining is cheap (O(n)), but splitting cuts the problem in half each time (log n levels), yielding O(n log n) total.

---

## 📐 Core Template

```python
# ─── MERGE SORT (classic D&C template) ───────────────────────────────────────
def merge_sort(arr):
    if len(arr) <= 1:
        return arr                         # Base case

    mid = len(arr) // 2
    left = merge_sort(arr[:mid])           # Conquer left
    right = merge_sort(arr[mid:])          # Conquer right
    return merge(left, right)              # Combine

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i]); i += 1
        else:
            result.append(right[j]); j += 1
    return result + left[i:] + right[j:]

# ─── MAJORITY ELEMENT (Boyer-Moore / D&C) ────────────────────────────────────
def majority_element(nums):
    def helper(lo, hi):
        if lo == hi:
            return nums[lo]                # Base case: single element

        mid = (lo + hi) // 2
        left_maj = helper(lo, mid)
        right_maj = helper(mid + 1, hi)

        if left_maj == right_maj:
            return left_maj

        left_count = sum(1 for i in range(lo, hi+1) if nums[i] == left_maj)
        right_count = sum(1 for i in range(lo, hi+1) if nums[i] == right_maj)
        return left_maj if left_count > right_count else right_maj

    return helper(0, len(nums) - 1)
```

---

## 📝 Problem Set

| # | Problem | Difficulty | D&C Insight |
|---|---------|------------|-------------|
| 1 | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) | 🟡 Medium | Max crossing mid = key merge step |
| 2 | [Majority Element](https://leetcode.com/problems/majority-element/) | 🟢 Easy | Majority in both halves = overall majority |
| 3 | [Find Peak Element](https://leetcode.com/problems/find-peak-element/) | 🟡 Medium | Peak must exist in direction of larger neighbor |
| 4 | [Sort an Array](https://leetcode.com/problems/sort-an-array/) | 🟡 Medium | Implement merge sort |
| 5 | [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) | 🔴 Hard | Merge sort with index tracking |
| 6 | [The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/) | 🔴 Hard | Merge sorted building outlines |

---

---

# 📊 Part 1 Quick Reference Card

| Pattern | Trigger | Complexity | Space |
|---------|---------|------------|-------|
| Prefix Sum | Range sum queries, count subarrays | O(n) build + O(1) query | O(n) |
| Hash Map | Existence, frequency, pairs | O(n) | O(n) |
| Two Pointers | Sorted array, in-place, pairs | O(n) | O(1) |
| Sliding Window | Contiguous subarray/string | O(n) | O(k) |
| Fast & Slow | Linked list cycle, middle | O(n) | O(1) |
| Binary Search | Sorted data, O(log n) needed | O(log n) | O(1) |
| BS on Answer | Minimize/maximize with feasibility check | O(n log n) | O(1) |
| Greedy | Local optimal = global optimal | O(n log n) | O(1) |
| Bit Manipulation | XOR, powers of 2, toggles | O(n) or O(1) | O(1) |
| Divide & Conquer | Split, solve halves, merge | O(n log n) | O(log n) |

---

> 📘 **Continue to Part 2** for Patterns 11–20: BFS, DFS, Tree DFS, Backtracking, and all 4 DP sub-patterns, plus Monotonic Stack and Queue.

---
*Part 1 of 3 — DSA Pattern Mastery Guide*
