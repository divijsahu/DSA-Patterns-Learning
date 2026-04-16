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

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))   # Initially, each node is its own parent
        self.rank = [0] * n            # Tree height (for union by rank)
        self.components = n            # Number of separate groups

    def find(self, x):
        # Path compression: make x point directly to root
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])   # Recursive compression
        return self.parent[x]

    def union(self, x, y):
        px, py = self.find(x), self.find(y)

        if px == py:
            return False               # Already in the same group (edge is redundant!)

        # Union by rank: attach smaller tree to larger
        if self.rank[px] < self.rank[py]:
            px, py = py, px            # px should be the larger tree

        self.parent[py] = px           # Attach py's tree under px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1

        self.components -= 1
        return True                    # Successfully merged two groups

    def connected(self, x, y):
        return self.find(x) == self.find(y)

    def count(self):
        return self.components


# ─── USAGE EXAMPLE: Number of Islands ────────────────────────────────────────
def num_islands(grid):
    if not grid: return 0

    rows, cols = len(grid), len(grid[0])
    uf = UnionFind(rows * cols)
    islands = 0

    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                islands += 1
                for dr, dc in [(0,1),(1,0)]:   # Only check right and down
                    nr, nc = r + dr, c + dc
                    if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == '1':
                        if uf.union(r * cols + c, nr * cols + nc):
                            islands -= 1        # Two islands became one

    return islands
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
