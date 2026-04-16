# Pattern 2 — Hash Map / Frequency Count

**Category:** Array/String · **Difficulty Band:** Easy–Medium

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Check if an element **was seen before**
- Find **pairs, duplicates, or complements**
- Count **frequency** of characters or numbers
- "Two Sum" style: find two elements that sum to target
- Problem says: *"find if exists"*, *"count occurrences"*, *"group by"*, *"anagram"*

**This pattern is NOT for:**
- Problems needing sorted order (use sorted array or BST)
- Range queries (use Prefix Sum or Segment Tree)

---

## 🧠 Intuition in Plain English

A hash map is a **memory palace** — you trade space for instant lookup. Instead of searching the entire array for a complement, you ask: "Have I seen `target - current` before?" The moment you've seen it, you have your answer. This converts O(n²) search loops into O(n) single passes.

---

## 🔮 Pattern Prediction Checklist

- [ ] Do I need to know if something **exists** or how often it appears? → Hash Map
- [ ] Am I looking for a **pair** that satisfies a condition (sum, difference, product)? → Hash Map
- [ ] Do I need to **group** elements by some property (anagram grouping, etc.)? → Hash Map

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> two_sum(const vector<int>& nums, int target) {
    unordered_map<int, int> seen;
    for (int i = 0; i < (int)nums.size(); ++i) {
        int complement = target - nums[i];
        auto it = seen.find(complement);
        if (it != seen.end()) {
            return {it->second, i};
        }
        seen[nums[i]] = i;
    }
    return {};
}

bool is_anagram(const string& s, const string& t) {
    if (s.size() != t.size()) return false;
    unordered_map<char, int> freq;
    for (char c : s) freq[c]++;
    for (char c : t) {
        auto it = freq.find(c);
        if (it == freq.end()) return false;
        if (--it->second < 0) return false;
    }
    return true;
}

vector<vector<string>> group_anagrams(const vector<string>& strs) {
    unordered_map<string, vector<string>> groups;
    for (string word : strs) {
        string key = word;
        sort(key.begin(), key.end());
        groups[key].push_back(word);
    }

    vector<vector<string>> result;
    for (auto& entry : groups) result.push_back(move(entry.second));
    return result;
}
```

---

## 🔀 Variants

| Variant | Use Case |
|---------|----------|
| **`Counter`** | Frequency count in O(n), supports arithmetic |
| **`defaultdict(list)`** | Grouping items by key |
| **`defaultdict(int)`** | Running counts without KeyError |
| **Set instead of dict** | When you only need existence, not count/index |

---

## ⏱️ Complexity

| | Brute Force | Hash Map |
|-|-------------|----------|
| **Lookup** | O(n) | O(1) average |
| **Total** | O(n²) | O(n) |
| **Space** | O(1) | O(n) |

---

## ❌ Anti-patterns & Traps

- **Using same element twice:** In Two Sum, store the index *after* checking the complement.
- **Hash collisions in interviews:** Know that worst-case is O(n), not O(1).
- **Forgetting sets are faster:** If you only need existence (not count), `set` is cleaner than `dict`.

---

## 📝 Problem Set

| # | Problem | Difficulty | Why This Pattern |
|---|---------|------------|------------------|
| 1 | [Two Sum](https://leetcode.com/problems/two-sum/) | 🟢 Easy | Classic complement lookup |
| 2 | [Valid Anagram](https://leetcode.com/problems/valid-anagram/) | 🟢 Easy | Frequency comparison |
| 3 | [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/) | 🟢 Easy | Existence check with set |
| 4 | [Group Anagrams](https://leetcode.com/problems/group-anagrams/) | 🟡 Medium | Group by canonical key |
| 5 | [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/) | 🟡 Medium | Set + streak detection |
| 6 | [4Sum II](https://leetcode.com/problems/4sum-ii/) | 🟡 Medium | Two-pass hash map |
| 7 | [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) | 🟡 Medium | Hash map + prefix sum combo |

---
---
