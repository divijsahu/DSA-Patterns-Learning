# Pattern 28 — Segment Tree / Fenwick Tree (BIT)

**Category:** Data Structure · **Difficulty Band:** Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Range queries** (sum, min, max) with **point updates** (values change)
- Need O(log n) for both updates and queries (Prefix Sum is O(n) for updates)
- **Range updates** with range queries
- Problem says: *"range sum query with updates"*, *"range minimum/maximum with updates"*, *"count inversions"*, *"rank of element"*

---

## 🧠 Intuition in Plain English

Prefix Sum is great for static arrays but breaks when values update (you'd have to rebuild the whole prefix array). Fenwick Tree (Binary Indexed Tree) stores partial sums in a clever bit-based structure, allowing O(log n) point updates AND O(log n) prefix queries. Segment Tree is more powerful: it supports range updates and range queries for any associative operation (sum, min, max, GCD).

---

## 📐 Core Template

```python
# ─── FENWICK TREE (BIT) — Point Update, Prefix Sum Query ─────────────────────
class FenwickTree:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (n + 1)     # 1-indexed

    def update(self, i, delta):
        """Add delta to index i (1-indexed)"""
        while i <= self.n:
            self.tree[i] += delta
            i += i & (-i)             # Move to next responsible index

    def query(self, i):
        """Get prefix sum from 1 to i (inclusive)"""
        total = 0
        while i > 0:
            total += self.tree[i]
            i -= i & (-i)             # Move to parent
        return total

    def range_query(self, left, right):
        """Get sum of elements from left to right (both 1-indexed)"""
        return self.query(right) - self.query(left - 1)


# ─── SEGMENT TREE — Range Update, Range Query ─────────────────────────────────
class SegmentTree:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (4 * self.n)
        self._build(nums, 0, 0, self.n - 1)

    def _build(self, nums, node, start, end):
        if start == end:
            self.tree[node] = nums[start]
        else:
            mid = (start + end) // 2
            self._build(nums, 2*node+1, start, mid)
            self._build(nums, 2*node+2, mid+1, end)
            self.tree[node] = self.tree[2*node+1] + self.tree[2*node+2]

    def update(self, idx, val, node=0, start=0, end=None):
        if end is None: end = self.n - 1
        if start == end:
            self.tree[node] = val
        else:
            mid = (start + end) // 2
            if idx <= mid:
                self.update(idx, val, 2*node+1, start, mid)
            else:
                self.update(idx, val, 2*node+2, mid+1, end)
            self.tree[node] = self.tree[2*node+1] + self.tree[2*node+2]

    def query(self, left, right, node=0, start=0, end=None):
        if end is None: end = self.n - 1
        if right < start or end < left:
            return 0                   # Out of range
        if left <= start and end <= right:
            return self.tree[node]     # Fully within range
        mid = (start + end) // 2
        return (self.query(left, right, 2*node+1, start, mid) +
                self.query(left, right, 2*node+2, mid+1, end))
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Which Tree |
|---|---------|------------|------------|
| 1 | [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/) | 🟡 Medium | Fenwick or Segment Tree |
| 2 | [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) | 🔴 Hard | Fenwick (coordinate compression) |
| 3 | [Count of Range Sum](https://leetcode.com/problems/count-of-range-sum/) | 🔴 Hard | Segment Tree or Merge Sort |
| 4 | [The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/) | 🔴 Hard | Segment Tree + coordinate compression |
| 5 | [My Calendar III](https://leetcode.com/problems/my-calendar-iii/) | 🔴 Hard | Segment Tree with lazy propagation |

---
---
