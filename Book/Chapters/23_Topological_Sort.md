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

```python
from collections import deque, defaultdict

# ─── KAHN'S ALGORITHM (BFS-based topological sort) ───────────────────────────
def topo_sort(n, prerequisites):
    graph = defaultdict(list)         # Adjacency list
    in_degree = [0] * n               # How many prerequisites does each node have?

    for course, prereq in prerequisites:
        graph[prereq].append(course)
        in_degree[course] += 1

    # Start with all nodes that have NO prerequisites
    queue = deque([i for i in range(n) if in_degree[i] == 0])
    order = []

    while queue:
        node = queue.popleft()
        order.append(node)

        for neighbor in graph[node]:
            in_degree[neighbor] -= 1            # Remove this prerequisite
            if in_degree[neighbor] == 0:
                queue.append(neighbor)          # Now this node is ready

    # If we couldn't process all nodes, there's a cycle
    return order if len(order) == n else []


# ─── DFS-BASED TOPOLOGICAL SORT ───────────────────────────────────────────────
def topo_sort_dfs(n, graph):
    state = [0] * n                   # 0=unvisited, 1=visiting, 2=done
    order = []
    has_cycle = [False]

    def dfs(node):
        if state[node] == 1:          # Back edge = cycle!
            has_cycle[0] = True
            return
        if state[node] == 2:
            return

        state[node] = 1               # Mark as visiting (in current path)
        for neighbor in graph[node]:
            dfs(neighbor)

        state[node] = 2               # Mark as done
        order.append(node)            # Post-order: add AFTER all descendants

    for i in range(n):
        if state[i] == 0:
            dfs(i)

    return [] if has_cycle[0] else order[::-1]   # Reverse post-order = topo order
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
