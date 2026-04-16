# Pattern 21 — Heap / Priority Queue

**Category:** Data Structure · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Find the **Kth largest or Kth smallest** element
- Get the **top K** elements by frequency, distance, or value
- Merge **K sorted** arrays or lists
- Find a **running median** from a stream of numbers
- **Scheduling** problems where you always process the highest-priority task
- Problem says: *"K largest"*, *"K most frequent"*, *"K closest"*, *"running median"*, *"merge K sorted"*, *"find median from stream"*

---

## 🧠 Intuition in Plain English

A heap is a special tree that always keeps the minimum (or maximum) at the root. Think of it like a hospital emergency queue — no matter how many patients arrive, the most critical one always surfaces to the top in O(log n) time. For "top K" problems, a **min-heap of size K** acts as a filter: as new elements arrive, only the K largest survive in the heap (the smallest of them sits at the top, ready to be compared and discarded).

---

## 🔮 Pattern Prediction Checklist

- [ ] Does the problem ask for **Kth** or **top K** something? → Heap of size K
- [ ] Do I need to process elements in priority order **as they arrive**? → Heap
- [ ] Is there a **stream** of numbers and I need running stats? → Two Heaps (median)
- [ ] Are there **K sorted** things to merge efficiently? → Min-Heap on heads

---

## 📐 Core Template

```python
import heapq

# ─── TOP K LARGEST (min-heap of size K) ───────────────────────────────────────
def top_k_largest(nums, k):
    heap = []

    for num in nums:
        heapq.heappush(heap, num)
        if len(heap) > k:
            heapq.heappop(heap)        # Remove smallest — only K largest remain

    return list(heap)                  # heap[0] is the Kth largest

# Python's heapq is a MIN-HEAP. To simulate MAX-HEAP: negate all values.
# heapq.heappush(heap, -num)
# -heapq.heappop(heap)


# ─── K MOST FREQUENT ELEMENTS ─────────────────────────────────────────────────
def top_k_frequent(nums, k):
    from collections import Counter
    freq = Counter(nums)               # {element: frequency}

    # Min-heap keyed by frequency — keeps only K highest-frequency elements
    heap = []
    for num, count in freq.items():
        heapq.heappush(heap, (count, num))
        if len(heap) > k:
            heapq.heappop(heap)

    return [num for count, num in heap]


# ─── K CLOSEST POINTS TO ORIGIN ───────────────────────────────────────────────
def k_closest(points, k):
    # Max-heap of size K (negate distance)
    heap = []
    for x, y in points:
        dist = -(x*x + y*y)           # Negate = max-heap simulation
        heapq.heappush(heap, (dist, x, y))
        if len(heap) > k:
            heapq.heappop(heap)

    return [[x, y] for (_, x, y) in heap]


# ─── FIND MEDIAN FROM DATA STREAM ────────────────────────────────────────────
class MedianFinder:
    def __init__(self):
        self.small = []                # Max-heap (lower half) — negate values
        self.large = []                # Min-heap (upper half)

    def add_num(self, num):
        # Always push to small first
        heapq.heappush(self.small, -num)

        # Balance: largest of small ≤ smallest of large
        if self.small and self.large and -self.small[0] > self.large[0]:
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)

        # Balance sizes: small can have at most one more than large
        if len(self.small) > len(self.large) + 1:
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        elif len(self.large) > len(self.small):
            val = heapq.heappop(self.large)
            heapq.heappush(self.small, -val)

    def find_median(self):
        if len(self.small) > len(self.large):
            return -self.small[0]
        return (-self.small[0] + self.large[0]) / 2.0


# ─── MERGE K SORTED LISTS ─────────────────────────────────────────────────────
def merge_k_sorted(lists):
    heap = []
    dummy = ListNode(0)
    curr = dummy

    for i, node in enumerate(lists):
        if node:
            heapq.heappush(heap, (node.val, i, node))

    while heap:
        val, i, node = heapq.heappop(heap)
        curr.next = node
        curr = curr.next
        if node.next:
            heapq.heappush(heap, (node.next.val, i, node.next))

    return dummy.next
```

---

## ⏱️ Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Push/Pop | O(log k) | — |
| Top K from n elements | O(n log k) | O(k) |
| Merge K sorted (N total elements) | O(N log K) | O(K) |
| Median (per operation) | O(log n) | O(n) |

---

## ❌ Anti-patterns & Traps

- **Python is min-heap only:** Negate values to simulate max-heap. Store `(-val, other_data)`.
- **Heap vs sorted array:** Heap gives O(log n) dynamic insert/delete. If the data is static, sorting + direct access is simpler.
- **Tie-breaking in heapq:** When comparing tuples `(val, object)`, Python compares elements in order. If `val` ties, it compares the next element — ensure all tuple elements are comparable.

---

## 📝 Problem Set

| # | Problem | Difficulty | Heap Type |
|---|---------|------------|-----------|
| 1 | [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) | 🟡 Medium | Min-heap size K |
| 2 | [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) | 🟡 Medium | Max-heap size K on distance |
| 3 | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | 🟡 Medium | Min-heap on frequencies |
| 4 | [Task Scheduler](https://leetcode.com/problems/task-scheduler/) | 🟡 Medium | Max-heap + cooldown queue |
| 5 | [Reorganize String](https://leetcode.com/problems/reorganize-string/) | 🟡 Medium | Always place most frequent char |
| 6 | [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | 🔴 Hard | Min-heap on list heads |
| 7 | [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) | 🔴 Hard | Two heaps (max lower + min upper) |
| 8 | [Smallest Range Covering Elements from K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/) | 🔴 Hard | Min-heap + max tracking |

---
---
