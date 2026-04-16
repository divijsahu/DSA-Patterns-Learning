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

```python
def bellman_ford(n, edges, start):
    # edges = [(u, v, weight), ...]
    dist = [float('inf')] * n
    dist[start] = 0

    # Relax all edges V-1 times
    for _ in range(n - 1):
        for u, v, w in edges:
            if dist[u] != float('inf') and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w

    # Check for negative cycles: if we can still relax, cycle exists
    for u, v, w in edges:
        if dist[u] != float('inf') and dist[u] + w < dist[v]:
            return None              # Negative cycle detected

    return dist


# ─── K STOPS VARIANT (for "Cheapest Flights Within K Stops") ─────────────────
def find_cheapest_price(n, flights, src, dst, k):
    prices = [float('inf')] * n
    prices[src] = 0

    for i in range(k + 1):           # Relax exactly k+1 times (k stops = k+1 edges)
        temp = prices[:]             # Work on a COPY to not use current-round updates

        for u, v, w in flights:
            if prices[u] != float('inf') and prices[u] + w < temp[v]:
                temp[v] = prices[u] + w

        prices = temp

    return prices[dst] if prices[dst] != float('inf') else -1
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
