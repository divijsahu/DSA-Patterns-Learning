# Pattern 26 — Union-Find (Disjoint Set Union)

**Category:** Graph · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Dynamically connecting** nodes and querying if two are connected
- Finding **number of connected components** that changes over time
- Detecting **cycles** in an undirected graph (when adding edges one by one)
- **Merging groups** or sets
- Problem says: *"same group"*, *"connected"*, *"merge"*, *"union"*, *"redundant connection"*, *"accounts merge"*

**BFS/DFS vs Union-Find:**
- Static graph → BFS/DFS is fine
- **Dynamic** connections added one at a time → Union-Find is far more efficient

---

## 🧠 Intuition in Plain English

Each node starts as its own group leader. When you `union(A, B)`, you make one group's leader point to the other's. When you `find(X)`, you follow the chain up to the root — that root is X's group ID. Two nodes are in the same group if and only if their roots are the same. **Path compression** (making every node point directly to root) and **union by rank** (always attach smaller tree to larger) make both operations nearly O(1) amortized.

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

class UnionFind {
public:
    vector<int> parent;
    vector<int> rank;
    int components;

    UnionFind(int n) : parent(n), rank(n, 0), components(n) {
        iota(parent.begin(), parent.end(), 0);
    }

    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }

    bool unite(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return false;
        if (rank[px] < rank[py]) swap(px, py);
        parent[py] = px;
        if (rank[px] == rank[py]) ++rank[px];
        --components;
        return true;
    }

    bool connected(int x, int y) {
        return find(x) == find(y);
    }
};

int num_islands(vector<vector<char>>& grid) {
    if (grid.empty()) return 0;
    int rows = grid.size(), cols = grid[0].size();
    UnionFind uf(rows * cols);
    int islands = 0;
    auto id = [&](int r, int c) { return r * cols + c; };

    for (int r = 0; r < rows; ++r) {
        for (int c = 0; c < cols; ++c) {
            if (grid[r][c] == '1') {
                ++islands;
                for (auto [dr, dc] : vector<pair<int, int>>{{0, 1}, {1, 0}}) {
                    int nr = r + dr, nc = c + dc;
                    if (0 <= nr && nr < rows && 0 <= nc && nc < cols && grid[nr][nc] == '1') {
                        if (uf.unite(id(r, c), id(nr, nc))) --islands;
                    }
                }
            }
        }
    }
    return islands;
}
```

---

## ⏱️ Complexity

With path compression + union by rank:
- `find`: O(α(n)) ≈ O(1) amortized (α is inverse Ackermann — grows slower than log*)
- `union`: O(α(n)) ≈ O(1) amortized

---

## 📝 Problem Set

| # | Problem | Difficulty | Key Insight |
|---|---------|------------|-------------|
| 1 | [Number of Provinces](https://leetcode.com/problems/number-of-provinces/) | 🟡 Medium | Count remaining components |
| 2 | [Redundant Connection](https://leetcode.com/problems/redundant-connection/) | 🟡 Medium | `union` returns false = redundant edge |
| 3 | [Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/) | 🟡 Medium | Valid tree = n-1 edges + no cycle |
| 4 | [Accounts Merge](https://leetcode.com/problems/accounts-merge/) | 🟡 Medium | Union emails that share an account |
| 5 | [Number of Islands II](https://leetcode.com/problems/number-of-islands-ii/) | 🔴 Hard | Dynamic unions as islands added |
| 6 | [Smallest String With Swaps](https://leetcode.com/problems/smallest-string-with-swaps/) | 🟡 Medium | Group swappable indices, sort each |
| 7 | [Making a Large Island](https://leetcode.com/problems/making-a-large-island/) | 🔴 Hard | Label islands, check merged sizes |

---
---
