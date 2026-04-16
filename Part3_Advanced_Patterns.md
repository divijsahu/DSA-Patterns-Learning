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

```python
import heapq

# ─── TOP K LARGEST (min-heap of size K) ───────────────────────────────────────
def top_k_largest(nums, k):
    heap = []

    for num in nums:
        heapq.heappush(heap, num)
        if len(heap) > k:
            heapq.heappop(heap)        # Remove smallest — only K largest remain

    return list(heap)                  # heap[0] is the Kth largest

# Python's heapq is a MIN-HEAP. To simulate MAX-HEAP: negate all values.
# heapq.heappush(heap, -num)
# -heapq.heappop(heap)


# ─── K MOST FREQUENT ELEMENTS ─────────────────────────────────────────────────
def top_k_frequent(nums, k):
    from collections import Counter
    freq = Counter(nums)               # {element: frequency}

    # Min-heap keyed by frequency — keeps only K highest-frequency elements
    heap = []
    for num, count in freq.items():
        heapq.heappush(heap, (count, num))
        if len(heap) > k:
            heapq.heappop(heap)

    return [num for count, num in heap]


# ─── K CLOSEST POINTS TO ORIGIN ───────────────────────────────────────────────
def k_closest(points, k):
    # Max-heap of size K (negate distance)
    heap = []
    for x, y in points:
        dist = -(x*x + y*y)           # Negate = max-heap simulation
        heapq.heappush(heap, (dist, x, y))
        if len(heap) > k:
            heapq.heappop(heap)

    return [[x, y] for (_, x, y) in heap]


# ─── FIND MEDIAN FROM DATA STREAM ────────────────────────────────────────────
class MedianFinder:
    def __init__(self):
        self.small = []                # Max-heap (lower half) — negate values
        self.large = []                # Min-heap (upper half)

    def add_num(self, num):
        # Always push to small first
        heapq.heappush(self.small, -num)

        # Balance: largest of small ≤ smallest of large
        if self.small and self.large and -self.small[0] > self.large[0]:
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)

        # Balance sizes: small can have at most one more than large
        if len(self.small) > len(self.large) + 1:
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        elif len(self.large) > len(self.small):
            val = heapq.heappop(self.large)
            heapq.heappush(self.small, -val)

    def find_median(self):
        if len(self.small) > len(self.large):
            return -self.small[0]
        return (-self.small[0] + self.large[0]) / 2.0


# ─── MERGE K SORTED LISTS ─────────────────────────────────────────────────────
def merge_k_sorted(lists):
    heap = []
    dummy = ListNode(0)
    curr = dummy

    for i, node in enumerate(lists):
        if node:
            heapq.heappush(heap, (node.val, i, node))

    while heap:
        val, i, node = heapq.heappop(heap)
        curr.next = node
        curr = curr.next
        if node.next:
            heapq.heappush(heap, (node.next.val, i, node.next))

    return dummy.next
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

```python
# ─── MERGE OVERLAPPING INTERVALS ──────────────────────────────────────────────
def merge(intervals):
    if not intervals:
        return []

    intervals.sort(key=lambda x: x[0])   # Sort by START time
    merged = [intervals[0]]

    for start, end in intervals[1:]:
        if start <= merged[-1][1]:        # Overlaps with the last merged interval
            merged[-1][1] = max(merged[-1][1], end)   # Extend the end
        else:
            merged.append([start, end])   # No overlap: commit new interval

    return merged


# ─── INSERT INTERVAL ──────────────────────────────────────────────────────────
def insert(intervals, new_interval):
    result = []
    i = 0
    n = len(intervals)

    # Add all intervals that END before new_interval starts
    while i < n and intervals[i][1] < new_interval[0]:
        result.append(intervals[i])
        i += 1

    # Merge all overlapping intervals
    while i < n and intervals[i][0] <= new_interval[1]:
        new_interval[0] = min(new_interval[0], intervals[i][0])
        new_interval[1] = max(new_interval[1], intervals[i][1])
        i += 1

    result.append(new_interval)

    # Add remaining intervals
    while i < n:
        result.append(intervals[i])
        i += 1

    return result


