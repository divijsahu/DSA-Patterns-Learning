# Pattern 20 — Monotonic Queue (Deque)

**Category:** Queue · **Difficulty Band:** Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Sliding window maximum or minimum** (fixed-size window that slides)
- Need to efficiently track max/min as a window moves
- Problems where you need the maximum/minimum in a recent window of size K
- Problem says: *"sliding window maximum"*, *"maximum in window"*, *"shortest subarray"*

---

## 🧠 Intuition in Plain English

A regular sliding window tracks content with a hash map. But if you need the *maximum* of the window at all times, a hash map is O(log n) or O(n) to find the max. A monotonic deque maintains a window's maximum in amortized O(1): **the front of the deque is always the current maximum**. Elements that can never be the maximum (because a larger element entered after them) are kicked out the back immediately.

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> maxSlidingWindow(const vector<int>& nums, int k) {
    deque<int> dq;
    vector<int> result;
    for (int i = 0; i < (int)nums.size(); ++i) {
        while (!dq.empty() && dq.front() <= i - k) dq.pop_front();
        while (!dq.empty() && nums[dq.back()] <= nums[i]) dq.pop_back();
        dq.push_back(i);
        if (i >= k - 1) result.push_back(nums[dq.front()]);
    }
    return result;
}
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Insight |
|---|---------|------------|---------|
| 1 | [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) | 🔴 Hard | Classic monotonic deque |
| 2 | [Shortest Subarray with Sum at Least K](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/) | 🔴 Hard | Prefix sum + monotonic deque |
| 3 | [Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum/) | 🔴 Hard | DP + monotonic deque for window max |

---

---

# 📊 Part 2 Quick Reference Card

| Pattern | Core Data Structure | Key Trigger |
|---------|---------------------|-------------|
| BFS | Queue (deque) | Shortest path, level order, minimum steps |
| DFS | Stack / Recursion | All paths, connectivity, cycle detection |
| Tree DFS | Recursion | Post-order: aggregate from children upward |
| Backtracking | Recursion + Undo | Generate all valid combinations |
| DP 1D | Array | Linear recurrence, sequential choices |
| DP Knapsack | Array | Include/exclude items under capacity |
| DP 2D/Strings | 2D Array | Two sequences, grid traversal |
| DP Interval | 2D Array | Split range at pivot, fill by length |
| Monotonic Stack | Stack | Next/prev greater/smaller element |
| Monotonic Queue | Deque | Sliding window max/min |

---

> 📘 **Continue to Part 3** for Patterns 21–30: Heap, Intervals, Graph Algorithms, Union-Find, Trie, Segment Tree, Linked List Manipulation, Matrix Patterns, and more — plus the complete 30-pattern cheat sheet.

---
*Part 2 of 3 — DSA Pattern Mastery Guide*
