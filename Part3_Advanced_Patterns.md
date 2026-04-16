# ⚡ THE COMPLETE DSA PATTERN MASTERY GUIDE
## Part 3 of 3 — Advanced Patterns (Patterns 21–30) + Master Cheat Sheet

> *"The difference between a good engineer and a great one is not speed — it's the ability to see the shape of a problem before touching the keyboard."*

---

# PART 3: ADVANCED PATTERNS

*These patterns appear in the hardest interview problems and competitive programming. Master Parts 1 & 2 first. Then these become the edge that separates you.*

---

# Pattern 21 — Heap / Priority Queue

**Category:** Data Structure · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Find the **Kth largest or Kth smallest** element
- Get the **top K** elements by frequency, distance, or value
- Merge **K sorted** arrays or lists
- Find a **running median** from a stream of numbers
- **Scheduling** problems where you always process the highest-priority task
- Problem says: *"K largest"*, *"K most frequent"*, *"K closest"*, *"running median"*, *"merge K sorted"*, *"find median from stream"*

---

## 🧠 Intuition in Plain English

A heap is a special tree that always keeps the minimum (or maximum) at the root. Think of it like a hospital emergency queue — no matter how many patients arrive, the most critical one always surfaces to the top in O(log n) time. For "top K" problems, a **min-heap of size K** acts as a filter: as new elements arrive, only the K largest survive in the heap (the smallest of them sits at the top, ready to be compared and discarded).

---

## 🔮 Pattern Prediction Checklist

- [ ] Does the problem ask for **Kth** or **top K** something? → Heap of size K
- [ ] Do I need to process elements in priority order **as they arrive**? → Heap
- [ ] Is there a **stream** of numbers and I need running stats? → Two Heaps (median)
- [ ] Are there **K sorted** things to merge efficiently? → Min-Heap on heads

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

vector<int> top_k_largest(const vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> heap;
    for (int num : nums) {
        heap.push(num);
        if ((int)heap.size() > k) heap.pop();
    }
    vector<int> result;
    while (!heap.empty()) {
        result.push_back(heap.top());
        heap.pop();
    }
    return result;
}

vector<int> top_k_frequent(const vector<int>& nums, int k) {
    unordered_map<int, int> freq;
    for (int num : nums) freq[num]++;
    using Item = pair<int, int>;
    priority_queue<Item, vector<Item>, greater<Item>> heap;
    for (auto& [num, count] : freq) {
        heap.push({count, num});
        if ((int)heap.size() > k) heap.pop();
    }
    vector<int> result;
    while (!heap.empty()) {
        result.push_back(heap.top().second);
        heap.pop();
    }
    return result;
}

vector<vector<int>> k_closest(const vector<vector<int>>& points, int k) {
    priority_queue<tuple<int, int, int>> heap;
    for (const auto& p : points) {
        int x = p[0], y = p[1];
        int dist = -(x * x + y * y);
        heap.push({dist, x, y});
        if ((int)heap.size() > k) heap.pop();
    }
    vector<vector<int>> result;
    while (!heap.empty()) {
        auto [dist, x, y] = heap.top();
        heap.pop();
        result.push_back({x, y});
    }
    return result;
}

class MedianFinder {
public:
    priority_queue<int> small;
    priority_queue<int, vector<int>, greater<int>> large;

    void add_num(int num) {
        small.push(num);
        if (!large.empty() && small.top() > large.top()) {
            large.push(small.top());
            small.pop();
        }
        if ((int)small.size() > (int)large.size() + 1) {
            large.push(small.top());
            small.pop();
        } else if ((int)large.size() > (int)small.size()) {
            small.push(large.top());
            large.pop();
        }
    }

    double find_median() {
        if (small.size() > large.size()) return small.top();
        return (small.top() + large.top()) / 2.0;
    }
};

