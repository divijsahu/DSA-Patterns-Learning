# ⚡ THE COMPLETE DSA PATTERN MASTERY GUIDE
## Part 1 of 3 — Foundation Patterns (Patterns 1–10)

> *"An expert is not someone who has solved the most problems. An expert is someone who, upon reading the first line of a problem, already knows where it's going."*

---

# 🗺️ BEFORE YOU BEGIN: HOW TO USE THIS GUIDE

This is not a problem collection. This is a **pattern recognition training system**.

The difference between a candidate who clears FAANG interviews and one who doesn't is rarely intelligence — it's **pattern fluency**. The ability to read a problem and *feel* the solution structure before writing a single line of code.

This guide is split into **3 parts, 30 patterns**. Each pattern is taught as a complete mental model:
- **What triggers it** (the fingerprint)
- **Why it works** (the intuition)
- **How to verify it** (the checklist)
- **How to code it** (the template)
- **What breaks it** (the traps)
- **Where to practice** (curated problems, easy → hard)

**The ritual for every pattern:**
1. Read the fingerprint and checklist until you can recite them
2. Study the template once — understand *why* each line exists
3. Solve the Easy problems without looking at the template
4. Solve the Mediums by identifying the pattern *before* opening the editor
5. Log the clue that tipped you off for every problem you solve

---

# 🔮 THE MASTER PATTERN PREDICTOR

> Read this section multiple times. This is the heart of the entire guide.

## The 30-Second Problem Scan Technique

When you open any LeetCode problem, your eyes should land in this exact order:

```
Step 1 → Read the RETURN TYPE and OUTPUT FORMAT   (5 seconds)
         Is it: a number? a boolean? an array? all combinations?

Step 2 → Read the CONSTRAINTS                      (5 seconds)
         n ≤ 20? Backtracking allowed
         n ≤ 10^3? O(n²) fine
         n ≤ 10^5? Must be O(n log n) or better
         n ≤ 10^6? Must be O(n) or O(n log n)

Step 3 → Scan for KEYWORDS in the problem          (10 seconds)
         (see keyword table below)

Step 4 → Note the DATA STRUCTURE given             (5 seconds)
         Array? Linked list? Tree? Graph? String?

Step 5 → Ask THE FIVE UNIVERSAL QUESTIONS          (5 seconds)
```

## The 5 Universal Questions

Before writing **any** code, answer these:

| # | Question | What "Yes" Suggests |
|---|----------|---------------------|
| 1 | Is the input **sorted** or can sorting help? | Two Pointers, Binary Search, Greedy |
| 2 | Are we looking for a **contiguous** subarray/substring? | Sliding Window, Prefix Sum, Monotonic Stack |
| 3 | Do we need to explore **all possibilities**? | Backtracking, DFS |
| 4 | Is the problem asking for **optimal value** (min/max/count)? | DP, Greedy, Heap |
| 5 | Is there **repeated work** if done naively? | DP (memoization), Prefix Sum, caching |

## The Pattern Decision Flowchart