# ─── MINIMUM MEETING ROOMS (event sweep) ─────────────────────────────────────
def min_meeting_rooms(intervals):
    # Sweep line: +1 at start, -1 at end
    events = []
    for start, end in intervals:
        events.append((start, 1))         # Room needed
        events.append((end, -1))          # Room freed

    events.sort(key=lambda x: (x[0], x[1]))   # Sort by time; ends before starts at same time

    rooms = max_rooms = 0
    for time, delta in events:
        rooms += delta
        max_rooms = max(max_rooms, rooms)

    return max_rooms


# ─── MINIMUM ROOMS (heap approach) ───────────────────────────────────────────
def min_meeting_rooms_heap(intervals):
    if not intervals: return 0

    intervals.sort(key=lambda x: x[0])
    heap = []                             # Stores end times of active meetings

    for start, end in intervals:
        if heap and heap[0] <= start:
            heapq.heapreplace(heap, end)  # Reuse the room that ended earliest
        else:
            heapq.heappush(heap, end)     # Need a new room

    return len(heap)
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

```python
class TrieNode:
    def __init__(self):
        self.children = {}             # char → TrieNode
        self.is_end = False            # True if a complete word ends here
        # Optional: store the word itself at the node for backtracking problems
        self.word = None


class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True
        node.word = word               # Store for backtracking retrieval

    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end             # Must end a complete word

    def starts_with(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True                    # Prefix exists in the trie

    def search_with_wildcard(self, word):
        """Supports '.' as wildcard matching any single character"""
        def dfs(node, i):
            if i == len(word):
                return node.is_end
            char = word[i]
            if char == '.':
                return any(dfs(child, i + 1) for child in node.children.values())
            if char not in node.children:
                return False
            return dfs(node.children[char], i + 1)

        return dfs(self.root, 0)
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

```python
# ─── FENWICK TREE (BIT) — Point Update, Prefix Sum Query ─────────────────────
class FenwickTree:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (n + 1)     # 1-indexed

    def update(self, i, delta):
        """Add delta to index i (1-indexed)"""
        while i <= self.n:
            self.tree[i] += delta
            i += i & (-i)             # Move to next responsible index

    def query(self, i):
        """Get prefix sum from 1 to i (inclusive)"""
        total = 0
        while i > 0:
            total += self.tree[i]
            i -= i & (-i)             # Move to parent
        return total

    def range_query(self, left, right):
        """Get sum of elements from left to right (both 1-indexed)"""
        return self.query(right) - self.query(left - 1)


# ─── SEGMENT TREE — Range Update, Range Query ─────────────────────────────────
class SegmentTree:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (4 * self.n)
        self._build(nums, 0, 0, self.n - 1)

    def _build(self, nums, node, start, end):
        if start == end:
            self.tree[node] = nums[start]
        else:
            mid = (start + end) // 2
            self._build(nums, 2*node+1, start, mid)
            self._build(nums, 2*node+2, mid+1, end)
            self.tree[node] = self.tree[2*node+1] + self.tree[2*node+2]

    def update(self, idx, val, node=0, start=0, end=None):
        if end is None: end = self.n - 1
        if start == end:
            self.tree[node] = val
        else:
            mid = (start + end) // 2
            if idx <= mid:
                self.update(idx, val, 2*node+1, start, mid)
            else:
                self.update(idx, val, 2*node+2, mid+1, end)
            self.tree[node] = self.tree[2*node+1] + self.tree[2*node+2]

    def query(self, left, right, node=0, start=0, end=None):
        if end is None: end = self.n - 1
        if right < start or end < left:
            return 0                   # Out of range
        if left <= start and end <= right:
            return self.tree[node]     # Fully within range
        mid = (start + end) // 2
        return (self.query(left, right, 2*node+1, start, mid) +
                self.query(left, right, 2*node+2, mid+1, end))
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

