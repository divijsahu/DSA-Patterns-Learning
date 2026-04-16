# Pattern 11 — BFS (Breadth-First Search)

**Category:** Graph/Tree · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Find the **shortest path** in an unweighted graph or grid
- Find the **minimum number of steps/moves** to reach a target
- Process nodes **level by level** (tree level order traversal)
- **Multi-source** spreading (e.g., all rotten oranges spread simultaneously)
- Problem says: *"shortest path"*, *"minimum moves"*, *"nearest"*, *"level order"*, *"minimum steps"*, *"closest"*

**BFS vs DFS decision rule:**
- Need **shortest path** → BFS (guaranteed shortest in unweighted graphs)
- Need **all paths** or **explore everything** → DFS

---

## 🧠 Intuition in Plain English

Drop a pebble in still water and watch the ripples. Each ripple ring represents one "hop" away from the center. BFS works exactly like this — it explores all nodes one step away before exploring nodes two steps away. This guarantees that the **first time you reach the target**, it's via the shortest path. A queue (FIFO) is the natural data structure because you process things in the order you discover them.

---

## 🔮 Pattern Prediction Checklist

- [ ] Am I finding the **shortest/minimum** something? → BFS (if unweighted)
- [ ] Does the problem involve a **grid** where you move in 4/8 directions? → BFS
- [ ] Does the problem ask for **level-by-level** processing? → BFS
- [ ] Are there **multiple starting points** that all spread simultaneously? → Multi-source BFS
- [ ] Are the **edges unweighted**? → BFS (if weighted, use Dijkstra)

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

int bfs(const unordered_map<int, vector<int>>& graph, int start, int target) {
    queue<int> q;
    unordered_set<int> visited;
    q.push(start);
    visited.insert(start);
    int steps = 0;

    while (!q.empty()) {
        int level_size = q.size();
        while (level_size--) {
            int node = q.front(); q.pop();
            if (node == target) return steps;
            auto it = graph.find(node);
            if (it == graph.end()) continue;
            for (int neighbor : it->second) {
                if (!visited.count(neighbor)) {
                    visited.insert(neighbor);
                    q.push(neighbor);
                }
            }
        }
        ++steps;
    }
    return -1;
}

int bfs_grid(vector<vector<int>>& grid, int start_r, int start_c, int target_r, int target_c) {
    int rows = grid.size(), cols = grid[0].size();
    queue<pair<int, int>> q;
    vector<vector<int>> visited(rows, vector<int>(cols, 0));
    q.push({start_r, start_c});
    visited[start_r][start_c] = 1;
    int steps = 0;
    vector<pair<int, int>> dirs{{0,1},{0,-1},{1,0},{-1,0}};

    while (!q.empty()) {
        int level_size = q.size();
        while (level_size--) {
            auto [r, c] = q.front(); q.pop();
            if (r == target_r && c == target_c) return steps;
            for (auto [dr, dc] : dirs) {
                int nr = r + dr, nc = c + dc;
                if (0 <= nr && nr < rows && 0 <= nc && nc < cols && !visited[nr][nc] && grid[nr][nc] != 0) {
                    visited[nr][nc] = 1;
                    q.push({nr, nc});
                }
            }
        }
        ++steps;
    }
    return -1;
}

int multi_source_bfs(vector<vector<int>>& grid) {
    int rows = grid.size(), cols = grid[0].size();
    queue<pair<int, int>> q;
    int fresh_count = 0;

    for (int r = 0; r < rows; ++r) {
        for (int c = 0; c < cols; ++c) {
            if (grid[r][c] == 2) q.push({r, c});
            else if (grid[r][c] == 1) ++fresh_count;
        }
    }

    int minutes = 0;
    vector<pair<int, int>> dirs{{0,1},{0,-1},{1,0},{-1,0}};
    while (!q.empty() && fresh_count > 0) {
        int level_size = q.size();
        while (level_size--) {
            auto [r, c] = q.front(); q.pop();
            for (auto [dr, dc] : dirs) {
                int nr = r + dr, nc = c + dc;
                if (0 <= nr && nr < rows && 0 <= nc && nc < cols && grid[nr][nc] == 1) {
                    grid[nr][nc] = 2;
                    --fresh_count;
                    q.push({nr, nc});
                }
            }
        }
        ++minutes;
    }
    return fresh_count == 0 ? minutes : -1;
}
```

---

## 🔀 Variants

| Variant | Use Case | Template Change |
|---------|----------|-----------------|
| **Tree Level Order** | Process tree level by level | Use node.left / node.right as neighbors |
| **Multi-source BFS** | All sources spread at once | Pre-load ALL sources into queue |
| **0-1 BFS** | Edge weights are 0 or 1 | Use deque: append left for 0-weight, right for 1-weight |
| **Bidirectional BFS** | Shortest path in huge graph | Search from both ends, check intersection |

---

## ⏱️ Complexity

| | Time | Space |
|-|------|-------|
| Graph BFS | O(V + E) | O(V) |
| Grid BFS | O(rows × cols) | O(rows × cols) |

---

## ❌ Anti-patterns & Traps

- **Adding to `visited` when dequeuing (instead of enqueuing):** Same node can be added to queue multiple times, causing TLE or wrong results.
- **Forgetting the `for _ in range(len(queue))` loop:** Without it, you can't count steps/levels correctly.
- **Using BFS for weighted graphs:** BFS doesn't account for edge weights — use Dijkstra instead.

---

## 📝 Problem Set

| # | Problem | Difficulty | Key Insight |
|---|---------|------------|-------------|
| 1 | [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) | 🟢 Easy | Collect nodes per level |
| 2 | [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) | 🟢 Easy | Count BFS levels |
| 3 | [Flood Fill](https://leetcode.com/problems/flood-fill/) | 🟢 Easy | BFS from source cell |
| 4 | [Number of Islands](https://leetcode.com/problems/number-of-islands/) | 🟡 Medium | Count BFS calls (= island count) |
| 5 | [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/) | 🟡 Medium | Multi-source BFS |
| 6 | [Walls and Gates](https://leetcode.com/problems/walls-and-gates/) | 🟡 Medium | Multi-source BFS from all gates |
| 7 | [Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/) | 🟡 Medium | 8-directional BFS |
| 8 | [Word Ladder](https://leetcode.com/problems/word-ladder/) | 🔴 Hard | BFS on implicit word graph |
| 9 | [Cut Off Trees for Golf Event](https://leetcode.com/problems/cut-off-trees-for-golf-event/) | 🔴 Hard | Repeated BFS for ordering |

---
---