```
START: Read the problem
         │
         ▼
Is input a LINKED LIST?
   ├── YES → Fast/Slow Pointers (cycles, middle)
   │         OR Linked List Manipulation (reverse, merge)
   └── NO ──────────────────────────────────────────────┐
                                                         │
Is input a TREE?                                         │
   ├── YES → Is it level-by-level? → BFS                 │
   │         Is it path/depth/value? → Tree DFS          │
   └── NO ──────────────────────────────────────────────┐│
                                                         ││
Is input a GRAPH? (adjacency list, edges, grid)          ││
   ├── YES → Shortest path (unweighted)? → BFS           ││
   │         Shortest path (weighted)? → Dijkstra        ││
   │         All paths / connectivity? → DFS             ││
   │         Ordering / dependencies? → Topological Sort ││
   │         Dynamic groups? → Union-Find                ││
   └── NO ──────────────────────────────────────────────┐││
                                                         │││
Is it a STRING with PREFIX operations?                   │││
   ├── YES → Trie                                        │││
   └── NO ──────────────────────────────────────────────┐│││
                                                         ││││
Is it an ARRAY/STRING problem?                           ││││
   ├── Is it CONTIGUOUS subarray/substring?              ││││
   │      └── Sliding Window / Prefix Sum                ││││
   ├── Is it PAIRS in SORTED array?                      ││││
   │      └── Two Pointers                               ││││
   ├── Is it O(log n) on sorted data?                    ││││
   │      └── Binary Search                              ││││
   ├── Is it INTERVALS?                                  ││││
   │      └── Interval Merge / Sweep Line                ││││
   ├── Is it TOP K / Kth element?                        ││││
   │      └── Heap                                       ││││
   └── Is it NEXT GREATER / histogram?                   ││││
          └── Monotonic Stack                            ││││
                                                         ││││
Does it ask for COUNT/MAX/MIN with CHOICES?  ◄───────────┘│││
   ├── YES → Is there OVERLAPPING SUBPROBLEMS?            │││
   │            ├── YES → Dynamic Programming             │││
   │            └── NO  → Greedy                          │││
   └── NO ─────────────────────────────────────────────────┘││
                                                             ││
Does it ask for ALL COMBINATIONS/PERMUTATIONS?  ◄────────────┘│
   └── YES → Backtracking                                     │
                                                              │
Is it a MATH / BIT problem?  ◄────────────────────────────────┘
   └── YES → Bit Manipulation / Math patterns
```

## The Mega Keyword → Pattern Table

| Keyword / Phrase in Problem | Primary Pattern | Secondary |
|-----------------------------|-----------------|-----------|
| "subarray sum equals K" | Prefix Sum + Hash Map | Sliding Window |
| "contiguous subarray" | Sliding Window | Prefix Sum |
| "longest substring without" | Sliding Window | Two Pointers |
| "sorted array + find pair" | Two Pointers | Binary Search |
| "two sum" | Hash Map | Two Pointers |
| "find duplicate / cycle" | Fast & Slow Pointers | Union-Find |
| "middle of linked list" | Fast & Slow Pointers | — |
| "search in sorted" | Binary Search | — |
| "find minimum X that satisfies" | Binary Search on Answer | — |
| "rotated sorted array" | Binary Search | — |
| "shortest path" (unweighted) | BFS | — |
| "minimum steps / moves" | BFS | — |
| "level order / level by level" | BFS | — |
| "nearest / closest cell" | BFS | — |
| "all paths from source" | DFS / Backtracking | — |
| "number of islands / components" | DFS / BFS / Union-Find | — |
| "path sum in tree" | Tree DFS | — |
| "lowest common ancestor" | Tree DFS | — |
| "generate all combinations" | Backtracking | — |
| "all permutations / subsets" | Backtracking | — |
| "N-Queens / Sudoku" | Backtracking | — |
| "how many ways" | DP | Backtracking |
| "maximum profit / minimum cost" | DP | Greedy |
| "longest common subsequence" | DP (2D) | — |
| "coin change / unbounded" | DP (Unbounded Knapsack) | — |
| "partition equal subset" | DP (0/1 Knapsack) | — |
| "next greater element" | Monotonic Stack | — |
| "largest rectangle / histogram" | Monotonic Stack | — |
| "sliding window maximum" | Monotonic Deque | — |
| "top K / K largest / K most frequent" | Heap | QuickSelect |
| "merge K sorted" | Heap | — |
| "running median" | Two Heaps | — |
| "meeting rooms / schedule" | Intervals / Heap | — |
| "merge intervals" | Intervals | — |
| "course schedule / prerequisites" | Topological Sort | DFS |
| "shortest path weighted" | Dijkstra | Bellman-Ford |
| "negative weights" | Bellman-Ford | — |
| "connected / same group" | Union-Find | DFS |
| "prefix / starts with / autocomplete" | Trie | Hash Map |
| "range sum query" | Prefix Sum / Segment Tree | — |
| "range update / point query" | Fenwick Tree (BIT) | Segment Tree |
| "reverse linked list" | Linked List Manipulation | — |
| "XOR / single number" | Bit Manipulation | — |
| "power of two / count bits" | Bit Manipulation | — |
| "find peak / majority element" | Divide & Conquer | Binary Search |

---
---

# PART 1: FOUNDATION PATTERNS

---

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

