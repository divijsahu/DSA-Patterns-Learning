# Pattern 1 — Prefix Sum

**Category:** Array · **Difficulty Band:** Easy–Medium

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Query for **sum of a subarray** `[i, j]` multiple times
- Problem asks for **number of subarrays** with a given sum, average, or divisibility
- Need to quickly compute cumulative totals without re-summing
- Problem says: *"range sum"*, *"subarray sum equals K"*, *"between index i and j"*

**This pattern is NOT for:**
- Finding a single subarray max/min (use Sliding Window)
- Problems where values update frequently (use Segment Tree)

---

## 🧠 Intuition in Plain English

Imagine you're a cashier and 100 customers ask you the total sales between hour 3 and hour 9 of a 24-hour day. Instead of re-adding hours 3–9 for each customer, you pre-compute a running total once. Then any range query becomes: `total_up_to_hour_9 - total_up_to_hour_2`. That's prefix sum — **pay the O(n) cost once, answer every query in O(1).**

The magic formula: `sum(i, j) = prefix[j+1] - prefix[i]`

---

## 🔮 Pattern Prediction Checklist

Ask yourself these before coding:
- [ ] Am I querying **sums of subarrays** more than once? → YES → Prefix Sum
- [ ] Is the problem asking *how many* subarrays have a certain property? → YES → Prefix Sum + Hash Map
- [ ] Do I need cumulative counts, not just sums? → YES → Prefix Sum variant

**If 1+ boxes checked → use Prefix Sum.**

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> build_prefix(const vector<int>& nums) {
    int n = nums.size();
    vector<int> prefix(n + 1, 0);
    for (int i = 0; i < n; ++i) {
        prefix[i + 1] = prefix[i] + nums[i];
    }
    return prefix;
}

int range_sum(const vector<int>& prefix, int left, int right) {
    return prefix[right + 1] - prefix[left];
}

int subarray_sum_equals_k(const vector<int>& nums, int k) {
    unordered_map<int, int> seen;
    seen[0] = 1;
    int running_sum = 0;
    int count = 0;

    for (int num : nums) {
        running_sum += num;
        auto it = seen.find(running_sum - k);
        if (it != seen.end()) {
            count += it->second;
        }
        seen[running_sum] += 1;
    }

    return count;
}
```

---

## 🔀 Variants

| Variant | Modification |
|---------|-------------|
| **2D Prefix Sum** | `prefix[i][j] = prefix[i-1][j] + prefix[i][j-1] - prefix[i-1][j-1] + matrix[i][j]` |
| **Divisibility by K** | Store `running_sum % k` in hash map instead of `running_sum` |
| **Running XOR** | Replace `+` with `^` to find subarrays with XOR = target |

---

## ⏱️ Complexity

| | Brute Force | Prefix Sum |
|-|-------------|------------|
| **Preprocessing** | O(1) | O(n) |
| **Each Query** | O(n) | O(1) |
| **Q Queries Total** | O(n·Q) | O(n + Q) |
| **Space** | O(1) | O(n) |

---

## ❌ Anti-patterns & Traps

- **Off-by-one:** `prefix[j+1] - prefix[i]` (not `prefix[j] - prefix[i]`). Build `prefix` with length `n+1`.
- **Missing seed in hash map:** Always initialize `seen = {0: 1}` for the "subarray sum = K" variant, or you miss subarrays starting at index 0.
- **Mutating input:** Build a new prefix array, don't modify `nums`.

---

## 📝 Problem Set

| # | Problem | Difficulty | Why This Pattern |
|---|---------|------------|------------------|
| 1 | [Running Sum of 1D Array](https://leetcode.com/problems/running-sum-of-1d-array/) | 🟢 Easy | Literal prefix sum construction |
| 2 | [Find Pivot Index](https://leetcode.com/problems/find-pivot-index/) | 🟢 Easy | Left prefix = total - right prefix |
| 3 | [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/) | 🟢 Easy | Classic repeated range queries |
| 4 | [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) | 🟡 Medium | Prefix + hash map for count |
| 5 | [Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/) | 🟡 Medium | Prefix mod K variant |
| 6 | [Count of Range Sum](https://leetcode.com/problems/count-of-range-sum/) | 🔴 Hard | Prefix + merge sort / BIT |

---
---
