# ⚡ THE COMPLETE DSA PATTERN MASTERY GUIDE
## Part 1 of 3 — Foundation Patterns (Patterns 1–10)

> *"An expert is not someone who has solved the most problems. An expert is someone who, upon reading the first line of a problem, already knows where it's going."*

## Course Navigation

- Start here: [README](README.md)
- Next: [Part 2 - Intermediate Patterns](Part2_Intermediate_Patterns.md)
- Later: [Part 3 - Advanced Patterns](Part3_Advanced_Patterns.md)

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

```python
# ─── BUILD THE PREFIX SUM ARRAY ───────────────────────────────────────────────
def build_prefix(nums):
    n = len(nums)
    prefix = [0] * (n + 1)          # prefix[0] = 0 acts as a sentinel

    for i in range(n):
        prefix[i + 1] = prefix[i] + nums[i]

    return prefix

# ─── RANGE SUM QUERY ──────────────────────────────────────────────────────────
def range_sum(prefix, left, right):
    # Sum of nums[left..right] inclusive
    return prefix[right + 1] - prefix[left]


# ─── COUNT SUBARRAYS WITH SUM = K  (the powerful variant) ────────────────────
def subarray_sum_equals_k(nums, k):
    count = 0
    running_sum = 0
    seen = {0: 1}        # {prefix_sum: frequency} — seed with 0 so we catch
                         # subarrays starting from index 0

    for num in nums:
        running_sum += num

        # If (running_sum - k) was seen before, the subarray between
        # that earlier index and now has sum = k
        if (running_sum - k) in seen:
            count += seen[running_sum - k]

        seen[running_sum] = seen.get(running_sum, 0) + 1

    return count
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

```python
from collections import defaultdict, Counter

# ─── TWO SUM PATTERN (find complement) ───────────────────────────────────────
def two_sum(nums, target):
    seen = {}                          # {value: index}

    for i, num in enumerate(nums):
        complement = target - num

        if complement in seen:
            return [seen[complement], i]

        seen[num] = i                  # Store AFTER checking (avoid using same index)

    return []

# ─── FREQUENCY COUNT PATTERN ─────────────────────────────────────────────────
def is_anagram(s, t):
    if len(s) != len(t):
        return False

    freq = Counter(s)                  # {char: count}

    for char in t:
        freq[char] -= 1
        if freq[char] < 0:
            return False

    return True

# ─── GROUPING PATTERN ────────────────────────────────────────────────────────
def group_anagrams(strs):
    groups = defaultdict(list)         # {sorted_word: [original_words]}

    for word in strs:
        key = tuple(sorted(word))      # Canonical form of an anagram group
        groups[key].append(word)

    return list(groups.values())
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

```python
# ─── VARIANT 1: CONVERGING (find pair with target sum) ───────────────────────
def two_sum_sorted(arr, target):
    left, right = 0, len(arr) - 1

    while left < right:
        current = arr[left] + arr[right]

        if current == target:
            return [left, right]
        elif current < target:
            left += 1          # Sum too small → move left pointer right
        else:
            right -= 1         # Sum too big → move right pointer left

    return []

# ─── VARIANT 2: SAME DIRECTION (write pointer for in-place ops) ──────────────
def remove_duplicates(arr):
    if not arr:
        return 0

    write = 1                  # Points to where next unique element goes

    for read in range(1, len(arr)):
        if arr[read] != arr[write - 1]:    # New unique element found
            arr[write] = arr[read]
            write += 1

    return write               # Length of deduplicated array

# ─── VARIANT 3: 3SUM (fix one, two-pointer the rest) ─────────────────────────
def three_sum(nums):
    nums.sort()
    result = []

    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i - 1]:   # Skip duplicates for fixed element
            continue

        left, right = i + 1, len(nums) - 1

        while left < right:
            total = nums[i] + nums[left] + nums[right]

            if total == 0:
                result.append([nums[i], nums[left], nums[right]])
                while left < right and nums[left] == nums[left + 1]:
                    left += 1                   # Skip duplicates
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1
                left += 1
                right -= 1
            elif total < 0:
                left += 1
            else:
                right -= 1

    return result
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

```python
# ─── CYCLE DETECTION ──────────────────────────────────────────────────────────
def has_cycle(head):
    slow = fast = head

    while fast and fast.next:
        slow = slow.next           # Move 1 step
        fast = fast.next.next      # Move 2 steps

        if slow == fast:
            return True            # They met inside a cycle

    return False                   # Fast reached None → no cycle

# ─── FIND CYCLE START ─────────────────────────────────────────────────────────
def detect_cycle(head):
    slow = fast = head

    # Phase 1: Detect meeting point inside cycle
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            break
    else:
        return None                # No cycle

    # Phase 2: Find cycle start
    # Mathematical proof: distance(head → start) == distance(meeting → start)
    slow = head
    while slow != fast:
        slow = slow.next
        fast = fast.next

    return slow                    # Both pointers are now at cycle start

# ─── FIND MIDDLE ──────────────────────────────────────────────────────────────
def find_middle(head):
    slow = fast = head

    # When fast reaches end, slow is at middle
    # Even length: slow ends at second middle node
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

    return slow
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