# Pattern 3 — Two Pointers

**Category:** Array/String · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Array or string is **sorted** (or sorting would help)
- Find a **pair, triplet, or group** that satisfies a numeric condition
- Need to perform **in-place** operations (reverse, remove, partition)
- Compare elements from **both ends** toward the middle
- Merge two sorted arrays/lists
- Problem says: *"two sum in sorted"*, *"palindrome"*, *"remove duplicates in-place"*, *"3Sum"*, *"container with most water"*

**This pattern is NOT for:**
- Unsorted arrays where you can't reason about which pointer to move (use Hash Map)
- Subarray problems (use Sliding Window)

---

## 🧠 Intuition in Plain English

Picture two people walking toward each other on a number line. The left person represents "too small" and the right person represents "too large." Because the array is sorted, moving left rightward increases the sum, and moving right leftward decreases it. This navigation eliminates the need for nested loops — you converge on the answer in one pass. It's like a binary search but for pairs.

---

## 🔮 Pattern Prediction Checklist

- [ ] Is the array **sorted** (or should I sort it first)? → Two Pointers viable
- [ ] Am I finding a **pair/triplet** that meets a sum/difference constraint? → Two Pointers
- [ ] Is the operation **in-place** without extra space? → Two Pointers (write pointer variant)
- [ ] Am I comparing elements from **both ends**? → Two Pointers (inward converging)

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> two_sum_sorted(const vector<int>& arr, int target) {
    int left = 0, right = (int)arr.size() - 1;
    while (left < right) {
        int current = arr[left] + arr[right];
        if (current == target) return {left, right};
        if (current < target) ++left;
        else --right;
    }
    return {};
}

int remove_duplicates(vector<int>& arr) {
    if (arr.empty()) return 0;
    int write = 1;
    for (int read = 1; read < (int)arr.size(); ++read) {
        if (arr[read] != arr[write - 1]) {
            arr[write++] = arr[read];
        }
    }
    return write;
}

vector<vector<int>> three_sum(vector<int> nums) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> result;

    for (int i = 0; i + 2 < (int)nums.size(); ++i) {
        if (i > 0 && nums[i] == nums[i - 1]) continue;
        int left = i + 1, right = (int)nums.size() - 1;
        while (left < right) {
            long long total = 1LL * nums[i] + nums[left] + nums[right];
            if (total == 0) {
                result.push_back({nums[i], nums[left], nums[right]});
                while (left < right && nums[left] == nums[left + 1]) ++left;
                while (left < right && nums[right] == nums[right - 1]) --right;
                ++left;
                --right;
            } else if (total < 0) {
                ++left;
            } else {
                --right;
            }
        }
    }

    return result;
}
```

---

## 🔀 Variants

| Variant | Description | Direction |
|---------|-------------|-----------|
| **Converging** | Find pair from both ends | →←  |
| **Same Direction** | Read/write pointer for in-place | →→ |
| **3Sum / kSum** | Fix outer, two-pointer inner | Fix + →← |
| **Dutch National Flag** | Partition into 3 groups | 3 pointers |

---

## ⏱️ Complexity

| | Brute Force | Two Pointers |
|-|-------------|--------------|
| **Time (pair)** | O(n²) | O(n) |
| **Time (triplet)** | O(n³) | O(n²) |
| **Space** | O(1) | O(1) |

---

## ❌ Anti-patterns & Traps

- **Forgetting to sort:** Two pointers only work on sorted data (or data where pointer direction has a predictable effect).
- **Duplicate handling in 3Sum:** Skip over duplicate values after finding a valid triplet, or you'll output duplicate triplets.
- **`while left < right` vs `while left <= right`:** Use `<` for pair finding (stop before they cross), `<=` when single pointer can be valid.

---

## 📝 Problem Set

| # | Problem | Difficulty | Key Insight |
|---|---------|------------|-------------|
| 1 | [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) | 🟢 Easy | Compare chars from both ends |
| 2 | [Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) | 🟢 Easy | Classic converging pointers |
| 3 | [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) | 🟢 Easy | Write pointer pattern |
| 4 | [Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/) | 🟢 Easy | Place larger square from the back |
| 5 | [3Sum](https://leetcode.com/problems/3sum/) | 🟡 Medium | Fix one + two-pointer rest |
| 6 | [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) | 🟡 Medium | Always move the shorter wall |
| 7 | [Sort Colors (Dutch Flag)](https://leetcode.com/problems/sort-colors/) | 🟡 Medium | Three-pointer partition |
| 8 | [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) | 🔴 Hard | Track max from both sides as you go |

---
---

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

# Pattern 5 — Fast & Slow Pointers

**Category:** Linked List · **Difficulty Band:** Easy–Medium

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Linked list** with possible **cycle detection**
- Find the **middle** of a linked list
- Find the **start of a cycle** in a linked list
- Find if a sequence eventually reaches a **repeated state** (like Happy Number)
- Problem says: *"cycle"*, *"circular"*, *"middle"*, *"nth from end"*, *"meeting point"*

**This pattern is NOT for:**
- Array cycle detection (use visited set or index math)
- Shortest path (use BFS)

---

## 🧠 Intuition in Plain English

Think of a race track. A fast runner (2 steps) and a slow runner (1 step) start together. If there's a loop in the track, the fast runner will eventually lap the slow runner and they'll meet. If there's no loop, the fast runner just reaches the finish line first. This is Floyd's Tortoise and Hare algorithm — elegant, O(1) space, O(n) time.

For finding the middle: when the fast pointer reaches the end, the slow pointer is exactly at the halfway point (since fast traveled twice as far).

---

## 🔮 Pattern Prediction Checklist

- [ ] Is the input a **linked list**? → Consider Fast & Slow
- [ ] Does the problem involve a **cycle** or **repeated state**? → Fast & Slow
- [ ] Do I need to find the **middle** without knowing the length? → Fast & Slow
- [ ] Do I need O(1) **extra space**? → Fast & Slow (over storing visited nodes)

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

bool has_cycle(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return true;
    }
    return false;
}

ListNode* detect_cycle(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;

    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) break;
    }
    if (!fast || !fast->next) return nullptr;

    slow = head;
    while (slow != fast) {
        slow = slow->next;
        fast = fast->next;
    }
    return slow;
}

ListNode* find_middle(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}
```

