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

```cpp
#include <bits/stdc++.h>
using namespace std;

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

vector<int> top_k_largest(const vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> heap;
    for (int num : nums) {
        heap.push(num);
        if ((int)heap.size() > k) heap.pop();
    }
    vector<int> result;
    while (!heap.empty()) {
        result.push_back(heap.top());
        heap.pop();
    }
    return result;
}

vector<int> top_k_frequent(const vector<int>& nums, int k) {
    unordered_map<int, int> freq;
    for (int num : nums) freq[num]++;
    using Item = pair<int, int>;
    priority_queue<Item, vector<Item>, greater<Item>> heap;
    for (auto& [num, count] : freq) {
        heap.push({count, num});
        if ((int)heap.size() > k) heap.pop();
    }
    vector<int> result;
    while (!heap.empty()) {
        result.push_back(heap.top().second);
        heap.pop();
    }
    return result;
}

vector<vector<int>> k_closest(const vector<vector<int>>& points, int k) {
    priority_queue<tuple<int, int, int>> heap;
    for (const auto& p : points) {
        int x = p[0], y = p[1];
        int dist = -(x * x + y * y);
        heap.push({dist, x, y});
        if ((int)heap.size() > k) heap.pop();
    }
    vector<vector<int>> result;
    while (!heap.empty()) {
        auto [dist, x, y] = heap.top();
        heap.pop();
        result.push_back({x, y});
    }
    return result;
}

class MedianFinder {
public:
    priority_queue<int> small;
    priority_queue<int, vector<int>, greater<int>> large;

    void add_num(int num) {
        small.push(num);
        if (!large.empty() && small.top() > large.top()) {
            large.push(small.top());
            small.pop();
        }
        if ((int)small.size() > (int)large.size() + 1) {
            large.push(small.top());
            small.pop();
        } else if ((int)large.size() > (int)small.size()) {
            small.push(large.top());
            large.pop();
        }
    }

    double find_median() {
        if (small.size() > large.size()) return small.top();
        return (small.top() + large.top()) / 2.0;
    }
};

ListNode* merge_k_sorted(const vector<ListNode*>& lists) {
    using Item = tuple<int, int, ListNode*>;
    priority_queue<Item, vector<Item>, greater<Item>> heap;
    for (int i = 0; i < (int)lists.size(); ++i) {
        if (lists[i]) heap.push({lists[i]->val, i, lists[i]});
    }
    ListNode dummy(0);
    ListNode* curr = &dummy;
    while (!heap.empty()) {
        auto [val, i, node] = heap.top();
        heap.pop();
        curr->next = node;
        curr = curr->next;
        if (node->next) heap.push({node->next->val, i, node->next});
    }
    return dummy.next;
}
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