ListNode* merge_k_sorted(const vector<ListNode*>& lists) {
    using Item = tuple<int, int, ListNode*>;
    priority_queue<Item, vector<Item>, greater<Item>> heap;
    for (int i = 0; i < (int)lists.size(); ++i) {
        if (lists[i]) heap.push({lists[i]->val, i, lists[i]});
    }
    ListNode dummy(0);
    ListNode* curr = &dummy;
    while (!heap.empty()) {
        auto [val, i, node] = heap.top();
        heap.pop();
        curr->next = node;
        curr = curr->next;
        if (node->next) heap.push({node->next->val, i, node->next});
    }
    return dummy.next;
}
```

---

## ⏱️ Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Push/Pop | O(log k) | — |
| Top K from n elements | O(n log k) | O(k) |
| Merge K sorted (N total elements) | O(N log K) | O(K) |
| Median (per operation) | O(log n) | O(n) |

---

## ❌ Anti-patterns & Traps

- **Python is min-heap only:** Negate values to simulate max-heap. Store `(-val, other_data)`.
- **Heap vs sorted array:** Heap gives O(log n) dynamic insert/delete. If the data is static, sorting + direct access is simpler.
- **Tie-breaking in heapq:** When comparing tuples `(val, object)`, Python compares elements in order. If `val` ties, it compares the next element — ensure all tuple elements are comparable.

---

## 📝 Problem Set

| # | Problem | Difficulty | Heap Type |
|---|---------|------------|-----------|
| 1 | [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) | 🟡 Medium | Min-heap size K |
| 2 | [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) | 🟡 Medium | Max-heap size K on distance |
| 3 | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | 🟡 Medium | Min-heap on frequencies |
| 4 | [Task Scheduler](https://leetcode.com/problems/task-scheduler/) | 🟡 Medium | Max-heap + cooldown queue |
| 5 | [Reorganize String](https://leetcode.com/problems/reorganize-string/) | 🟡 Medium | Always place most frequent char |
| 6 | [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | 🔴 Hard | Min-heap on list heads |
| 7 | [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) | 🔴 Hard | Two heaps (max lower + min upper) |
| 8 | [Smallest Range Covering Elements from K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/) | 🔴 Hard | Min-heap + max tracking |

---
---

# Pattern 22 — Intervals (Merge / Sweep Line)

**Category:** Array · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Input is a list of `[start, end]` pairs
- Need to **merge** overlapping intervals
- Count **minimum resources** needed (meeting rooms, servers, employees)
- Find **gaps** or **free time** between intervals
- Problem says: *"intervals"*, *"meetings"*, *"overlapping"*, *"schedule"*, *"free time"*, *"insert interval"*

---

## 🧠 Intuition in Plain English

Sort the intervals by start time, then walk through them with a pointer to the last "committed" interval. If the next interval overlaps (its start ≤ previous end), extend the previous one. If not, commit the new interval. For "minimum rooms needed," use the elegant **event sweep**: separate all starts and ends, sort them, and simulate a counter going up on starts and down on ends.

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<vector<int>> merge_intervals(vector<vector<int>> intervals) {
    if (intervals.empty()) return {};
    sort(intervals.begin(), intervals.end(), [](const auto& a, const auto& b) {
        return a[0] < b[0];
    });
    vector<vector<int>> merged{intervals[0]};
    for (int i = 1; i < (int)intervals.size(); ++i) {
        auto& last = merged.back();
        if (intervals[i][0] <= last[1]) last[1] = max(last[1], intervals[i][1]);
        else merged.push_back(intervals[i]);
    }
    return merged;
}

vector<vector<int>> insert_interval(vector<vector<int>> intervals, vector<int> new_interval) {
    vector<vector<int>> result;
    int i = 0, n = intervals.size();
    while (i < n && intervals[i][1] < new_interval[0]) result.push_back(intervals[i++]);
    while (i < n && intervals[i][0] <= new_interval[1]) {
        new_interval[0] = min(new_interval[0], intervals[i][0]);
        new_interval[1] = max(new_interval[1], intervals[i][1]);
        ++i;
    }
    result.push_back(new_interval);
    while (i < n) result.push_back(intervals[i++]);
    return result;
}

int min_meeting_rooms(const vector<vector<int>>& intervals) {
    vector<pair<int, int>> events;
    for (const auto& interval : intervals) {
        events.push_back({interval[0], 1});
        events.push_back({interval[1], -1});
    }
    sort(events.begin(), events.end(), [](const auto& a, const auto& b) {
        if (a.first != b.first) return a.first < b.first;
        return a.second < b.second;
    });
    int rooms = 0, best = 0;
    for (const auto& [time, delta] : events) {
        rooms += delta;
        best = max(best, rooms);
    }
    return best;
}
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Key Insight |
|---|---------|------------|-------------|
| 1 | [Merge Intervals](https://leetcode.com/problems/merge-intervals/) | 🟡 Medium | Sort by start, extend or commit |
| 2 | [Insert Interval](https://leetcode.com/problems/insert-interval/) | 🟡 Medium | Three phases: before, merge, after |
| 3 | [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) | 🟡 Medium | Greedy: keep earliest-ending |
| 4 | [Meeting Rooms](https://leetcode.com/problems/meeting-rooms/) | 🟢 Easy | Any overlap → false |
| 5 | [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) | 🟡 Medium | Min heap or event sweep |
| 6 | [Employee Free Time](https://leetcode.com/problems/employee-free-time/) | 🔴 Hard | Merge all, find gaps |
| 7 | [My Calendar I](https://leetcode.com/problems/my-calendar-i/) | 🟡 Medium | Check overlap before inserting |

---
---

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

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> topo_sort(int n, const vector<pair<int, int>>& prerequisites) {
    vector<vector<int>> graph(n);
    vector<int> indegree(n, 0);
    for (auto [course, prereq] : prerequisites) {
        graph[prereq].push_back(course);
        ++indegree[course];
    }
    queue<int> q;
    for (int i = 0; i < n; ++i) {
        if (indegree[i] == 0) q.push(i);
    }
    vector<int> order;
    while (!q.empty()) {
        int node = q.front(); q.pop();
        order.push_back(node);
        for (int neighbor : graph[node]) {
            if (--indegree[neighbor] == 0) q.push(neighbor);
        }
    }
    return (int)order.size() == n ? order : vector<int>{};
}

vector<int> topo_sort_dfs(int n, const vector<vector<int>>& graph) {
    vector<int> state(n, 0);
    vector<int> order;
    bool has_cycle = false;

    function<void(int)> dfs = [&](int node) {
        if (state[node] == 1) {
            has_cycle = true;
            return;
        }
        if (state[node] == 2) return;
        state[node] = 1;
        for (int neighbor : graph[node]) dfs(neighbor);
        state[node] = 2;
        order.push_back(node);
    };

    for (int i = 0; i < n; ++i) {
        if (state[i] == 0) dfs(i);
    }
    if (has_cycle) return {};
    reverse(order.begin(), order.end());
    return order;
}
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

# Pattern 27 — Trie (Prefix Tree)

**Category:** String/Tree · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Prefix-based** operations: autocomplete, word suggestions
- Search for words with **wildcards** (`.` or `*`)
- **Dictionary** operations: insert, search, starts-with
- Finding **longest common prefix** among strings
- Efficient storage of many strings with shared prefixes
- Problem says: *"prefix"*, *"starts with"*, *"autocomplete"*, *"word search"*, *"add and search"*

---

## 🧠 Intuition in Plain English

A Trie is a tree where each path from root to a node spells a word (or prefix). The root is empty, and each edge represents one character. All words starting with "ca" share the path c → a from the root. This structure makes prefix lookup O(L) where L is the length of the prefix — no matter how many words are in the dictionary. Compare to a hash set: O(L) lookup but no support for prefix queries.

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TrieNode {
    unordered_map<char, TrieNode*> children;
    bool is_end = false;
    string word;
};

class Trie {
public:
    TrieNode* root;

    Trie() : root(new TrieNode()) {}

    void insert(const string& word) {
        TrieNode* node = root;
        for (char ch : word) {
            if (!node->children.count(ch)) node->children[ch] = new TrieNode();
            node = node->children[ch];
        }
        node->is_end = true;
        node->word = word;
    }

    bool search(const string& word) {
        TrieNode* node = root;
        for (char ch : word) {
            if (!node->children.count(ch)) return false;
            node = node->children[ch];
        }
        return node->is_end;
    }

    bool starts_with(const string& prefix) {
        TrieNode* node = root;
        for (char ch : prefix) {
            if (!node->children.count(ch)) return false;
            node = node->children[ch];
        }
        return true;
    }

    bool search_with_wildcard(const string& word) {
        function<bool(TrieNode*, int)> dfs = [&](TrieNode* node, int i) {
            if (i == (int)word.size()) return node->is_end;
            char ch = word[i];
            if (ch == '.') {
                for (auto& entry : node->children) {
                    if (dfs(entry.second, i + 1)) return true;
                }
                return false;
            }
            if (!node->children.count(ch)) return false;
            return dfs(node->children[ch], i + 1);
        };
        return dfs(root, 0);
    }
};
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Trie Operation |
|---|---------|------------|----------------|
| 1 | [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/) | 🟡 Medium | Build the data structure |
| 2 | [Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/) | 🟡 Medium | Trie + DFS for top 3 suggestions |
| 3 | [Design Add and Search Words](https://leetcode.com/problems/design-add-and-search-words-data-structure/) | 🟡 Medium | Wildcard '.' with DFS |
| 4 | [Replace Words](https://leetcode.com/problems/replace-words/) | 🟡 Medium | Find shortest root prefix |
| 5 | [Palindrome Pairs](https://leetcode.com/problems/palindrome-pairs/) | 🔴 Hard | Reverse words in trie |
| 6 | [Word Search II](https://leetcode.com/problems/word-search-ii/) | 🔴 Hard | Trie + backtracking on grid |
| 7 | [Maximum XOR of Two Numbers](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/) | 🟡 Medium | Binary trie (bit-by-bit) |

---
---

# Pattern 28 — Segment Tree / Fenwick Tree (BIT)

**Category:** Data Structure · **Difficulty Band:** Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Range queries** (sum, min, max) with **point updates** (values change)
- Need O(log n) for both updates and queries (Prefix Sum is O(n) for updates)
- **Range updates** with range queries
- Problem says: *"range sum query with updates"*, *"range minimum/maximum with updates"*, *"count inversions"*, *"rank of element"*

---

## 🧠 Intuition in Plain English

Prefix Sum is great for static arrays but breaks when values update (you'd have to rebuild the whole prefix array). Fenwick Tree (Binary Indexed Tree) stores partial sums in a clever bit-based structure, allowing O(log n) point updates AND O(log n) prefix queries. Segment Tree is more powerful: it supports range updates and range queries for any associative operation (sum, min, max, GCD).

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

class FenwickTree {
public:
    int n;
    vector<long long> tree;

    FenwickTree(int n) : n(n), tree(n + 1, 0) {}

    void update(int i, long long delta) {
        while (i <= n) {
            tree[i] += delta;
            i += i & -i;
        }
    }

    long long query(int i) const {
        long long total = 0;
        while (i > 0) {
            total += tree[i];
            i -= i & -i;
        }
        return total;
    }

    long long range_query(int left, int right) const {
        return query(right) - query(left - 1);
    }
};

class SegmentTree {
public:
    int n;
    vector<long long> tree;

    SegmentTree(const vector<int>& nums) {
        n = nums.size();
        tree.assign(4 * max(1, n), 0);
        if (n > 0) build(nums, 0, 0, n - 1);
    }

    void build(const vector<int>& nums, int node, int start, int end) {
        if (start == end) {
            tree[node] = nums[start];
            return;
        }
        int mid = (start + end) / 2;
        build(nums, 2 * node + 1, start, mid);
        build(nums, 2 * node + 2, mid + 1, end);
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }

    void update(int idx, int val, int node, int start, int end) {
        if (start == end) {
            tree[node] = val;
            return;
        }
        int mid = (start + end) / 2;
        if (idx <= mid) update(idx, val, 2 * node + 1, start, mid);
        else update(idx, val, 2 * node + 2, mid + 1, end);
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }

    long long query(int left, int right, int node, int start, int end) const {
        if (right < start || end < left) return 0;
        if (left <= start && end <= right) return tree[node];
        int mid = (start + end) / 2;
        return query(left, right, 2 * node + 1, start, mid) + query(left, right, 2 * node + 2, mid + 1, end);
    }
};
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Which Tree |
|---|---------|------------|------------|
| 1 | [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/) | 🟡 Medium | Fenwick or Segment Tree |
| 2 | [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) | 🔴 Hard | Fenwick (coordinate compression) |
| 3 | [Count of Range Sum](https://leetcode.com/problems/count-of-range-sum/) | 🔴 Hard | Segment Tree or Merge Sort |
| 4 | [The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/) | 🔴 Hard | Segment Tree + coordinate compression |
| 5 | [My Calendar III](https://leetcode.com/problems/my-calendar-iii/) | 🔴 Hard | Segment Tree with lazy propagation |

---
---

# Pattern 29 — Linked List Manipulation

**Category:** Linked List · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Reverse** a linked list (fully or partially)
- **Merge** two sorted linked lists
- Remove **Nth node from end**
- Find **intersection** of two linked lists
- Sort a linked list
- Problem says: *"reverse linked list"*, *"merge"*, *"nth from end"*, *"rotate"*, *"flatten"*

---

## 🧠 Intuition in Plain English

Linked list problems require careful pointer choreography. The key is to always sketch the pointer state before and after the operation. Reversing requires three pointers (prev, curr, next). Merging uses the classic two-pointer merge from merge sort. Most linked list problems are about pointer re-wiring, not data — visualize the arrows, not the values.

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

ListNode* reverse_list(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    while (curr) {
        ListNode* next_node = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next_node;
    }
    return prev;
}

ListNode* reverse_k_group(ListNode* head, int k) {
    ListNode* node = head;
    int count = 0;
    while (node && count < k) {
        node = node->next;
        ++count;
    }
    if (count < k) return head;

    ListNode* prev = nullptr;
    ListNode* curr = head;
    for (int i = 0; i < k; ++i) {
        ListNode* next_node = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next_node;
    }
    head->next = reverse_k_group(curr, k);
    return prev;
}

ListNode* merge_two_sorted(ListNode* l1, ListNode* l2) {
    ListNode dummy(0);
    ListNode* curr = &dummy;
    while (l1 && l2) {
        if (l1->val <= l2->val) {
            curr->next = l1;
            l1 = l1->next;
        } else {
            curr->next = l2;
            l2 = l2->next;
        }
        curr = curr->next;
    }
    curr->next = l1 ? l1 : l2;
    return dummy.next;
}

ListNode* remove_nth_from_end(ListNode* head, int n) {
    ListNode dummy(0);
    dummy.next = head;
    ListNode* fast = &dummy;
    ListNode* slow = &dummy;
    for (int i = 0; i < n + 1; ++i) fast = fast->next;
    while (fast) {
        fast = fast->next;
        slow = slow->next;
    }
    slow->next = slow->next->next;
    return dummy.next;
}
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Operation |
|---|---------|------------|-----------|
| 1 | [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) | 🟢 Easy | Basic reversal |
| 2 | [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) | 🟢 Easy | Two-pointer merge |
| 3 | [Remove Nth Node From End](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) | 🟡 Medium | Two pointers n apart |
| 4 | [Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/) | 🟡 Medium | Partial reversal with reconnection |
| 5 | [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/) | 🟡 Medium | Hash map or interleave clones |
| 6 | [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/) | 🟡 Medium | Digit-by-digit with carry |
| 7 | [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/) | 🔴 Hard | Reverse groups recursively |
| 8 | [Sort List](https://leetcode.com/problems/sort-list/) | 🟡 Medium | Merge sort on linked list |

---
---

# Pattern 30 — Matrix / Grid Patterns

**Category:** 2D Array · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- 2D grid traversal (islands, paths, regions)
- **Rotate, transpose, spiral** traversal
- **In-place** matrix operations
- **Diagonal** traversal, layer-by-layer operations
- Problem says: *"rotate image"*, *"spiral order"*, *"set matrix zeroes"*, *"diagonal"*, *"spiral matrix"*

---

## 🧠 Intuition in Plain English

Matrix problems require understanding the coordinate system and the relationship between indices and spatial transformations. Most in-place operations follow a "layer by layer" structure (working from outer rings inward). BFS/DFS handle connectivity. Spiral traversal uses boundary pointers that close inward. The key insight: geometric operations on matrices usually have elegant mathematical formulations.

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    for (int i = 0; i < n; ++i) {
        for (int j = i + 1; j < n; ++j) {
            swap(matrix[i][j], matrix[j][i]);
        }
    }
    for (auto& row : matrix) reverse(row.begin(), row.end());
}

vector<int> spiral_order(const vector<vector<int>>& matrix) {
    vector<int> result;
    if (matrix.empty()) return result;
    int top = 0, bottom = matrix.size() - 1;
    int left = 0, right = matrix[0].size() - 1;
    while (top <= bottom && left <= right) {
        for (int c = left; c <= right; ++c) result.push_back(matrix[top][c]);
        ++top;
        for (int r = top; r <= bottom; ++r) result.push_back(matrix[r][right]);
        --right;
        if (top <= bottom) {
            for (int c = right; c >= left; --c) result.push_back(matrix[bottom][c]);
            --bottom;
        }
        if (left <= right) {
            for (int r = bottom; r >= top; --r) result.push_back(matrix[r][left]);
            ++left;
        }
    }
    return result;
}

void set_zeroes(vector<vector<int>>& matrix) {
    int rows = matrix.size(), cols = matrix[0].size();
    vector<int> zero_rows(rows, 0), zero_cols(cols, 0);
    for (int r = 0; r < rows; ++r) {
        for (int c = 0; c < cols; ++c) {
            if (matrix[r][c] == 0) {
                zero_rows[r] = 1;
                zero_cols[c] = 1;
            }
        }
    }
    for (int r = 0; r < rows; ++r) {
        for (int c = 0; c < cols; ++c) {
            if (zero_rows[r] || zero_cols[c]) matrix[r][c] = 0;
        }
    }
}

vector<pair<int, int>> get_neighbors_4(int r, int c, int rows, int cols) {
    vector<pair<int, int>> neighbors;
    for (auto [dr, dc] : vector<pair<int, int>>{{0,1},{0,-1},{1,0},{-1,0}}) {
        int nr = r + dr, nc = c + dc;
        if (0 <= nr && nr < rows && 0 <= nc && nc < cols) neighbors.push_back({nr, nc});
    }
    return neighbors;
}

vector<pair<int, int>> get_neighbors_8(int r, int c, int rows, int cols) {
    vector<pair<int, int>> neighbors;
    for (int dr = -1; dr <= 1; ++dr) {
        for (int dc = -1; dc <= 1; ++dc) {
            if (dr == 0 && dc == 0) continue;
            int nr = r + dr, nc = c + dc;
            if (0 <= nr && nr < rows && 0 <= nc && nc < cols) neighbors.push_back({nr, nc});
        }
    }
    return neighbors;
}
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Technique |
|---|---------|------------|-----------|
| 1 | [Flood Fill](https://leetcode.com/problems/flood-fill/) | 🟢 Easy | DFS/BFS on grid |
| 2 | [Rotate Image](https://leetcode.com/problems/rotate-image/) | 🟡 Medium | Transpose + reverse rows |
| 3 | [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/) | 🟡 Medium | Four boundary pointers |
| 4 | [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/) | 🟡 Medium | Two-pass: find then set |
| 5 | [Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/) | 🟡 Medium | Start top-right, move based on comparison |
| 6 | [Game of Life](https://leetcode.com/problems/game-of-life/) | 🟡 Medium | Encode current+next state in one cell |
| 7 | [Word Search](https://leetcode.com/problems/word-search/) | 🟡 Medium | DFS + backtracking on grid |
| 8 | [Maximal Square](https://leetcode.com/problems/maximal-square/) | 🟡 Medium | DP: min of top, left, top-left |

---

---

# 🏆 THE COMPLETE MASTER CHEAT SHEET
## All 30 Patterns at a Glance

---

### 🎯 Pattern Recognition by Return Type

| Return Type | Candidate Patterns |
|------------|-------------------|
| **Boolean** (can we / does it) | BFS, DFS, Greedy, Two Pointers, DP, Union-Find |
| **Integer** (count/minimum/maximum) | DP, Greedy, BFS (steps), Heap, Binary Search on Answer |
| **Array / List of elements** | Sliding Window, Two Pointers, Sorting |
| **All combinations / permutations** | Backtracking |
| **Shortest path** | BFS (unweighted), Dijkstra (weighted), Bellman-Ford (negative) |
| **Sorted order / ranking** | Topological Sort, Heap |
| **Data structure design** | Trie, Heap, Segment Tree, Union-Find |

---

### 📊 Pattern Complexity Reference

| # | Pattern | Time | Space | Key Operation |
|---|---------|------|-------|---------------|
| 1 | Prefix Sum | O(n) build, O(1) query | O(n) | Precompute cumulative sum |
| 2 | Hash Map | O(n) | O(n) | O(1) insert/lookup |
| 3 | Two Pointers | O(n) | O(1) | Converging or same-direction scan |
| 4 | Sliding Window | O(n) | O(k) | Expand right, shrink left |
| 5 | Fast & Slow | O(n) | O(1) | Speed-2 pointer catches speed-1 |
| 6 | Binary Search | O(log n) | O(1) | Halve search space each step |
| 7 | BS on Answer | O(n log n) | O(1) | Feasibility check × log(range) |
| 8 | Greedy | O(n log n) | O(1) | Local optimal = global optimal |
| 9 | Bit Manipulation | O(n) or O(1) | O(1) | XOR, AND, shift tricks |
| 10 | Divide & Conquer | O(n log n) | O(log n) | Split, recurse, merge |
| 11 | BFS | O(V+E) | O(V) | Level-by-level with queue |
| 12 | DFS | O(V+E) | O(V) | Stack/recursion, go deep first |
| 13 | Tree DFS | O(n) | O(h) | Post/pre/in-order recursion |
| 14 | Backtracking | O(b^d) | O(d) | Build + undo at each step |
| 15 | DP 1D | O(n) | O(n) or O(1) | Linear recurrence |
| 16 | DP Knapsack | O(n×W) | O(W) | Include/exclude per item |
| 17 | DP 2D/Strings | O(m×n) | O(m×n) | Two-sequence subproblems |
| 18 | DP Interval | O(n³) | O(n²) | Fill by interval length |
| 19 | Monotonic Stack | O(n) | O(n) | Pop when order breaks |
| 20 | Monotonic Queue | O(n) | O(k) | Maintain window max/min |
| 21 | Heap | O(n log k) | O(k) | Always pop min/max |
| 22 | Intervals | O(n log n) | O(n) | Sort + sweep |
| 23 | Topological Sort | O(V+E) | O(V) | Process 0-in-degree first |
| 24 | Dijkstra | O((V+E) log V) | O(V) | Min-heap on distances |
| 25 | Bellman-Ford | O(V×E) | O(V) | Relax all edges V-1 times |
| 26 | Union-Find | O(α(n)) per op | O(n) | Path compress + union by rank |
| 27 | Trie | O(L) per op | O(n×L) | Char-by-char tree traversal |
| 28 | Segment Tree | O(log n) per op | O(n) | Divide range, combine results |
| 29 | Linked List | O(n) | O(1) | Pointer re-wiring |
| 30 | Matrix/Grid | O(m×n) | O(m×n) | Layer, boundary, BFS/DFS |

---

### 🔑 The 50 Most Common LeetCode Keywords → Pattern

| Keyword | Pattern(s) |
|---------|-----------|
| "subarray sum equals K" | Prefix Sum + Hash Map |
| "longest substring without repeating" | Sliding Window |
| "two sum" | Hash Map |
| "three sum" | Two Pointers (sort first) |
| "shortest path" (unweighted) | BFS |
| "shortest path" (weighted, positive) | Dijkstra |
| "shortest path" (negative weights) | Bellman-Ford |
| "detect cycle" (undirected) | Union-Find |
| "detect cycle" (directed) | DFS (3-color) |
| "number of islands" | BFS/DFS/Union-Find |
| "rotting oranges" | Multi-source BFS |
| "course schedule" | Topological Sort |
| "word ladder" | BFS on word graph |
| "path sum" | Tree DFS |
| "diameter of tree" | Tree DFS (post-order) |
| "lowest common ancestor" | Tree DFS |
| "serialize/deserialize tree" | Pre-order DFS |
| "all combinations" | Backtracking |
| "N-Queens" | Backtracking (constraint) |
| "climbing stairs" | DP 1D (Fibonacci) |
| "house robber" | DP 1D (skip/take) |
| "coin change" | DP Unbounded Knapsack |
| "partition equal subset" | DP 0/1 Knapsack |
| "longest common subsequence" | DP 2D |
| "edit distance" | DP 2D |
| "burst balloons" | DP Interval |
| "next greater element" | Monotonic Stack |
| "largest rectangle" | Monotonic Stack |
| "sliding window maximum" | Monotonic Queue |
| "top K frequent" | Heap (min, size K) |
| "find median from stream" | Two Heaps |
| "merge K sorted lists" | Heap |
| "merge intervals" | Intervals (sort + extend) |
| "meeting rooms II" | Intervals + Heap/Sweep |
| "employee free time" | Intervals (merge all, find gaps) |
| "implement trie" | Trie |
| "word search II" | Trie + Backtracking |
| "range sum query mutable" | Fenwick Tree |
| "count inversions" | Fenwick Tree or Merge Sort |
| "rotate image" | Matrix (transpose + reverse) |
| "spiral matrix" | Matrix (boundary pointers) |
| "reverse linked list" | Linked List (prev/curr/next) |
| "remove nth from end" | Linked List (two pointers) |
| "middle of linked list" | Fast & Slow Pointers |
| "has cycle" (linked list) | Fast & Slow Pointers |
| "binary search in sorted" | Binary Search |
| "rotated sorted array" | Modified Binary Search |
| "koko eating bananas" | Binary Search on Answer |
| "XOR single number" | Bit Manipulation |
| "power of two" | Bit Manipulation |

---

### 🧭 The Pattern Selection Decision Flowchart (Simplified)

```
What is the INPUT?
│
├── Linked List → Fast/Slow, Linked List Manipulation
│
├── Tree → Tree DFS (almost always)
│           └── Level-by-level? → BFS
│
├── Graph → Shortest path? → BFS (unweighted) / Dijkstra (weighted)
│           Ordering?      → Topological Sort
│           Components?    → DFS / Union-Find
│
├── String → Prefix operations? → Trie
│            Substrings?         → Sliding Window
│            Two strings?        → DP 2D
│
└── Array/Numbers:
    ├── Sorted + pairs?               → Two Pointers
    ├── Contiguous subarray?          → Sliding Window / Prefix Sum
    ├── O(log n) needed?              → Binary Search
    ├── Minimize/maximize with check? → Binary Search on Answer
    ├── Generate all combos?          → Backtracking
    ├── Count ways / min cost?        → DP
    ├── Schedule / intervals?         → Intervals / Greedy
    ├── Top K?                        → Heap
    └── Next greater?                 → Monotonic Stack