---

## ⏱️ Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Cycle detection | O(n) | O(1) |
| Cycle start | O(n) | O(1) |
| Find middle | O(n) | O(1) |

Brute force (storing visited nodes): O(n) time but **O(n) space** — Fast & Slow gives O(1) space.

---

## ❌ Anti-patterns & Traps

- **Null check before accessing `.next.next`:** Always check `fast and fast.next`, not just `fast`.
- **Phase 2 misunderstanding:** After detecting the cycle, move `slow` back to `head` and advance **both** at speed 1.
- **Missing the Happy Number pattern:** The sequence of digit-sum computations forms a virtual linked list — use fast/slow on the *values*, not actual nodes.

---

## 📝 Problem Set

| # | Problem | Difficulty | Key Insight |
|---|---------|------------|-------------|
| 1 | [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) | 🟢 Easy | Basic fast/slow detection |
| 2 | [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/) | 🟢 Easy | Slow stops at middle when fast ends |
| 3 | [Happy Number](https://leetcode.com/problems/happy-number/) | 🟢 Easy | Digit-sum as virtual linked list |
| 4 | [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) | 🟡 Medium | Two-phase Floyd's algorithm |
| 5 | [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/) | 🟡 Medium | Array index treated as next pointer |
| 6 | [Reorder List](https://leetcode.com/problems/reorder-list/) | 🟡 Medium | Find middle + reverse second half + merge |

---
---

# Pattern 6 — Binary Search (Classic)

**Category:** Search · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Data is **sorted** (array, matrix with sorted rows, etc.)
- Need O(log n) lookup
- Find **first/last occurrence** of a value
- Find if a value **exists** in a sorted structure
- Problem says: *"search in sorted"*, *"find position"*, *"rotated sorted array"*, *"first occurrence"*

**This pattern is NOT for:**
- Unsorted data (sort first, then binary search if O(n log n) is acceptable)
- When you need the answer's exact index in the original (might need Pattern 7 instead)

---

## 🧠 Intuition in Plain English

You're looking for a name in a phone book. You don't start from page 1 — you open the middle, see if the name comes before or after, and discard half the book. Repeat. After log₂(n) flips, you've found it. Binary search is the algorithmic embodiment of this: **every comparison eliminates half the remaining search space**.

---

## 🔮 Pattern Prediction Checklist

- [ ] Is the array **sorted**? → Binary Search
- [ ] Am I looking for an **exact value** or **first/last occurrence**? → Binary Search
- [ ] Is the required time complexity **O(log n)**? → Binary Search is almost certainly the answer
- [ ] Is the array **rotated sorted**? → Modified Binary Search (one half is always sorted)

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

int binary_search_exact(const vector<int>& nums, int target) {
    int left = 0, right = (int)nums.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) return mid;
        if (nums[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}

int lower_bound_index(const vector<int>& nums, int target) {
    int left = 0, right = (int)nums.size();
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) left = mid + 1;
        else right = mid;
    }
    return left;
}

int search_rotated_sorted_array(const vector<int>& nums, int target) {
    int left = 0, right = (int)nums.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) return mid;
        if (nums[left] <= nums[mid]) {
            if (nums[left] <= target && target < nums[mid]) right = mid - 1;
            else left = mid + 1;
        } else {
            if (nums[mid] < target && target <= nums[right]) left = mid + 1;
            else right = mid - 1;
        }
    }
    return -1;
}
```

---

## ⏱️ Complexity

| | Time | Space |
|-|------|-------|
| Classic | O(log n) | O(1) |
| First/Last occurrence | O(log n) | O(1) |

---

## ❌ Anti-patterns & Traps

- **Infinite loop when `left = mid` or `right = mid`:** Always do `left = mid + 1` and `right = mid - 1` in classic search.
- **Integer overflow:** Use `mid = left + (right - left) // 2`, not `(left + right) // 2`.
- **Off-by-one on `while` condition:** `left <= right` for exact search, `left < right` for finding boundaries.