```python
# ─── REVERSE LINKED LIST ──────────────────────────────────────────────────────
def reverse_list(head):
    prev, curr = None, head

    while curr:
        next_node = curr.next          # Save next before overwriting
        curr.next = prev               # Reverse the pointer
        prev = curr                    # Move prev forward
        curr = next_node               # Move curr forward

    return prev                        # New head is the old tail


# ─── REVERSE IN K-GROUPS ──────────────────────────────────────────────────────
def reverse_k_group(head, k):
    # Check if there are k nodes left
    count, node = 0, head
    while node and count < k:
        node = node.next
        count += 1
    if count < k:
        return head                    # Fewer than k nodes: don't reverse

    # Reverse k nodes
    prev, curr = None, head
    for _ in range(k):
        next_node = curr.next
        curr.next = prev
        prev = curr
        curr = next_node

    # head is now the tail of reversed group
    head.next = reverse_k_group(curr, k)   # Recursively handle rest
    return prev                            # Return new head of this group


# ─── MERGE TWO SORTED LISTS ───────────────────────────────────────────────────
def merge_two_sorted(l1, l2):
    dummy = ListNode(0)
    curr = dummy

    while l1 and l2:
        if l1.val <= l2.val:
            curr.next = l1
            l1 = l1.next
        else:
            curr.next = l2
            l2 = l2.next
        curr = curr.next

    curr.next = l1 or l2               # Attach remaining nodes
    return dummy.next


# ─── NTH NODE FROM END ────────────────────────────────────────────────────────
def remove_nth_from_end(head, n):
    dummy = ListNode(0)
    dummy.next = head
    fast = slow = dummy

    # Move fast n+1 steps ahead
    for _ in range(n + 1):
        fast = fast.next

    # Move both until fast reaches end
    while fast:
        fast = fast.next
        slow = slow.next

    slow.next = slow.next.next         # Remove the nth node
    return dummy.next
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

```python
# ─── ROTATE MATRIX 90° CLOCKWISE ─────────────────────────────────────────────
def rotate(matrix):
    n = len(matrix)

    # Step 1: Transpose (swap matrix[i][j] with matrix[j][i])
    for i in range(n):
        for j in range(i + 1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]

    # Step 2: Reverse each row
    for row in matrix:
        row.reverse()


# ─── SPIRAL ORDER TRAVERSAL ───────────────────────────────────────────────────
def spiral_order(matrix):
    result = []
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1

    while top <= bottom and left <= right:
        # Traverse right (top row)
        for col in range(left, right + 1):
            result.append(matrix[top][col])
        top += 1

        # Traverse down (right column)
        for row in range(top, bottom + 1):
            result.append(matrix[row][right])
        right -= 1

        # Traverse left (bottom row)
        if top <= bottom:
            for col in range(right, left - 1, -1):
                result.append(matrix[bottom][col])
            bottom -= 1

        # Traverse up (left column)
        if left <= right:
            for row in range(bottom, top - 1, -1):
                result.append(matrix[row][left])
            left += 1

    return result


# ─── SET MATRIX ZEROES (in-place) ────────────────────────────────────────────
def set_zeroes(matrix):
    rows, cols = len(matrix), len(matrix[0])
    zero_rows, zero_cols = set(), set()

    # First pass: find which rows and cols have zeros
    for r in range(rows):
        for c in range(cols):
            if matrix[r][c] == 0:
                zero_rows.add(r)
                zero_cols.add(c)

    # Second pass: set zeros
    for r in range(rows):
        for c in range(cols):
            if r in zero_rows or c in zero_cols:
                matrix[r][c] = 0


# ─── USEFUL GRID UTILITIES ───────────────────────────────────────────────────
def get_neighbors_4(r, c, rows, cols):
    """4-directional grid neighbors"""
    for dr, dc in [(0,1),(0,-1),(1,0),(-1,0)]:
        nr, nc = r + dr, c + dc
        if 0 <= nr < rows and 0 <= nc < cols:
            yield nr, nc

def get_neighbors_8(r, c, rows, cols):
    """8-directional grid neighbors (including diagonals)"""
    for dr in [-1, 0, 1]:
        for dc in [-1, 0, 1]:
            if dr == 0 and dc == 0: continue
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols:
                yield nr, nc
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