```python
# ─── CLASSIC BINARY SEARCH ────────────────────────────────────────────────────
def binary_search(arr, target):
    left, right = 0, len(arr) - 1

    while left <= right:
        mid = left + (right - left) // 2     # NEVER use (left + right) // 2
                                              # → integer overflow in other languages

        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1                    # Target is in RIGHT half
        else:
            right = mid - 1                   # Target is in LEFT half

    return -1                                 # Not found

# ─── FIND LEFTMOST (FIRST) OCCURRENCE ─────────────────────────────────────────
def find_first(arr, target):
    left, right = 0, len(arr) - 1
    result = -1

    while left <= right:
        mid = left + (right - left) // 2

        if arr[mid] == target:
            result = mid           # Record, but keep searching LEFT
            right = mid - 1
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return result

# ─── ROTATED SORTED ARRAY ─────────────────────────────────────────────────────
def search_rotated(arr, target):
    left, right = 0, len(arr) - 1

    while left <= right:
        mid = left + (right - left) // 2

        if arr[mid] == target:
            return mid

        # KEY INSIGHT: One half is ALWAYS sorted in a rotated array
        if arr[left] <= arr[mid]:                    # Left half is sorted
            if arr[left] <= target < arr[mid]:
                right = mid - 1                      # Target is in left half
            else:
                left = mid + 1                       # Target is in right half
        else:                                        # Right half is sorted
            if arr[mid] < target <= arr[right]:
                left = mid + 1                       # Target is in right half
            else:
                right = mid - 1                      # Target is in left half

    return -1
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

```python
# ─── BINARY SEARCH ON ANSWER (find minimum valid) ────────────────────────────
def binary_search_answer(arr, constraint):

    # Step 1: Define the search space
    left = min_possible_answer          # E.g., 1, min(arr), or 0
    right = max_possible_answer         # E.g., sum(arr), max(arr)

    # Step 2: Binary search the boundary
    while left < right:                 # NOT <=, we want the boundary
        mid = left + (right - left) // 2

        if is_feasible(arr, mid, constraint):
            right = mid                 # Valid! Try to find something smaller
        else:
            left = mid + 1              # Invalid, need to go bigger

    return left                         # left == right == the minimum valid answer


# ─── EXAMPLE: KOKO EATING BANANAS ────────────────────────────────────────────
def min_eating_speed(piles, h):
    import math

    def can_finish(speed):
        # Can Koko finish all piles at this speed in h hours?
        hours_needed = sum(math.ceil(pile / speed) for pile in piles)
        return hours_needed <= h

    left, right = 1, max(piles)         # Speed between 1 and max pile size

    while left < right:
        mid = left + (right - left) // 2
        if can_finish(mid):
            right = mid                 # Try slower
        else:
            left = mid + 1             # Need to go faster

    return left


# ─── EXAMPLE: SPLIT ARRAY LARGEST SUM ────────────────────────────────────────
def split_array(nums, k):

    def is_feasible(max_sum):
        # Can we split nums into k parts where each part sum ≤ max_sum?
        parts = 1
        current_sum = 0
        for num in nums:
            if current_sum + num > max_sum:
                parts += 1             # Start new partition
                current_sum = num
                if parts > k:
                    return False
            else:
                current_sum += num
        return True

    left = max(nums)                    # At minimum, max single element
    right = sum(nums)                   # At maximum, entire array in one part

    while left < right:
        mid = left + (right - left) // 2
        if is_feasible(mid):
            right = mid
        else:
            left = mid + 1

    return left
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

```python
# ─── JUMP GAME (can we reach the end?) ───────────────────────────────────────
def can_jump(nums):
    max_reach = 0                       # Farthest index we can currently reach

    for i, jump in enumerate(nums):
        if i > max_reach:
            return False                # We're stuck, can't reach index i
        max_reach = max(max_reach, i + jump)   # Greedily extend reach

    return True

# ─── INTERVAL SCHEDULING (maximum non-overlapping) ───────────────────────────
def max_non_overlapping(intervals):
    # Greedy: always pick the interval that ends earliest
    intervals.sort(key=lambda x: x[1])   # Sort by END time
    count = 0
    last_end = float('-inf')

    for start, end in intervals:
        if start >= last_end:            # No overlap with last chosen
            count += 1
            last_end = end               # Greedily commit to this interval

    return count

# ─── GAS STATION (circular greedy) ───────────────────────────────────────────
def can_complete_circuit(gas, cost):
    total_surplus = 0
    current_surplus = 0
    start = 0

    for i in range(len(gas)):
        net = gas[i] - cost[i]
        total_surplus += net
        current_surplus += net

        if current_surplus < 0:          # Can't reach next station from 'start'
            start = i + 1               # Restart: try starting after this station
            current_surplus = 0

    return start if total_surplus >= 0 else -1
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

```python
# ─── ESSENTIAL BIT TRICKS ────────────────────────────────────────────────────
x & (x - 1)      # Clears the lowest set bit of x (used to count bits)
x & (-x)         # Isolates the lowest set bit of x
x ^ x            # Always 0 (cancellation)
x ^ 0            # Always x (identity)
x >> 1           # Divide by 2
x << 1           # Multiply by 2
x & 1            # Check if x is odd (last bit)
~x               # Bitwise NOT

# ─── SINGLE NUMBER (XOR cancellation) ────────────────────────────────────────
def single_number(nums):
    result = 0
    for num in nums:
        result ^= num              # All duplicates cancel, unique remains
    return result

# ─── COUNT SET BITS (Hamming Weight) ─────────────────────────────────────────
def count_bits(n):
    count = 0
    while n:
        n &= n - 1                 # Each iteration removes one set bit
        count += 1
    return count

# ─── POWER OF TWO CHECK ───────────────────────────────────────────────────────
def is_power_of_two(n):
    return n > 0 and (n & (n - 1)) == 0   # Powers of 2 have exactly one set bit

# ─── MISSING NUMBER ───────────────────────────────────────────────────────────
def missing_number(nums):
    n = len(nums)
    result = n                     # Start with n
    for i, num in enumerate(nums):
        result ^= i ^ num          # XOR with both index and value
    return result                  # Remaining = missing number
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
