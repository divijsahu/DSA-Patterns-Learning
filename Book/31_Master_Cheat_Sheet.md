# 🏆 THE COMPLETE MASTER CHEAT SHEET
## All 30 Patterns at a Glance

---

### 🎯 Pattern Recognition by Return Type

| Return Type | Candidate Patterns |
|------------|-------------------|
| **Boolean** (can we / does it) | BFS, DFS, Greedy, Two Pointers, DP, Union-Find |
| **Integer** (count/minimum/maximum) | DP, Greedy, BFS (steps), Heap, Binary Search on Answer |
| **Array / List of elements** | Sliding Window, Two Pointers, Sorting |
| **All combinations / permutations** | Backtracking |
| **Shortest path** | BFS (unweighted), Dijkstra (weighted), Bellman-Ford (negative) |
| **Sorted order / ranking** | Topological Sort, Heap |
| **Data structure design** | Trie, Heap, Segment Tree, Union-Find |

---

### 📊 Pattern Complexity Reference

| # | Pattern | Time | Space | Key Operation |
|---|---------|------|-------|---------------|
| 1 | Prefix Sum | O(n) build, O(1) query | O(n) | Precompute cumulative sum |
| 2 | Hash Map | O(n) | O(n) | O(1) insert/lookup |
| 3 | Two Pointers | O(n) | O(1) | Converging or same-direction scan |
| 4 | Sliding Window | O(n) | O(k) | Expand right, shrink left |
| 5 | Fast & Slow | O(n) | O(1) | Speed-2 pointer catches speed-1 |
| 6 | Binary Search | O(log n) | O(1) | Halve search space each step |
| 7 | BS on Answer | O(n log n) | O(1) | Feasibility check × log(range) |
| 8 | Greedy | O(n log n) | O(1) | Local optimal = global optimal |
| 9 | Bit Manipulation | O(n) or O(1) | O(1) | XOR, AND, shift tricks |
| 10 | Divide & Conquer | O(n log n) | O(log n) | Split, recurse, merge |
| 11 | BFS | O(V+E) | O(V) | Level-by-level with queue |
| 12 | DFS | O(V+E) | O(V) | Stack/recursion, go deep first |
| 13 | Tree DFS | O(n) | O(h) | Post/pre/in-order recursion |
| 14 | Backtracking | O(b^d) | O(d) | Build + undo at each step |
| 15 | DP 1D | O(n) | O(n) or O(1) | Linear recurrence |
| 16 | DP Knapsack | O(n×W) | O(W) | Include/exclude per item |
| 17 | DP 2D/Strings | O(m×n) | O(m×n) | Two-sequence subproblems |
| 18 | DP Interval | O(n³) | O(n²) | Fill by interval length |
| 19 | Monotonic Stack | O(n) | O(n) | Pop when order breaks |
| 20 | Monotonic Queue | O(n) | O(k) | Maintain window max/min |
| 21 | Heap | O(n log k) | O(k) | Always pop min/max |
| 22 | Intervals | O(n log n) | O(n) | Sort + sweep |
| 23 | Topological Sort | O(V+E) | O(V) | Process 0-in-degree first |
| 24 | Dijkstra | O((V+E) log V) | O(V) | Min-heap on distances |
| 25 | Bellman-Ford | O(V×E) | O(V) | Relax all edges V-1 times |
| 26 | Union-Find | O(α(n)) per op | O(n) | Path compress + union by rank |
| 27 | Trie | O(L) per op | O(n×L) | Char-by-char tree traversal |
| 28 | Segment Tree | O(log n) per op | O(n) | Divide range, combine results |
| 29 | Linked List | O(n) | O(1) | Pointer re-wiring |
| 30 | Matrix/Grid | O(m×n) | O(m×n) | Layer, boundary, BFS/DFS |

---

### 🔑 The 50 Most Common LeetCode Keywords → Pattern

| Keyword | Pattern(s) |
|---------|-----------|
| "subarray sum equals K" | Prefix Sum + Hash Map |
| "longest substring without repeating" | Sliding Window |
| "two sum" | Hash Map |
| "three sum" | Two Pointers (sort first) |
| "shortest path" (unweighted) | BFS |
| "shortest path" (weighted, positive) | Dijkstra |
| "shortest path" (negative weights) | Bellman-Ford |
| "detect cycle" (undirected) | Union-Find |
| "detect cycle" (directed) | DFS (3-color) |
| "number of islands" | BFS/DFS/Union-Find |
| "rotting oranges" | Multi-source BFS |
| "course schedule" | Topological Sort |
| "word ladder" | BFS on word graph |
| "path sum" | Tree DFS |
| "diameter of tree" | Tree DFS (post-order) |
| "lowest common ancestor" | Tree DFS |
| "serialize/deserialize tree" | Pre-order DFS |
| "all combinations" | Backtracking |
| "N-Queens" | Backtracking (constraint) |
| "climbing stairs" | DP 1D (Fibonacci) |
| "house robber" | DP 1D (skip/take) |
| "coin change" | DP Unbounded Knapsack |
| "partition equal subset" | DP 0/1 Knapsack |
| "longest common subsequence" | DP 2D |
| "edit distance" | DP 2D |
| "burst balloons" | DP Interval |
| "next greater element" | Monotonic Stack |
| "largest rectangle" | Monotonic Stack |
| "sliding window maximum" | Monotonic Queue |
| "top K frequent" | Heap (min, size K) |
| "find median from stream" | Two Heaps |
| "merge K sorted lists" | Heap |
| "merge intervals" | Intervals (sort + extend) |
| "meeting rooms II" | Intervals + Heap/Sweep |
| "employee free time" | Intervals (merge all, find gaps) |
| "implement trie" | Trie |
| "word search II" | Trie + Backtracking |
| "range sum query mutable" | Fenwick Tree |
| "count inversions" | Fenwick Tree or Merge Sort |
| "rotate image" | Matrix (transpose + reverse) |
| "spiral matrix" | Matrix (boundary pointers) |
| "reverse linked list" | Linked List (prev/curr/next) |
| "remove nth from end" | Linked List (two pointers) |
| "middle of linked list" | Fast & Slow Pointers |
| "has cycle" (linked list) | Fast & Slow Pointers |
| "binary search in sorted" | Binary Search |
| "rotated sorted array" | Modified Binary Search |
| "koko eating bananas" | Binary Search on Answer |
| "XOR single number" | Bit Manipulation |
| "power of two" | Bit Manipulation |

