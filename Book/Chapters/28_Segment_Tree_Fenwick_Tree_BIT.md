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

```cpp
#include <bits/stdc++.h>
using namespace std;

class FenwickTree {
public:
    int n;
    vector<long long> tree;

    FenwickTree(int n) : n(n), tree(n + 1, 0) {}

    void update(int i, long long delta) {
        while (i <= n) {
            tree[i] += delta;
            i += i & -i;
        }
    }

    long long query(int i) const {
        long long total = 0;
        while (i > 0) {
            total += tree[i];
            i -= i & -i;
        }
        return total;
    }

    long long range_query(int left, int right) const {
        return query(right) - query(left - 1);
    }
};

class SegmentTree {
public:
    int n;
    vector<long long> tree;

    SegmentTree(const vector<int>& nums) {
        n = nums.size();
        tree.assign(4 * max(1, n), 0);
        if (n > 0) build(nums, 0, 0, n - 1);
    }

    void build(const vector<int>& nums, int node, int start, int end) {
        if (start == end) {
            tree[node] = nums[start];
            return;
        }
        int mid = (start + end) / 2;
        build(nums, 2 * node + 1, start, mid);
        build(nums, 2 * node + 2, mid + 1, end);
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }

    void update(int idx, int val, int node, int start, int end) {
        if (start == end) {
            tree[node] = val;
            return;
        }
        int mid = (start + end) / 2;
        if (idx <= mid) update(idx, val, 2 * node + 1, start, mid);
        else update(idx, val, 2 * node + 2, mid + 1, end);
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }

    long long query(int left, int right, int node, int start, int end) const {
        if (right < start || end < left) return 0;
        if (left <= start && end <= right) return tree[node];
        int mid = (start + end) / 2;
        return query(left, right, 2 * node + 1, start, mid) + query(left, right, 2 * node + 2, mid + 1, end);
    }
};
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
