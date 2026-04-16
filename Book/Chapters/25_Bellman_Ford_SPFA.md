# Pattern 25 — Bellman-Ford / SPFA

**Category:** Graph · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Shortest path with **negative edge weights**
- Detect **negative weight cycles**
- Shortest path with **at most K edges**
- Problem says: *"negative weights"*, *"arbitrage"*, *"cheapest with at most K stops"*

---

## 🧠 Intuition in Plain English

Bellman-Ford works by "relaxing" all edges repeatedly — V-1 times total (where V is the number of vertices). Why V-1? The shortest path between any two nodes uses at most V-1 edges. If a path can still be shortened after V-1 iterations, there's a negative cycle (a loop that keeps reducing the total cost forever).

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Edge {
    int u, v, w;
};

vector<long long> bellman_ford(int n, const vector<Edge>& edges, int start) {
    const long long INF = 4e18;
    vector<long long> dist(n, INF);
    dist[start] = 0;

    for (int i = 0; i < n - 1; ++i) {
        bool changed = false;
        for (const auto& edge : edges) {
            if (dist[edge.u] != INF && dist[edge.u] + edge.w < dist[edge.v]) {
                dist[edge.v] = dist[edge.u] + edge.w;
                changed = true;
            }
        }
        if (!changed) break;
    }

    return dist;
}

int find_cheapest_price(int n, const vector<vector<int>>& flights, int src, int dst, int k) {
    const int INF = 1e9;
    vector<int> prices(n, INF);
    prices[src] = 0;

    for (int i = 0; i <= k; ++i) {
        vector<int> next = prices;
        for (const auto& flight : flights) {
            int u = flight[0], v = flight[1], w = flight[2];
            if (prices[u] != INF && prices[u] + w < next[v]) {
                next[v] = prices[u] + w;
            }
        }
        prices.swap(next);
    }

    return prices[dst] == INF ? -1 : prices[dst];
}
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Key Insight |
|---|---------|------------|-------------|
| 1 | [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/) | 🟡 Medium | Exactly K+1 relaxations |
| 2 | [Find Negative Cycle](https://leetcode.com/problems/find-negative-cycle-in-a-directed-graph/) | 🟡 Medium | Extra iteration detects cycle |
| 3 | [Minimum Cost to Reach Destination in Time](https://leetcode.com/problems/minimum-cost-to-reach-destination-in-time/) | 🔴 Hard | DP on graph with time constraint |

---
---