---

### 🧭 The Pattern Selection Decision Flowchart (Simplified)

```
What is the INPUT?
│
├── Linked List → Fast/Slow, Linked List Manipulation
│
├── Tree → Tree DFS (almost always)
│           └── Level-by-level? → BFS
│
├── Graph → Shortest path? → BFS (unweighted) / Dijkstra (weighted)
│           Ordering?      → Topological Sort
│           Components?    → DFS / Union-Find
│
├── String → Prefix operations? → Trie
│            Substrings?         → Sliding Window
│            Two strings?        → DP 2D
│
└── Array/Numbers:
    ├── Sorted + pairs?               → Two Pointers
    ├── Contiguous subarray?          → Sliding Window / Prefix Sum
    ├── O(log n) needed?              → Binary Search
    ├── Minimize/maximize with check? → Binary Search on Answer
    ├── Generate all combos?          → Backtracking
    ├── Count ways / min cost?        → DP
    ├── Schedule / intervals?         → Intervals / Greedy
    ├── Top K?                        → Heap
    └── Next greater?                 → Monotonic Stack
```

---

### 📅 Recommended 12-Week Study Schedule

| Week | Patterns | Focus |
|------|----------|-------|
| 1 | 1 (Prefix Sum), 2 (Hash Map) | Foundation array tricks |
| 2 | 3 (Two Pointers), 4 (Sliding Window) | Linear scan mastery |
| 3 | 5 (Fast/Slow), 6 (Binary Search) | Pointer techniques |
| 4 | 7 (BS on Answer), 8 (Greedy) | Optimization thinking |
| 5 | 9 (Bit Manip), 10 (Divide & Conquer) | Math patterns |
| 6 | 11 (BFS), 12 (DFS) | Graph traversal |
| 7 | 13 (Tree DFS), 14 (Backtracking) | Recursion mastery |
| 8 | 15, 16 (DP 1D, Knapsack) | DP foundation |
| 9 | 17, 18 (DP 2D, Interval) | Advanced DP |
| 10 | 19, 20, 21 (Stacks, Queue, Heap) | Data structure patterns |
| 11 | 22–26 (Intervals, Graphs) | Advanced graph algorithms |
| 12 | 27–30 (Trie, SegTree, LL, Matrix) | Advanced data structures |
| 13+ | Mixed pattern drills | Identify pattern in 60 seconds |

---

### 🎯 The Final Test: Can You Pass The 60-Second Pattern Challenge?

For any problem you haven't seen before, you should be able to answer these in under 60 seconds:

1. **What is the output type?** (number, boolean, array, all combinations)
2. **What data structure is the input?** (array, tree, graph, string, linked list)
3. **Which 3 keywords from the problem best match the trigger table?**
4. **What is the time complexity constraint?** (from the constraints, infer O(n), O(n log n), etc.)
5. **Which pattern's template would you start writing first?**

If you can answer all 5 consistently without hesitation — **you're ready for FAANG interviews.**

---

## 🚀 What Comes After This Guide

Once you've internalized all 30 patterns:

1. **Practice mixed problems** — Open random LeetCode Mediums and Hard, identify the pattern before opening the solution
2. **Time yourself** — Can you write the correct template in 5 minutes? 3 minutes?
3. **Review failures** — When you get a pattern wrong, note which clue you missed
4. **Study hard problems** — Every hard problem is usually 2–3 patterns composed together (e.g., BFS + Binary Search on Answer, or DP + Binary Search, or Trie + Backtracking)
5. **Mock interviews** — Pattern recognition under pressure is a separate skill from calm practice

> *"You don't need to practice 1000 problems. You need to deeply understand 30 patterns and practice enough to recognize them instantly. That's the difference between grinding and mastery."*

---

*Part 3 of 3 — DSA Pattern Mastery Guide*
*Complete series: Part 1 (Patterns 1–10), Part 2 (Patterns 11–20), Part 3 (Patterns 21–30)*
