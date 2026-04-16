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

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<long long> dijkstra(const vector<vector<pair<int, int>>>& graph, int start, int n) {
    const long long INF = 4e18;
    vector<long long> dist(n, INF);
    dist[start] = 0;
    priority_queue<pair<long long, int>, vector<pair<long long, int>>, greater<pair<long long, int>>> heap;
    heap.push({0, start});

    while (!heap.empty()) {
        auto [d, node] = heap.top(); heap.pop();
        if (d > dist[node]) continue;
        for (auto [neighbor, weight] : graph[node]) {
            long long nd = d + weight;
            if (nd < dist[neighbor]) {
                dist[neighbor] = nd;
                heap.push({nd, neighbor});
            }
        }
    }

    return dist;
}

pair<vector<int>, long long> dijkstra_with_path(const vector<vector<pair<int, int>>>& graph, int start, int end, int n) {
    const long long INF = 4e18;
    vector<long long> dist(n, INF);
    vector<int> prev(n, -1);
    dist[start] = 0;
    priority_queue<pair<long long, int>, vector<pair<long long, int>>, greater<pair<long long, int>>> heap;
    heap.push({0, start});

    while (!heap.empty()) {
        auto [d, node] = heap.top(); heap.pop();
        if (d > dist[node]) continue;
        if (node == end) break;
        for (auto [neighbor, weight] : graph[node]) {
            long long nd = d + weight;
            if (nd < dist[neighbor]) {
                dist[neighbor] = nd;
                prev[neighbor] = node;
                heap.push({nd, neighbor});
            }
        }
    }

    vector<int> path;
    for (int cur = end; cur != -1; cur = prev[cur]) path.push_back(cur);
    reverse(path.begin(), path.end());
    return {path, dist[end]};
}
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