```

---

### 📅 Recommended 12-Week Study Schedule

| Week | Patterns | Focus |
|------|----------|-------|
| 1 | 1 (Prefix Sum), 2 (Hash Map) | Foundation array tricks |
| 2 | 3 (Two Pointers), 4 (Sliding Window) | Linear scan mastery |
| 3 | 5 (Fast/Slow), 6 (Binary Search) | Pointer techniques |
| 4 | 7 (BS on Answer), 8 (Greedy) | Optimization thinking |
| 5 | 9 (Bit Manip), 10 (Divide & Conquer) | Math patterns |
| 6 | 11 (BFS), 12 (DFS) | Graph traversal |
| 7 | 13 (Tree DFS), 14 (Backtracking) | Recursion mastery |
| 8 | 15, 16 (DP 1D, Knapsack) | DP foundation |
| 9 | 17, 18 (DP 2D, Interval) | Advanced DP |
| 10 | 19, 20, 21 (Stacks, Queue, Heap) | Data structure patterns |
| 11 | 22–26 (Intervals, Graphs) | Advanced graph algorithms |
| 12 | 27–30 (Trie, SegTree, LL, Matrix) | Advanced data structures |
| 13+ | Mixed pattern drills | Identify pattern in 60 seconds |

---

### 🎯 The Final Test: Can You Pass The 60-Second Pattern Challenge?

For any problem you haven't seen before, you should be able to answer these in under 60 seconds:

1. **What is the output type?** (number, boolean, array, all combinations)
2. **What data structure is the input?** (array, tree, graph, string, linked list)
3. **Which 3 keywords from the problem best match the trigger table?**
4. **What is the time complexity constraint?** (from the constraints, infer O(n), O(n log n), etc.)
5. **Which pattern's template would you start writing first?**

If you can answer all 5 consistently without hesitation — **you're ready for FAANG interviews.**

---

## 🚀 What Comes After This Guide

Once you've internalized all 30 patterns:

1. **Practice mixed problems** — Open random LeetCode Mediums and Hard, identify the pattern before opening the solution
2. **Time yourself** — Can you write the correct template in 5 minutes? 3 minutes?
3. **Review failures** — When you get a pattern wrong, note which clue you missed
4. **Study hard problems** — Every hard problem is usually 2–3 patterns composed together (e.g., BFS + Binary Search on Answer, or DP + Binary Search, or Trie + Backtracking)
5. **Mock interviews** — Pattern recognition under pressure is a separate skill from calm practice

> *"You don't need to practice 1000 problems. You need to deeply understand 30 patterns and practice enough to recognize them instantly. That's the difference between grinding and mastery."*

---

*Part 3 of 3 — DSA Pattern Mastery Guide*
*Complete series: Part 1 (Patterns 1–10), Part 2 (Patterns 11–20), Part 3 (Patterns 21–30)*
