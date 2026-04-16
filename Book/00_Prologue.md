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
