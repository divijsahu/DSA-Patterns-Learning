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

```python
# ─── FIXED SIZE WINDOW ────────────────────────────────────────────────────────
def max_sum_fixed(arr, k):
    # O(n) instead of O(n*k)
    window_sum = sum(arr[:k])          # Initialize first window
    max_sum = window_sum

    for i in range(k, len(arr)):
        window_sum += arr[i] - arr[i - k]   # Add new tail, remove old head
        max_sum = max(max_sum, window_sum)

    return max_sum


# ─── VARIABLE SIZE WINDOW (find longest valid window) ────────────────────────
def longest_valid_window(s, k):
    # Example: longest substring with at most k distinct characters
    left = 0
    freq = {}                          # Tracks contents of the current window
    result = 0

    for right in range(len(s)):
        # ── EXPAND: add s[right] to window ───────────────────────────────────
        freq[s[right]] = freq.get(s[right], 0) + 1

        # ── SHRINK: while window is INVALID, move left ────────────────────────
        while len(freq) > k:           # Condition depends on the problem!
            freq[s[left]] -= 1
            if freq[s[left]] == 0:
                del freq[s[left]]
            left += 1

        # ── RECORD: window is now valid, update result ─────────────────────────
        result = max(result, right - left + 1)

    return result


# ─── VARIABLE SIZE WINDOW (find shortest valid window) ───────────────────────
def shortest_valid_window(s, target_counts):
    # Example: minimum window containing all target characters
    need = dict(target_counts)         # Characters still needed
    missing = sum(need.values())       # Total characters still needed
    left = 0
    best = (float('inf'), 0, 0)        # (length, left, right)

    for right, char in enumerate(s):
        if char in need:
            need[char] -= 1
            if need[char] == 0:
                missing -= 1           # One more character type is satisfied

        while missing == 0:            # Window is valid — try to shrink
            span = right - left + 1
            if span < best[0]:
                best = (span, left, right)

            if s[left] in need:
                need[s[left]] += 1
                if need[s[left]] > 0:
                    missing += 1       # Window becomes invalid after shrinking
            left += 1

    return s[best[1]: best[2] + 1] if best[0] != float('inf') else ""
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
