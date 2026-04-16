# Pattern 23 — Topological Sort

**Category:** Graph · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Task **dependencies** (do A before B)
- **Ordering** elements where some must come before others
- Detecting **cycles** in a directed graph
- Build **compilation order**, course scheduling
- Problem says: *"prerequisites"*, *"before"*, *"depends on"*, *"compile order"*, *"course schedule"*, *"alien dictionary"*

---

## 🧠 Intuition in Plain English

Topological sort gives a linear ordering of nodes in a directed acyclic graph (DAG) such that for every edge A→B, A comes before B. Imagine getting dressed: underwear must go before pants, pants before shoes. Topological sort finds a valid "getting dressed order" for any such dependency graph. If a cycle exists (chicken must come before egg, egg before chicken), there's no valid ordering — and that's a detectable cycle.

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> topo_sort(int n, const vector<pair<int, int>>& prerequisites) {
    vector<vector<int>> graph(n);
    vector<int> indegree(n, 0);
    for (auto [course, prereq] : prerequisites) {
        graph[prereq].push_back(course);
        ++indegree[course];
    }
    queue<int> q;
    for (int i = 0; i < n; ++i) {
        if (indegree[i] == 0) q.push(i);
    }
    vector<int> order;
    while (!q.empty()) {
        int node = q.front(); q.pop();
        order.push_back(node);
        for (int neighbor : graph[node]) {
            if (--indegree[neighbor] == 0) q.push(neighbor);
        }
    }
    return (int)order.size() == n ? order : vector<int>{};
}

vector<int> topo_sort_dfs(int n, const vector<vector<int>>& graph) {
    vector<int> state(n, 0);
    vector<int> order;
    bool has_cycle = false;

    function<void(int)> dfs = [&](int node) {
        if (state[node] == 1) {
            has_cycle = true;
            return;
        }
        if (state[node] == 2) return;
        state[node] = 1;
        for (int neighbor : graph[node]) dfs(neighbor);
        state[node] = 2;
        order.push_back(node);
    };

    for (int i = 0; i < n; ++i) {
        if (state[i] == 0) dfs(i);
    }
    if (has_cycle) return {};
    reverse(order.begin(), order.end());
    return order;
}
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Key Insight |
|---|---------|------------|-------------|
| 1 | [Course Schedule](https://leetcode.com/problems/course-schedule/) | 🟡 Medium | Cycle detection (can all be taken?) |
| 2 | [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/) | 🟡 Medium | Return the ordering |
| 3 | [Find Order (Course Schedule II)](https://leetcode.com/problems/course-schedule-ii/) | 🟡 Medium | Kahn's with ordering |
| 4 | [Alien Dictionary](https://leetcode.com/problems/alien-dictionary/) | 🔴 Hard | Build graph from adjacent word pairs |
| 5 | [Sequence Reconstruction](https://leetcode.com/problems/sequence-reconstruction/) | 🟡 Medium | Is unique topo order possible? |
| 6 | [Parallel Courses](https://leetcode.com/problems/parallel-courses/) | 🟡 Medium | Longest path in DAG = min semesters |

---
---
