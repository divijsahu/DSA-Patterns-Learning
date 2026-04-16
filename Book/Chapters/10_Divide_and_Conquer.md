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

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> merge_vectors(const vector<int>& left, const vector<int>& right) {
    vector<int> result;
    int i = 0, j = 0;
    while (i < (int)left.size() && j < (int)right.size()) {
        if (left[i] <= right[j]) result.push_back(left[i++]);
        else result.push_back(right[j++]);
    }
    while (i < (int)left.size()) result.push_back(left[i++]);
    while (j < (int)right.size()) result.push_back(right[j++]);
    return result;
}

vector<int> merge_sort(const vector<int>& arr) {
    if (arr.size() <= 1) return arr;
    int mid = (int)arr.size() / 2;
    vector<int> left(arr.begin(), arr.begin() + mid);
    vector<int> right(arr.begin() + mid, arr.end());
    left = merge_sort(left);
    right = merge_sort(right);
    return merge_vectors(left, right);
}

int majority_helper(const vector<int>& nums, int lo, int hi) {
    if (lo == hi) return nums[lo];
    int mid = lo + (hi - lo) / 2;
    int left_major = majority_helper(nums, lo, mid);
    int right_major = majority_helper(nums, mid + 1, hi);
    if (left_major == right_major) return left_major;

    int left_count = 0, right_count = 0;
    for (int i = lo; i <= hi; ++i) {
        if (nums[i] == left_major) ++left_count;
        if (nums[i] == right_major) ++right_count;
    }
    return left_count > right_count ? left_major : right_major;
}

int majority_element(const vector<int>& nums) {
    return majority_helper(nums, 0, (int)nums.size() - 1);
}
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
