# Pattern 12 — DFS (Depth-First Search)

**Category:** Graph · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Explore all paths** from a source
- **Detect cycles** in a directed or undirected graph
- Count or verify **connected components**
- Find if a **path exists** between two nodes
- Solve problems on **grids** (mark regions, find connected areas)
- Problem says: *"all paths"*, *"can you reach"*, *"is there a path"*, *"connected components"*, *"number of regions"*

---

## 🧠 Intuition in Plain English

DFS is the algorithm that goes all-in before giving up. Imagine navigating a maze: DFS picks a direction and walks forward until it hits a dead end, then backtracks to the last junction and tries a different route. It uses a stack (or the call stack via recursion). While BFS guarantees the shortest path, DFS guarantees it will find *all* paths if you let it run completely.

---

## 🔮 Pattern Prediction Checklist

- [ ] Do I need to find **all** paths (not just shortest)? → DFS
- [ ] Is the problem about **existence** (does path exist, can we reach)? → DFS
- [ ] Do I need to detect a **cycle**? → DFS with coloring (white/gray/black)
- [ ] Is it a **connected components** counting problem? → DFS or Union-Find

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

void dfs(const unordered_map<int, vector<int>>& graph, int node, unordered_set<int>& visited) {
    visited.insert(node);
    auto it = graph.find(node);
    if (it == graph.end()) return;
    for (int neighbor : it->second) {
        if (!visited.count(neighbor)) dfs(graph, neighbor, visited);
    }
}

void dfs_iterative(const unordered_map<int, vector<int>>& graph, int start) {
    stack<int> st;
    unordered_set<int> visited;
    st.push(start);
    visited.insert(start);

    while (!st.empty()) {
        int node = st.top(); st.pop();
        auto it = graph.find(node);
        if (it == graph.end()) continue;
        for (int neighbor : it->second) {
            if (!visited.count(neighbor)) {
                visited.insert(neighbor);
                st.push(neighbor);
            }
        }
    }
}

bool has_cycle_directed(const vector<vector<int>>& graph) {
    int n = graph.size();
    vector<int> state(n, 0);

    function<bool(int)> dfs_cycle = [&](int node) {
        state[node] = 1;
        for (int neighbor : graph[node]) {
            if (state[neighbor] == 1) return true;
            if (state[neighbor] == 0 && dfs_cycle(neighbor)) return true;
        }
        state[node] = 2;
        return false;
    };

    for (int i = 0; i < n; ++i) {
        if (state[i] == 0 && dfs_cycle(i)) return true;
    }
    return false;
}

void dfs_grid(vector<vector<int>>& grid, int r, int c) {
    int rows = grid.size(), cols = grid[0].size();
    if (r < 0 || r >= rows || c < 0 || c >= cols || grid[r][c] != 1) return;
    grid[r][c] = 0;
    dfs_grid(grid, r + 1, c);
    dfs_grid(grid, r - 1, c);
    dfs_grid(grid, r, c + 1);
    dfs_grid(grid, r, c - 1);
}
```

---

## ⏱️ Complexity

| | Time | Space |
|-|------|-------|
| Graph DFS | O(V + E) | O(V) — call stack |
| Grid DFS | O(rows × cols) | O(rows × cols) |

---

## ❌ Anti-patterns & Traps

- **Stack overflow on large inputs:** For deep recursion (n > 10^4), use iterative DFS.
- **Gray/black distinction in directed graphs:** For undirected cycle detection, just check visited. For directed graphs, you need 3 states (unvisited / in-progress / done).
- **Marking visited before vs after recursion:** Always mark visited **before** the recursive call to handle back edges.

---

## 📝 Problem Set

| # | Problem | Difficulty | Key Insight |
|---|---------|------------|-------------|
| 1 | [Number of Islands](https://leetcode.com/problems/number-of-islands/) | 🟡 Medium | Sink islands with DFS |
| 2 | [Max Area of Island](https://leetcode.com/problems/max-area-of-island/) | 🟡 Medium | DFS returns size of each island |
| 3 | [Clone Graph](https://leetcode.com/problems/clone-graph/) | 🟡 Medium | DFS + hash map for clones |
| 4 | [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/) | 🟡 Medium | Reverse DFS from both oceans |
| 5 | [Number of Provinces](https://leetcode.com/problems/number-of-provinces/) | 🟡 Medium | Count DFS calls on adjacency matrix |
| 6 | [Course Schedule (Cycle Detection)](https://leetcode.com/problems/course-schedule/) | 🟡 Medium | 3-color DFS on directed graph |
| 7 | [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/) | 🟡 Medium | DFS + path tracking |
| 8 | [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/) | 🟡 Medium | DFS from border 'O's |

---
---
