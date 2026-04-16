# Pattern 24 — Dijkstra / Shortest Path (Weighted)

**Category:** Graph · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Find shortest path in a graph with **positive edge weights**
- Find minimum **cost/time** to travel from source to destination
- All-pairs shortest paths (use Floyd-Warshall)
- Problem says: *"shortest weighted path"*, *"minimum cost to reach"*, *"network delay"*, *"cheapest flights"*

**BFS vs Dijkstra:**
- BFS: unweighted graph (all edges = weight 1)
- Dijkstra: positive-weight graph
- Bellman-Ford: negative weights allowed (Pattern 25)

---

## 🧠 Intuition in Plain English

Dijkstra is BFS with a twist: instead of a queue (FIFO), use a min-heap (priority queue) that always processes the node with the shortest known distance first. Greedily "settle" nodes in order of their shortest distance. Once settled, a node's distance is final (because all remaining paths through other nodes are longer — since weights are positive).

---

## 📐 Core Template

```python
import heapq

def dijkstra(graph, start, n):
    # graph[u] = [(v, weight), ...]
    dist = [float('inf')] * n
    dist[start] = 0
    heap = [(0, start)]               # (distance, node)

    while heap:
        d, node = heapq.heappop(heap)

        if d > dist[node]:            # Stale entry (already found shorter path)
            continue

        for neighbor, weight in graph[node]:
            new_dist = dist[node] + weight

            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                heapq.heappush(heap, (new_dist, neighbor))

    return dist


# ─── DIJKSTRA WITH PATH RECONSTRUCTION ───────────────────────────────────────
def dijkstra_with_path(graph, start, end, n):
    dist = [float('inf')] * n
    prev = [-1] * n
    dist[start] = 0
    heap = [(0, start)]

    while heap:
        d, node = heapq.heappop(heap)
        if d > dist[node]: continue

        if node == end:
            break                     # Early exit when target is settled

        for neighbor, weight in graph[node]:
            new_dist = dist[node] + weight
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                prev[neighbor] = node
                heapq.heappush(heap, (new_dist, neighbor))

    # Reconstruct path
    path = []
    curr = end
    while curr != -1:
        path.append(curr)
        curr = prev[curr]
    return path[::-1], dist[end]
```

---

## ⏱️ Complexity

| Implementation | Time | Space |
|----------------|------|-------|
| With binary heap | O((V + E) log V) | O(V) |
| Dense graph | O(V²) using array | O(V²) |

---

## 📝 Problem Set

| # | Problem | Difficulty | Dijkstra Variant |
|---|---------|------------|-----------------|
| 1 | [Network Delay Time](https://leetcode.com/problems/network-delay-time/) | 🟡 Medium | Classic single-source |
| 2 | [Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/) | 🟡 Medium | Max-heap (maximize product) |
| 3 | [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/) | 🟡 Medium | Modified with stop constraint |
| 4 | [Find the City With the Smallest Number of Neighbors](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/) | 🟡 Medium | Run from each city |
| 5 | [Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/) | 🔴 Hard | Dijkstra on grid |
| 6 | [The Maze II](https://leetcode.com/problems/the-maze-ii/) | 🟡 Medium | Shortest distance with obstacles |

---
---
