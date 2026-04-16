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

```python
from collections import deque

# ─── STANDARD GRAPH BFS ───────────────────────────────────────────────────────
def bfs(graph, start, target):
    queue = deque([start])
    visited = {start}              # Add to visited when ENQUEUING, not dequeuing
    steps = 0

    while queue:
        # Process all nodes at the current distance level
        for _ in range(len(queue)):
            node = queue.popleft()

            if node == target:
                return steps

            for neighbor in graph[node]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)

        steps += 1                 # Completed one full level

    return -1                      # Target unreachable


# ─── GRID BFS ─────────────────────────────────────────────────────────────────
def bfs_grid(grid, start_r, start_c, target_r, target_c):
    rows, cols = len(grid), len(grid[0])
    queue = deque([(start_r, start_c)])
    visited = {(start_r, start_c)}
    steps = 0

    # 4-directional movement (add diagonals for 8-directional)
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]

    while queue:
        for _ in range(len(queue)):
            r, c = queue.popleft()

            if r == target_r and c == target_c:
                return steps

            for dr, dc in directions:
                nr, nc = r + dr, c + dc
                if (0 <= nr < rows and 0 <= nc < cols
                        and (nr, nc) not in visited
                        and grid[nr][nc] != 0):      # 0 = blocked
                    visited.add((nr, nc))
                    queue.append((nr, nc))

        steps += 1

    return -1


# ─── MULTI-SOURCE BFS ─────────────────────────────────────────────────────────
def multi_source_bfs(grid):
    # Example: rotting oranges — all sources spread simultaneously
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    fresh_count = 0

    # Initialize: add ALL sources at once
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:       # Rotten orange = source
                queue.append((r, c))
            elif grid[r][c] == 1:
                fresh_count += 1

    minutes = 0
    while queue and fresh_count > 0:
        for _ in range(len(queue)):
            r, c = queue.popleft()
            for dr, dc in [(0,1),(0,-1),(1,0),(-1,0)]:
                nr, nc = r + dr, c + dc
                if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                    grid[nr][nc] = 2
                    fresh_count -= 1
                    queue.append((nr, nc))
        minutes += 1

    return minutes if fresh_count == 0 else -1
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
