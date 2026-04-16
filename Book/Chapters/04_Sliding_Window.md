# Pattern 4 — Sliding Window

**Category:** Array/String · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Contiguous** subarray or substring
- Find the **longest/shortest** subarray satisfying a condition
- Optimize over subarrays of **fixed size K**
- Count subarrays with a property
- Problem says: *"subarray"*, *"substring"*, *"window"*, *"contiguous"*, *"at most K distinct"*, *"without repeating"*

**This pattern is NOT for:**
- Non-contiguous subsequences (use DP or Two Pointers)
- Pair problems in sorted arrays (use Two Pointers)

---

## 🧠 Intuition in Plain English

Imagine a magnifying glass sliding across a newspaper. Instead of re-reading the entire visible text after each slide, you simply forget the leftmost letter and read the new rightmost letter. The window's "memory" is maintained as a running state. This makes O(n²) brute-force into O(n): every element enters the window once and leaves once.

---

## 🔮 Pattern Prediction Checklist

- [ ] Is the problem about a **contiguous** subarray or substring? → Sliding Window
- [ ] Is the window size **fixed**? → Fixed Window template
- [ ] Is the window size **variable** (find longest/shortest)? → Variable Window template
- [ ] Is there a **constraint** on window contents (at most K distinct chars, sum ≤ target)? → Variable Window with shrink condition

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

int max_sum_fixed(const vector<int>& arr, int k) {
    int window_sum = 0;
    for (int i = 0; i < k; ++i) window_sum += arr[i];
    int best = window_sum;
    for (int i = k; i < (int)arr.size(); ++i) {
        window_sum += arr[i] - arr[i - k];
        best = max(best, window_sum);
    }
    return best;
}

int longest_valid_window(const string& s, int k) {
    unordered_map<char, int> freq;
    int left = 0;
    int result = 0;
    for (int right = 0; right < (int)s.size(); ++right) {
        freq[s[right]]++;
        while ((int)freq.size() > k) {
            if (--freq[s[left]] == 0) freq.erase(s[left]);
            ++left;
        }
        result = max(result, right - left + 1);
    }
    return result;
}

string shortest_valid_window(const string& s, const unordered_map<char, int>& target_counts) {
    unordered_map<char, int> need = target_counts;
    int missing = 0;
    for (const auto& entry : need) missing += entry.second;

    int left = 0;
    int best_len = INT_MAX;
    int best_left = 0;

    for (int right = 0; right < (int)s.size(); ++right) {
        auto it = need.find(s[right]);
        if (it != need.end()) {
            if (it->second > 0) --missing;
            --it->second;
        }

        while (missing == 0) {
            int window_len = right - left + 1;
            if (window_len < best_len) {
                best_len = window_len;
                best_left = left;
            }

            auto left_it = need.find(s[left]);
            if (left_it != need.end()) {
                ++left_it->second;
                if (left_it->second > 0) ++missing;
            }
            ++left;
        }
    }

    return best_len == INT_MAX ? string() : s.substr(best_left, best_len);
}
```

---

## 🔀 Variants

| Variant | Template | Example Problem |
|---------|----------|-----------------|
| **Fixed size** | Add new, remove old element | Max sum subarray of size K |
| **Variable, find longest** | Expand right, shrink from left when invalid | Longest substring without repeating |
| **Variable, find shortest** | Expand until valid, then shrink while valid | Minimum window substring |
| **Count valid windows** | Track how many windows satisfy condition | Number of subarrays with product < K |

---

## ⏱️ Complexity

| | Time | Space |
|-|------|-------|
| Fixed Window | O(n) | O(1) |
| Variable Window | O(n) | O(k) — k = window state size |

The key insight: **each element is added and removed at most once** → O(n) total.

---

## ❌ Anti-patterns & Traps

- **Adding to `visited` when popping (not when adding):** Add to freq when `right` expands, remove when `left` shrinks.
- **Fixed vs variable confusion:** If window size is K, use fixed template. If you're optimizing the window size, use variable.
- **Forgetting to delete key from dict:** When `freq[char]` hits 0, delete the key or `len(freq)` stays artificially high.

---

## 📝 Problem Set

| # | Problem | Difficulty | Key Insight |
|---|---------|------------|-------------|
| 1 | [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/) | 🟢 Easy | Fixed window of size k |
| 2 | [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) | 🟢 Easy | Track running minimum to the left |
| 3 | [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | 🟡 Medium | Shrink when duplicate enters window |
| 4 | [Permutation in String](https://leetcode.com/problems/permutation-in-string/) | 🟡 Medium | Fixed window, track char count match |
| 5 | [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/) | 🟡 Medium | `window_size - max_freq ≤ k` is the validity check |
| 6 | [Fruits Into Baskets](https://leetcode.com/problems/fruit-into-baskets/) | 🟡 Medium | At most 2 distinct in window |
| 7 | [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) | 🔴 Hard | Shrink-while-valid variant |
| 8 | [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) | 🔴 Hard | Combine with Monotonic Deque |

---
---