---

## 📝 Problem Set

| # | Problem | Difficulty | Key Insight |
|---|---------|------------|-------------|
| 1 | [Binary Search](https://leetcode.com/problems/binary-search/) | 🟢 Easy | Pure classic template |
| 2 | [Search Insert Position](https://leetcode.com/problems/search-insert-position/) | 🟢 Easy | Return `left` if not found |
| 3 | [First Bad Version](https://leetcode.com/problems/first-bad-version/) | 🟢 Easy | Find leftmost `true` |
| 4 | [Find First and Last Position](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) | 🟡 Medium | Two binary searches |
| 5 | [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) | 🟡 Medium | One half always sorted |
| 6 | [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) | 🟡 Medium | Compare mid to right boundary |
| 7 | [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/) | 🟡 Medium | Treat matrix as flat sorted array |
| 8 | [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) | 🔴 Hard | Binary search on partition index |

---
---

# Pattern 7 — Binary Search on Answer Space

**Category:** Search/Optimization · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Find the **minimum** value that satisfies a condition
- Find the **maximum** value that still satisfies a constraint
- The answer is a number in a **range** and you can check if a candidate answer is valid
- Problem says: *"minimum speed"*, *"maximum capacity"*, *"split into K parts"*, *"minimize the maximum"*, *"can you finish in D days"*

**THIS IS THE MOST UNDERUSED PATTERN.** Many hard problems become medium when you recognize that you can binary search on the *answer*.

---

## 🧠 Intuition in Plain English

Instead of searching for the answer in the input data, you binary search over the *answer values* themselves. The key insight: **the validity of answers is monotonic** — if speed X lets Koko eat all bananas, then any speed > X also works. This creates a sorted true/false boundary, and you binary search for the boundary.

```
Can answer = 1 work? → NO
Can answer = 2 work? → NO
Can answer = 5 work? → YES  ← Find this leftmost YES
Can answer = 6 work? → YES
```

---

## 🔮 Pattern Prediction Checklist

This pattern applies when ALL of the following are true:
- [ ] The answer is a **number** (not a path, not an array)
- [ ] You can determine **valid bounds** for the answer (min possible, max possible)
- [ ] The validity check is **monotonic**: if X works, all values > X also work (or vice versa)
- [ ] Checking if a given answer is valid takes **O(n) or less**

If all 4 hold → Binary Search on Answer, giving O(n log n) total.

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

template <typename Feasible>
int binary_search_answer(int lo, int hi, Feasible feasible) {
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (feasible(mid)) hi = mid;
        else lo = mid + 1;
    }
    return lo;
}

int minEatingSpeed(const vector<int>& piles, int h) {
    int hi = *max_element(piles.begin(), piles.end());
    auto feasible = [&](int speed) {
        long long hours = 0;
        for (int pile : piles) {
            hours += (pile + speed - 1) / speed;
        }
        return hours <= h;
    };
    return binary_search_answer(1, hi, feasible);
}
```

---

## ⏱️ Complexity

| | Time | Space |
|-|------|-------|
| Total | O(n log(max - min)) | O(1) |
| Typical | O(n log n) | O(1) |

The `is_feasible` check is O(n), called O(log(range)) times → O(n log range) total.

---

## ❌ Anti-patterns & Traps

- **Wrong search space bounds:** `left` should be the smallest possible answer (often `1` or `max(arr)`), `right` the largest (`sum(arr)`).
- **Wrong termination:** Use `left < right` (not `<=`), and return `left` at the end.
- **Non-monotonic feasibility:** Make sure `is_feasible` is truly monotonic — if X works, X+1 also works. If not, binary search won't apply.

---

## 📝 Problem Set

| # | Problem | Difficulty | Answer Range |
|---|---------|------------|--------------|
| 1 | [First Bad Version](https://leetcode.com/problems/first-bad-version/) | 🟢 Easy | 1 to n |
| 2 | [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) | 🟡 Medium | 1 to max(piles) |
| 3 | [Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) | 🟡 Medium | max(weights) to sum(weights) |
| 4 | [Minimum Number of Days to Make m Bouquets](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/) | 🟡 Medium | 1 to max(bloomDay) |
| 5 | [Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/) | 🔴 Hard | max(nums) to sum(nums) |
| 6 | [Minimize Max Distance to Gas Station](https://leetcode.com/problems/minimize-max-distance-to-gas-station/) | 🔴 Hard | Binary search on floating-point range |

---
---

# Pattern 8 — Greedy

**Category:** Optimization · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Make a **sequence of choices** to reach a global optimum
- Each choice is **locally best** and doesn't need reconsideration
- Intervals, scheduling, jumps, or selection problems
- Problem says: *"minimum number of"*, *"maximum number of"*, *"can you reach"*, *"minimum cost to cover"*, *"earliest deadline"*

**The critical distinction — Greedy vs DP:**
- Greedy: local optimal = global optimal (no "undo" needed)
- DP: need to consider future consequences, choices interact

---

## 🧠 Intuition in Plain English

Greedy is the algorithm that always orders the cheapest coffee, then the cheapest sandwich, assuming that minimizing each item independently minimizes the total. It works when the problem has **greedy choice property** — the best local decision is always part of some global optimum. When in doubt between Greedy and DP, ask: "If I make the locally best choice, will I ever need to undo it?" If yes → DP. If no → Greedy.

---

## 🔮 Pattern Prediction Checklist

- [ ] Does the problem have **scheduling**, **intervals**, or **jumps**? → Greedy likely works
- [ ] Can I sort the input and make decisions in order? → Greedy
- [ ] Making the locally optimal choice **never needs undoing**? → Greedy (not DP)
- [ ] Is the answer about **existence** (can we reach X?) not count? → Often Greedy

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

bool can_jump(const vector<int>& nums) {
    int max_reach = 0;
    for (int i = 0; i < (int)nums.size(); ++i) {
        if (i > max_reach) return false;
        max_reach = max(max_reach, i + nums[i]);
    }
    return true;
}

int max_non_overlapping(vector<pair<int, int>> intervals) {
    sort(intervals.begin(), intervals.end(), [](const auto& a, const auto& b) {
        return a.second < b.second;
    });
    int count = 0;
    int last_end = INT_MIN;
    for (const auto& interval : intervals) {
        if (interval.first >= last_end) {
            ++count;
            last_end = interval.second;
        }
    }
    return count;
}

int can_complete_circuit(const vector<int>& gas, const vector<int>& cost) {
    int total_surplus = 0;
    int current_surplus = 0;
    int start = 0;
    for (int i = 0; i < (int)gas.size(); ++i) {
        int net = gas[i] - cost[i];
        total_surplus += net;
        current_surplus += net;
        if (current_surplus < 0) {
            start = i + 1;
            current_surplus = 0;
        }
    }
    return total_surplus >= 0 ? start : -1;
}
```

---

## ⏱️ Complexity

Usually O(n log n) if sorting is needed, O(n) if input already provides a natural ordering.

---

## ❌ Anti-patterns & Traps

- **Applying greedy when DP is needed:** Classic mistake on Coin Change (greedy fails with arbitrary denominations — use DP).
- **Wrong sorting key:** For interval problems, sort by **end time** (not start time) for maximum non-overlapping.

---

## 📝 Problem Set

| # | Problem | Difficulty | Greedy Choice |
|---|---------|------------|---------------|
| 1 | [Assign Cookies](https://leetcode.com/problems/assign-cookies/) | 🟢 Easy | Match smallest satisfying cookie |
| 2 | [Jump Game](https://leetcode.com/problems/jump-game/) | 🟡 Medium | Track maximum reach |
| 3 | [Jump Game II](https://leetcode.com/problems/jump-game-ii/) | 🟡 Medium | Greedy BFS levels |
| 4 | [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) | 🟡 Medium | Always keep earliest-ending interval |
| 5 | [Gas Station](https://leetcode.com/problems/gas-station/) | 🟡 Medium | Restart after negative surplus |
| 6 | [Candy](https://leetcode.com/problems/candy/) | 🔴 Hard | Two-pass greedy (left then right) |
| 7 | [Task Scheduler](https://leetcode.com/problems/task-scheduler/) | 🟡 Medium | Schedule most-frequent first |

---
---

# Pattern 9 — Bit Manipulation

**Category:** Math · **Difficulty Band:** Easy–Medium

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Find a **single number** in an array where others appear twice/three times
- **Toggle, set, or clear** specific bits
- Check if a number is a **power of 2**
- Count the number of **1s in binary representation**
- Problem says: *"XOR"*, *"single number"*, *"bit"*, *"power of two"*, *"count bits"*, *"missing number"*

---

## 🧠 Intuition in Plain English

Bits are nature's toggle switches — a 0 or a 1. XOR is the magician of bit operations: `a XOR a = 0` (same thing cancels itself) and `a XOR 0 = a` (zero doesn't change anything). So XOR-ing a list of numbers where every number appears twice leaves only the unique one. It's the algorithmic equivalent of every person pairing up and sitting down, leaving the lonely person standing.

---

## 📐 Core Bit Operations

```cpp
#include <bits/stdc++.h>
using namespace std;

int singleNumber(const vector<int>& nums) {
    int x = 0;
    for (int num : nums) x ^= num;
    return x;
}

bool isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}

vector<int> countBits(int n) {
    vector<int> dp(n + 1, 0);
    for (int i = 1; i <= n; ++i) {
        dp[i] = dp[i >> 1] + (i & 1);
    }
    return dp;
}
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Bit Trick |
|---|---------|------------|-----------|
| 1 | [Single Number](https://leetcode.com/problems/single-number/) | 🟢 Easy | XOR all elements |
| 2 | [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/) | 🟢 Easy | `n & (n-1)` loop |
| 3 | [Power of Two](https://leetcode.com/problems/power-of-two/) | 🟢 Easy | `n & (n-1) == 0` |
| 4 | [Missing Number](https://leetcode.com/problems/missing-number/) | 🟢 Easy | XOR with indices |
| 5 | [Counting Bits](https://leetcode.com/problems/counting-bits/) | 🟢 Easy | DP with bit trick |
| 6 | [Single Number II](https://leetcode.com/problems/single-number-ii/) | 🟡 Medium | 3-state bit counter |
| 7 | [Reverse Bits](https://leetcode.com/problems/reverse-bits/) | 🟢 Easy | Shift and mask 32 times |

---
---

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
