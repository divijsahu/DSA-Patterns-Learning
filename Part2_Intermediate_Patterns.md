# ⚡ THE COMPLETE DSA PATTERN MASTERY GUIDE
## Part 2 of 3 — Intermediate Patterns (Patterns 11–20)

> *"Trees and graphs aren't scary. They're just recursion wearing a Halloween costume."*

## Course Navigation

- Back: [README](README.md)
- Previous: [Part 1 - Foundation Patterns](Part1_Foundation_Patterns.md)
- Next: [Part 3 - Advanced Patterns](Part3_Advanced_Patterns.md)

---

# PART 2: INTERMEDIATE PATTERNS

*Prerequisites: Complete Part 1 before starting here. Patterns in this section build on recursion intuition and graph thinking.*

---

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

# Pattern 12 — DFS (Depth-First Search)

**Category:** Graph · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Explore all paths** from a source
- **Detect cycles** in a directed or undirected graph
- Count or verify **connected components**
- Find if a **path exists** between two nodes
- Solve problems on **grids** (mark regions, find connected areas)
- Problem says: *"all paths"*, *"can you reach"*, *"is there a path"*, *"connected components"*, *"number of regions"*

---

## 🧠 Intuition in Plain English

DFS is the algorithm that goes all-in before giving up. Imagine navigating a maze: DFS picks a direction and walks forward until it hits a dead end, then backtracks to the last junction and tries a different route. It uses a stack (or the call stack via recursion). While BFS guarantees the shortest path, DFS guarantees it will find *all* paths if you let it run completely.

---

## 🔮 Pattern Prediction Checklist

- [ ] Do I need to find **all** paths (not just shortest)? → DFS
- [ ] Is the problem about **existence** (does path exist, can we reach)? → DFS
- [ ] Do I need to detect a **cycle**? → DFS with coloring (white/gray/black)
- [ ] Is it a **connected components** counting problem? → DFS or Union-Find

---

## 📐 Core Template

```python
# ─── GRAPH DFS (recursive) ────────────────────────────────────────────────────
def dfs(graph, node, visited):
    visited.add(node)
    process(node)                          # Do something with current node

    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)

# ─── GRAPH DFS (iterative) ────────────────────────────────────────────────────
def dfs_iterative(graph, start):
    stack = [start]
    visited = {start}

    while stack:
        node = stack.pop()
        process(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                stack.append(neighbor)

# ─── CYCLE DETECTION IN DIRECTED GRAPH ───────────────────────────────────────
def has_cycle(graph, n):
    # 0 = unvisited, 1 = in current DFS path (gray), 2 = fully processed (black)
    state = [0] * n

    def dfs_cycle(node):
        state[node] = 1                    # Mark as "in progress"

        for neighbor in graph[node]:
            if state[neighbor] == 1:
                return True                # Back edge = cycle!
            if state[neighbor] == 0:
                if dfs_cycle(neighbor):
                    return True

        state[node] = 2                    # Mark as "done"
        return False

    return any(dfs_cycle(i) for i in range(n) if state[i] == 0)

# ─── GRID DFS ─────────────────────────────────────────────────────────────────
def dfs_grid(grid, r, c):
    rows, cols = len(grid), len(grid[0])
    if r < 0 or r >= rows or c < 0 or c >= cols or grid[r][c] != 1:
        return                             # Out of bounds or not land

    grid[r][c] = 0                         # Mark visited (in-place, avoid set)

    dfs_grid(grid, r + 1, c)
    dfs_grid(grid, r - 1, c)
    dfs_grid(grid, r, c + 1)
    dfs_grid(grid, r, c - 1)
```

---

## ⏱️ Complexity

| | Time | Space |
|-|------|-------|
| Graph DFS | O(V + E) | O(V) — call stack |
| Grid DFS | O(rows × cols) | O(rows × cols) |

---

## ❌ Anti-patterns & Traps

- **Stack overflow on large inputs:** For deep recursion (n > 10^4), use iterative DFS.
- **Gray/black distinction in directed graphs:** For undirected cycle detection, just check visited. For directed graphs, you need 3 states (unvisited / in-progress / done).
- **Marking visited before vs after recursion:** Always mark visited **before** the recursive call to handle back edges.

---

## 📝 Problem Set

| # | Problem | Difficulty | Key Insight |
|---|---------|------------|-------------|
| 1 | [Number of Islands](https://leetcode.com/problems/number-of-islands/) | 🟡 Medium | Sink islands with DFS |
| 2 | [Max Area of Island](https://leetcode.com/problems/max-area-of-island/) | 🟡 Medium | DFS returns size of each island |
| 3 | [Clone Graph](https://leetcode.com/problems/clone-graph/) | 🟡 Medium | DFS + hash map for clones |
| 4 | [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/) | 🟡 Medium | Reverse DFS from both oceans |
| 5 | [Number of Provinces](https://leetcode.com/problems/number-of-provinces/) | 🟡 Medium | Count DFS calls on adjacency matrix |
| 6 | [Course Schedule (Cycle Detection)](https://leetcode.com/problems/course-schedule/) | 🟡 Medium | 3-color DFS on directed graph |
| 7 | [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/) | 🟡 Medium | DFS + path tracking |
| 8 | [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/) | 🟡 Medium | DFS from border 'O's |

---
---

# Pattern 13 — Tree DFS Patterns

**Category:** Tree · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Path problems** in trees (path sum, path with max value)
- **Depth / height** calculations
- **Diameter, LCA** (Lowest Common Ancestor)
- Comparing left and right subtrees
- Serializing or reconstructing trees
- Problem says: *"path sum"*, *"height"*, *"depth"*, *"diameter"*, *"symmetric"*, *"lowest common ancestor"*, *"balanced"*

---

## 🧠 Intuition in Plain English

Trees are recursion's natural habitat. Every tree problem can be reduced to: "Given what my left child tells me and what my right child tells me, what do I tell my parent?" This post-order thinking (children first, then parent) solves the vast majority of tree problems. The key discipline: **identify what each recursive call should return**, and the code writes itself.

---

## 🔮 Pattern Prediction Checklist

- [ ] Does the answer depend on **combining** left and right subtree results? → Tree DFS (post-order)
- [ ] Do we need to pass information **downward** from parent to children? → Tree DFS (pre-order)
- [ ] Is there a **global maximum** (like diameter) that needs updating at each node? → Use a nonlocal/instance variable

**The 3 Tree DFS Orders:**
- **Pre-order** (root → left → right): Serialize, build, top-down path problems
- **In-order** (left → root → right): BST problems (gives sorted sequence)
- **Post-order** (left → right → root): Height, diameter, bottom-up aggregation

---

## 📐 Core Template

```python
# ─── POST-ORDER (most common for aggregation) ─────────────────────────────────
def tree_dfs(node):
    if not node:
        return base_value              # Base case — what to return for null

    left_val = tree_dfs(node.left)    # Recurse on left
    right_val = tree_dfs(node.right)  # Recurse on right

    # Use left_val, right_val, and node.val to compute THIS node's return value
    return combine(left_val, right_val, node.val)


# ─── DIAMETER OF BINARY TREE ──────────────────────────────────────────────────
def diameter_of_binary_tree(root):
    max_diameter = [0]                 # Use list to allow mutation in nested func

    def height(node):
        if not node:
            return 0

        left_h = height(node.left)
        right_h = height(node.right)

        # The diameter passing THROUGH this node = left_height + right_height
        max_diameter[0] = max(max_diameter[0], left_h + right_h)

        return 1 + max(left_h, right_h)   # Return height to parent

    height(root)
    return max_diameter[0]


# ─── LOWEST COMMON ANCESTOR ───────────────────────────────────────────────────
def lowest_common_ancestor(root, p, q):
    if not root or root == p or root == q:
        return root                    # Found one of the targets (or null)

    left = lowest_common_ancestor(root.left, p, q)
    right = lowest_common_ancestor(root.right, p, q)

    if left and right:
        return root                    # p and q are in different subtrees
    return left or right               # Both are in the same subtree


# ─── PATH SUM (root-to-leaf) ──────────────────────────────────────────────────
def has_path_sum(root, target):
    if not root:
        return False
    if not root.left and not root.right:   # Leaf node
        return root.val == target

    remaining = target - root.val
    return has_path_sum(root.left, remaining) or has_path_sum(root.right, remaining)


# ─── BINARY TREE MAXIMUM PATH SUM ────────────────────────────────────────────
def max_path_sum(root):
    max_sum = [float('-inf')]

    def gain(node):
        if not node:
            return 0

        left_gain = max(gain(node.left), 0)     # Ignore negative paths
        right_gain = max(gain(node.right), 0)

        # Update global max: path going through this node
        max_sum[0] = max(max_sum[0], node.val + left_gain + right_gain)

        # Return the best single-branch extension to parent
        return node.val + max(left_gain, right_gain)

    gain(root)
    return max_sum[0]
```

---

## 🔀 Variants by Return Type

| What to return from DFS call | Example Problems |
|------------------------------|-----------------|
| **Height (int)** | Max depth, diameter, balanced check |
| **Boolean** | Path sum exists, symmetric tree |
| **Node** | LCA, search in BST |
| **(max, min, both) tuple** | Camera coverage, path problems |

---

## ⏱️ Complexity

| Operation | Time | Space |
|-----------|------|-------|
| All tree DFS | O(n) | O(h) — h = tree height, O(n) worst case for skewed trees |

---

## ❌ Anti-patterns & Traps

- **Forgetting the null base case:** Always handle `if not node: return <base_value>` first.
- **Mutating a global max:** In Python, use a list `[value]` or instance variable, not a plain integer (can't assign to outer scope variables directly).
- **Confusing "path through node" vs "path to parent":** In diameter/max-path-sum, the global answer might use both left and right, but what you return to the parent is only the better single side.

---

## 📝 Problem Set

| # | Problem | Difficulty | DFS Return Value |
|---|---------|------------|-----------------|
| 1 | [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) | 🟢 Easy | Height (int) |
| 2 | [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/) | 🟢 Easy | Height or -1 if unbalanced |
| 3 | [Path Sum](https://leetcode.com/problems/path-sum/) | 🟢 Easy | Boolean |
| 4 | [Symmetric Tree](https://leetcode.com/problems/symmetric-tree/) | 🟢 Easy | Recursive mirror check |
| 5 | [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/) | 🟢 Easy | Height + global max update |
| 6 | [Lowest Common Ancestor of a BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) | 🟡 Medium | Use BST ordering |
| 7 | [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/) | 🟡 Medium | Last node at each BFS level |
| 8 | [Path Sum II](https://leetcode.com/problems/path-sum-ii/) | 🟡 Medium | Collect all root-to-leaf paths |
| 9 | [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) | 🔴 Hard | Gain function + global max |
| 10 | [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) | 🔴 Hard | Pre-order DFS with null markers |

---
---

# Pattern 14 — Backtracking

**Category:** Recursion · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Generate **all** combinations, permutations, or subsets
- Solve **constraint satisfaction** problems (Sudoku, N-Queens, word search)
- Find **all valid arrangements** or configurations
- Problems where you **build a solution piece by piece** and abandon it when invalid
- Problem says: *"all combinations"*, *"all permutations"*, *"generate all"*, *"find all valid"*, *"N-Queens"*, *"Sudoku"*

**Backtracking vs DFS:** Backtracking IS a form of DFS, but specifically for problems where you build state incrementally and prune invalid states early.

---

## 🧠 Intuition in Plain English

Think of a lock combination. You try digits one by one. If at any point you know the combination is wrong (constraint violated), you stop and backtrack to the last choice, trying the next option. You only commit to a choice temporarily — writing it in pencil, not ink. The "undo" operation (erasing the pencil) is the backtrack step.

The power is in **pruning**: if you can detect invalidity early, you skip enormous branches of the search tree without exploring them.

---

## 🔮 Pattern Prediction Checklist

- [ ] Does the problem ask for **all** valid solutions (not just one or the count)? → Backtracking
- [ ] Is the solution built **piece by piece** with choices at each step? → Backtracking
- [ ] Can I **prune** invalid partial solutions early? → Backtracking is efficient here
- [ ] Is the output a **list of lists** (combinations/permutations)? → Backtracking

---

## 📐 Core Template

```python
# ─── UNIVERSAL BACKTRACKING TEMPLATE ─────────────────────────────────────────
def backtrack(state, choices, result, ...):
    # ── BASE CASE: Is the current state a complete valid solution? ────────────
    if is_complete(state):
        result.append(state[:])        # CRITICAL: append a COPY, not a reference
        return

    for choice in get_choices(choices):
        # ── PRUNING: Skip this choice if it leads to an invalid state ─────────
        if not is_valid(state, choice):
            continue

        # ── CHOOSE: Add the choice to current state ───────────────────────────
        state.append(choice)
        mark_used(choice)

        # ── EXPLORE: Recurse with this choice made ────────────────────────────
        backtrack(state, remaining_choices, result, ...)

        # ── UNCHOOSE: Remove the choice (backtrack) ───────────────────────────
        state.pop()
        unmark_used(choice)


# ─── SUBSETS ──────────────────────────────────────────────────────────────────
def subsets(nums):
    result = []

    def backtrack(start, current):
        result.append(current[:])      # Every state is a valid subset

        for i in range(start, len(nums)):
            current.append(nums[i])
            backtrack(i + 1, current)  # i+1 ensures no repeats
            current.pop()

    backtrack(0, [])
    return result


# ─── PERMUTATIONS ─────────────────────────────────────────────────────────────
def permutations(nums):
    result = []
    used = [False] * len(nums)

    def backtrack(current):
        if len(current) == len(nums):
            result.append(current[:])
            return

        for i in range(len(nums)):
            if used[i]:
                continue
            used[i] = True
            current.append(nums[i])
            backtrack(current)
            current.pop()
            used[i] = False

    backtrack([])
    return result


# ─── COMBINATION SUM (reuse allowed) ─────────────────────────────────────────
def combination_sum(candidates, target):
    result = []
    candidates.sort()                  # Sort for early pruning

    def backtrack(start, current, remaining):
        if remaining == 0:
            result.append(current[:])
            return

        for i in range(start, len(candidates)):
            if candidates[i] > remaining:
                break                  # PRUNE: rest are too large too (sorted!)
            current.append(candidates[i])
            backtrack(i, current, remaining - candidates[i])  # i not i+1: reuse ok
            current.pop()

    backtrack(0, [], target)
    return result


# ─── N-QUEENS ─────────────────────────────────────────────────────────────────
def solve_n_queens(n):
    result = []
    cols = set()
    pos_diag = set()   # r + c is constant on positive diagonals
    neg_diag = set()   # r - c is constant on negative diagonals
    board = []

    def backtrack(row):
        if row == n:
            result.append(["".join(r) for r in board])
            return

        for col in range(n):
            if col in cols or (row + col) in pos_diag or (row - col) in neg_diag:
                continue               # PRUNE: queen conflicts

            # Place queen
            cols.add(col)
            pos_diag.add(row + col)
            neg_diag.add(row - col)
            board.append(['.' if c != col else 'Q' for c in range(n)])

            backtrack(row + 1)

            # Remove queen
            cols.remove(col)
            pos_diag.remove(row + col)
            neg_diag.remove(row - col)
            board.pop()

    backtrack(0)
    return result
```

---

## 🔀 Variants

| Problem Type | Key Difference | Template Mod |
|-------------|----------------|--------------|
| **Subsets** | All states valid | Add to result at entry, not just leaf |
| **Permutations** | Order matters, each element once | Use `used[]` array |
| **Combinations** | Order doesn't matter | Use `start` index to avoid reuse |
| **Combination Sum** | Can reuse elements | Pass `i` not `i+1` |
| **Constraint Satisfaction** | Validity check at each step | Strong pruning in `is_valid` |

---

## ⏱️ Complexity

| Problem | Time | Space |
|---------|------|-------|
| Subsets | O(2ⁿ) | O(n) |
| Permutations | O(n!) | O(n) |
| Combination Sum | O(2^target) | O(target) |

Backtracking can't do better than the output size — but **pruning dramatically reduces the constant factor** in practice.

---

## ❌ Anti-patterns & Traps

- **Appending reference instead of copy:** `result.append(current)` → wrong. Use `result.append(current[:])` or `result.append(current.copy())`.
- **Forgetting to undo the choice:** Every `append` must have a matching `pop`. Every `used[i] = True` must have `used[i] = False`.
- **Missing the duplicate handling:** When input has duplicates, sort first and skip `if i > start and nums[i] == nums[i-1]`.

---

## 📝 Problem Set

| # | Problem | Difficulty | Backtrack Type |
|---|---------|------------|---------------|
| 1 | [Subsets](https://leetcode.com/problems/subsets/) | 🟡 Medium | All states valid |
| 2 | [Permutations](https://leetcode.com/problems/permutations/) | 🟡 Medium | Order matters |
| 3 | [Combinations](https://leetcode.com/problems/combinations/) | 🟡 Medium | Pick k from n |
| 4 | [Combination Sum](https://leetcode.com/problems/combination-sum/) | 🟡 Medium | Reuse allowed |
| 5 | [Word Search](https://leetcode.com/problems/word-search/) | 🟡 Medium | Grid backtracking |
| 6 | [Subsets II](https://leetcode.com/problems/subsets-ii/) | 🟡 Medium | Skip duplicates |
| 7 | [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/) | 🟡 Medium | Constraint check (is palindrome) |
| 8 | [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) | 🟡 Medium | Map digits to letters |
| 9 | [N-Queens](https://leetcode.com/problems/n-queens/) | 🔴 Hard | Set-based constraint pruning |
| 10 | [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/) | 🔴 Hard | Propagate constraints |

---
---

# Pattern 15 — Dynamic Programming: 1D

**Category:** DP · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- A sequence of decisions, each dependent on **a fixed number of previous states**
- **"How many ways"**, **"can you reach"**, **"minimum cost"** in a linear structure
- Fibonacci-like recurrences (each state depends on 1–2 previous states)
- Problem says: *"climbing stairs"*, *"house robber"*, *"decode ways"*, *"word break"*, *"minimum jumps"*

---

## 🧠 Intuition in Plain English

1D DP is like tracking your bank balance — each day's balance depends only on yesterday's. You don't re-simulate every transaction from day 1. You store the result of each subproblem (a day's balance) and build forward. The recurrence relation is the mathematical rule that says: "Today's answer = f(yesterday's answer, some current value)."

**The 5-step DP recipe:**
1. Define what `dp[i]` means in plain English
2. Identify the base cases
3. Write the recurrence relation
4. Fill the table (bottom-up) or memoize (top-down)
5. Return the answer (often `dp[n]` or `max(dp)`)

---

## 📐 Core Template

```python
# ─── FIBONACCI / CLIMBING STAIRS ──────────────────────────────────────────────
def climb_stairs(n):
    if n <= 2: return n

    # dp[i] = number of ways to reach stair i
    prev2, prev1 = 1, 2

    for i in range(3, n + 1):
        curr = prev1 + prev2           # From stair i-1 (1 step) or i-2 (2 steps)
        prev2, prev1 = prev1, curr

    return prev1                       # Space optimized: only need last 2 values


# ─── HOUSE ROBBER ─────────────────────────────────────────────────────────────
def house_robber(nums):
    if len(nums) == 1: return nums[0]

    # dp[i] = max money from houses 0..i
    # Recurrence: dp[i] = max(dp[i-1], dp[i-2] + nums[i])
    prev2, prev1 = 0, 0

    for num in nums:
        curr = max(prev1, prev2 + num)
        prev2, prev1 = prev1, curr

    return prev1


# ─── WORD BREAK ───────────────────────────────────────────────────────────────
def word_break(s, word_dict):
    word_set = set(word_dict)
    n = len(s)

    # dp[i] = True if s[0..i-1] can be segmented
    dp = [False] * (n + 1)
    dp[0] = True                       # Empty string is always valid

    for i in range(1, n + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_set:   # s[j..i-1] is a valid word
                dp[i] = True
                break

    return dp[n]


# ─── DECODE WAYS ──────────────────────────────────────────────────────────────
def num_decodings(s):
    n = len(s)
    if s[0] == '0': return 0

    dp = [0] * (n + 1)
    dp[0] = 1                          # Empty string: 1 way
    dp[1] = 1                          # Single char (non-zero): 1 way

    for i in range(2, n + 1):
        one_digit = int(s[i-1])
        two_digits = int(s[i-2:i])

        if one_digit != 0:             # s[i-1] alone is valid
            dp[i] += dp[i-1]
        if 10 <= two_digits <= 26:     # s[i-2..i-1] together is valid
            dp[i] += dp[i-2]

    return dp[n]
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Recurrence |
|---|---------|------------|------------|
| 1 | [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) | 🟢 Easy | `dp[i] = dp[i-1] + dp[i-2]` |
| 2 | [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/) | 🟢 Easy | `dp[i] = min(dp[i-1], dp[i-2]) + cost[i]` |
| 3 | [House Robber](https://leetcode.com/problems/house-robber/) | 🟡 Medium | `dp[i] = max(dp[i-1], dp[i-2]+nums[i])` |
| 4 | [House Robber II](https://leetcode.com/problems/house-robber-ii/) | 🟡 Medium | Run robber twice on sliced arrays |
| 5 | [Decode Ways](https://leetcode.com/problems/decode-ways/) | 🟡 Medium | 1-digit + 2-digit transitions |
| 6 | [Word Break](https://leetcode.com/problems/word-break/) | 🟡 Medium | `dp[i] = any(dp[j] and s[j:i] in dict)` |
| 7 | [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) | 🟡 Medium | `dp[i] = max(dp[j]+1) where nums[j]<nums[i]` |
| 8 | [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/) | 🟡 Medium | Track max AND min (negatives flip sign) |

---
---

# Pattern 16 — Dynamic Programming: Knapsack

**Category:** DP · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Choose a **subset** of items subject to a **capacity/budget constraint**
- Maximize value or determine if a **target sum is achievable**
- **Include or exclude** each element is the fundamental decision
- Problem says: *"partition equal subset"*, *"target sum"*, *"can you achieve sum W"*, *"bounded items"*, *"unbounded coins"*

**Two main subtypes:**
- **0/1 Knapsack:** Each item used at most once
- **Unbounded Knapsack:** Each item can be used unlimited times (Coin Change)

---

## 🧠 Intuition in Plain English

You're packing a hiking bag with a weight limit. For each item, you face a binary decision: pack it or leave it. The total packing strategy can't be decided greedily — a cheap item now might block a much more valuable item later. DP solves this by recording the optimal packing for every possible remaining weight, building up from weight 0 to the limit.

---

## 📐 Core Template

```python
# ─── 0/1 KNAPSACK ─────────────────────────────────────────────────────────────
def knapsack_01(weights, values, capacity):
    n = len(weights)
    # dp[i][w] = max value using first i items with capacity w
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        w, v = weights[i-1], values[i-1]
        for cap in range(capacity + 1):
            # Option 1: Don't take item i
            dp[i][cap] = dp[i-1][cap]
            # Option 2: Take item i (if it fits)
            if cap >= w:
                dp[i][cap] = max(dp[i][cap], dp[i-1][cap - w] + v)

    return dp[n][capacity]


# ─── PARTITION EQUAL SUBSET SUM (0/1 Knapsack as boolean) ────────────────────
def can_partition(nums):
    total = sum(nums)
    if total % 2 != 0:
        return False

    target = total // 2
    # dp[j] = can we achieve sum j using a subset of nums?
    dp = [False] * (target + 1)
    dp[0] = True                       # Sum 0 is always achievable (empty subset)

    for num in nums:
        # Iterate BACKWARDS to prevent using same item twice
        for j in range(target, num - 1, -1):
            dp[j] = dp[j] or dp[j - num]

    return dp[target]


# ─── UNBOUNDED KNAPSACK (Coin Change) ─────────────────────────────────────────
def coin_change(coins, amount):
    # dp[i] = minimum coins to make amount i
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0                          # 0 coins needed to make 0

    for coin in coins:
        # Iterate FORWARD: items can be reused
        for i in range(coin, amount + 1):
            dp[i] = min(dp[i], dp[i - coin] + 1)

    return dp[amount] if dp[amount] != float('inf') else -1
```

---

## 🔑 The Key Loop Direction Rule

```
0/1 Knapsack (each item once):    iterate j from HIGH → LOW (backward)
Unbounded Knapsack (reuse ok):    iterate j from LOW  → HIGH (forward)
```

This single rule prevents the most common knapsack bugs.

---

## 📝 Problem Set

| # | Problem | Difficulty | Knapsack Type |
|---|---------|------------|---------------|
| 1 | [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/) | 🟡 Medium | 0/1 boolean |
| 2 | [Coin Change](https://leetcode.com/problems/coin-change/) | 🟡 Medium | Unbounded, minimize count |
| 3 | [Coin Change II](https://leetcode.com/problems/coin-change-ii/) | 🟡 Medium | Unbounded, count ways |
| 4 | [Target Sum](https://leetcode.com/problems/target-sum/) | 🟡 Medium | 0/1 count ways |
| 5 | [Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/) | 🟡 Medium | Partition into equal sums |
| 6 | [Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/) | 🟡 Medium | 2D knapsack (m zeros, n ones budget) |
| 7 | [Perfect Squares](https://leetcode.com/problems/perfect-squares/) | 🟡 Medium | Unbounded: min squares to sum to n |

---
---

# Pattern 17 — Dynamic Programming: 2D / Strings

**Category:** DP · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Problems on **two strings**: comparing, matching, transforming
- Problems on **grids**: paths, minimum cost, unique ways
- **Subsequence** or **substring** matching
- Problem says: *"longest common subsequence"*, *"edit distance"*, *"distinct subsequences"*, *"unique paths"*, *"minimum path sum"*

---

## 🧠 Intuition in Plain English

When you have two sequences (two strings or a grid with two dimensions), the subproblem involves choosing a prefix of each. `dp[i][j]` represents the answer for the first `i` characters of string 1 and the first `j` characters of string 2. The table fills left-to-right, top-to-bottom, each cell building on its neighbors.

---

## 📐 Core Template

```python
# ─── LONGEST COMMON SUBSEQUENCE ───────────────────────────────────────────────
def lcs(text1, text2):
    m, n = len(text1), len(text2)
    # dp[i][j] = LCS of text1[0..i-1] and text2[0..j-1]
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1      # Characters match: extend LCS
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])  # Take best of skipping one

    return dp[m][n]


# ─── EDIT DISTANCE ────────────────────────────────────────────────────────────
def edit_distance(word1, word2):
    m, n = len(word1), len(word2)
    # dp[i][j] = min operations to convert word1[0..i-1] to word2[0..j-1]
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    # Base cases: convert to/from empty string
    for i in range(m + 1): dp[i][0] = i   # Delete all chars
    for j in range(n + 1): dp[0][j] = j   # Insert all chars

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i-1] == word2[j-1]:
                dp[i][j] = dp[i-1][j-1]           # No operation needed
            else:
                dp[i][j] = 1 + min(
                    dp[i-1][j],                    # Delete from word1
                    dp[i][j-1],                    # Insert into word1
                    dp[i-1][j-1]                   # Replace
                )

    return dp[m][n]


# ─── UNIQUE PATHS (grid DP) ───────────────────────────────────────────────────
def unique_paths(m, n):
    # dp[i][j] = number of ways to reach cell (i, j)
    dp = [[1] * n for _ in range(m)]   # First row and col = 1 (only one way)

    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i-1][j] + dp[i][j-1]   # From above + from left

    return dp[m-1][n-1]
```

---

## 📝 Problem Set

| # | Problem | Difficulty | dp[i][j] Meaning |
|---|---------|------------|-----------------|
| 1 | [Unique Paths](https://leetcode.com/problems/unique-paths/) | 🟡 Medium | Ways to reach cell (i,j) |
| 2 | [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/) | 🟡 Medium | Min cost to reach cell (i,j) |
| 3 | [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) | 🟡 Medium | LCS of first i, j chars |
| 4 | [Edit Distance](https://leetcode.com/problems/edit-distance/) | 🟡 Medium | Min edits to convert prefixes |
| 5 | [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/) | 🟡 Medium | `lcs(s, reverse(s))` |
| 6 | [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/) | 🔴 Hard | Count ways to form t from s |
| 7 | [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/) | 🔴 Hard | Pattern match with `*` and `.` |

---
---

# Pattern 18 — Dynamic Programming: Interval

**Category:** DP · **Difficulty Band:** Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Problems on **contiguous segments** of an array where you make a decision about the whole segment
- Merging or splitting arrays for minimum/maximum cost
- Matrix chain multiplication style problems
- Problem says: *"burst balloons"*, *"remove boxes"*, *"strange printer"*, *"minimum cost to merge stones"*

---

## 🧠 Intuition in Plain English

Interval DP solves problems where the answer for a range `[i, j]` depends on splitting it at some pivot `k` and combining the results of `[i, k]` and `[k+1, j]`. You build up from smallest intervals (length 1) to the full range. The key insight: unlike 1D DP which fills a line, interval DP fills a triangle of the 2D table.

---

## 📐 Core Template

```python
# ─── INTERVAL DP TEMPLATE ─────────────────────────────────────────────────────
def interval_dp(arr):
    n = len(arr)
    # dp[i][j] = answer for subarray arr[i..j]
    dp = [[0] * n for _ in range(n)]

    # Fill by increasing interval LENGTH
    for length in range(2, n + 1):              # Length from 2 to n
        for i in range(n - length + 1):
            j = i + length - 1                  # Right endpoint

            dp[i][j] = float('inf')             # Or -inf, depending on problem

            for k in range(i, j):               # Try every split point
                # Combine dp[i][k] and dp[k+1][j]
                cost = dp[i][k] + dp[k+1][j] + merge_cost(arr, i, j, k)
                dp[i][j] = min(dp[i][j], cost)

    return dp[0][n-1]


# ─── BURST BALLOONS ───────────────────────────────────────────────────────────
def max_coins(nums):
    # Add boundary balloons with value 1
    nums = [1] + nums + [1]
    n = len(nums)
    dp = [[0] * n for _ in range(n)]

    # dp[i][j] = max coins from bursting all balloons between i and j (exclusive)
    for length in range(2, n):
        for left in range(0, n - length):
            right = left + length
            for k in range(left + 1, right):   # k = last balloon to burst in range
                coins = nums[left] * nums[k] * nums[right]
                dp[left][right] = max(dp[left][right],
                                      dp[left][k] + coins + dp[k][right])

    return dp[0][n-1]
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Interval Insight |
|---|---------|------------|-----------------|
| 1 | [Minimum Cost Tree from Leaf Values](https://leetcode.com/problems/minimum-cost-tree-from-leaf-values/) | 🟡 Medium | Split leaf arrays |
| 2 | [Burst Balloons](https://leetcode.com/problems/burst-balloons/) | 🔴 Hard | Last balloon to burst, not first |
| 3 | [Strange Printer](https://leetcode.com/problems/strange-printer/) | 🔴 Hard | When s[i] == s[j], extend interval |
| 4 | [Minimum Cost to Merge Stones](https://leetcode.com/problems/minimum-cost-to-merge-stones/) | 🔴 Hard | Only merge k consecutive piles |
| 5 | [Remove Boxes](https://leetcode.com/problems/remove-boxes/) | 🔴 Hard | 3D DP variant |

---
---

# Pattern 19 — Monotonic Stack

**Category:** Stack · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- For each element, find the **next/previous greater or smaller element**
- **Histogram** area problems
- Problems involving **spans** (stock span, daily temperatures)
- Sliding window minimum/maximum (→ use Monotonic Queue, Pattern 20)
- Problem says: *"next greater element"*, *"daily temperatures"*, *"stock span"*, *"largest rectangle"*, *"trap water"*

---

## 🧠 Intuition in Plain English

A monotonic stack maintains elements in sorted order (increasing or decreasing). When a new element breaks the sort order, it becomes the "answer" for every element it pops off. Imagine a queue of people waiting at a coffee shop — whenever a taller person arrives, everyone shorter in front of them can finally see who's ahead of them (the taller person is their "next greater element"). The stack holds the "still waiting" elements.

---

## 📐 Core Template

```python
# ─── NEXT GREATER ELEMENT (decreasing stack) ─────────────────────────────────
def next_greater(nums):
    n = len(nums)
    result = [-1] * n                  # Default: no greater element exists
    stack = []                         # Stack of indices, values decrease bottom→top

    for i in range(n):
        # Current element is GREATER than stack top → it's the answer for top
        while stack and nums[i] > nums[stack[-1]]:
            idx = stack.pop()
            result[idx] = nums[i]      # nums[i] is next greater for nums[idx]
        stack.append(i)

    return result

# Monotonic INCREASING stack → finds next SMALLER element
# Monotonic DECREASING stack → finds next GREATER element


# ─── LARGEST RECTANGLE IN HISTOGRAM ──────────────────────────────────────────
def largest_rectangle(heights):
    stack = []                         # Monotonic increasing stack of indices
    max_area = 0
    heights = heights + [0]            # Sentinel 0 flushes all remaining bars

    for i, h in enumerate(heights):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            # Width: from current position back to previous smaller bar
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)

    return max_area


# ─── DAILY TEMPERATURES ───────────────────────────────────────────────────────
def daily_temperatures(temps):
    n = len(temps)
    result = [0] * n
    stack = []                         # Stack of indices

    for i in range(n):
        while stack and temps[i] > temps[stack[-1]]:
            idx = stack.pop()
            result[idx] = i - idx      # Days until warmer = index difference
        stack.append(i)

    return result
```

---

## 🔑 Key Insight

```
When you POP from the stack, the current element is the answer for the popped element.
What "answer" means depends on the problem:
  - Next greater: result[popped_idx] = current_value
  - Days until warmer: result[popped_idx] = current_idx - popped_idx
  - Largest rectangle: area = height[popped] × width_calculation
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Stack Type |
|---|---------|------------|------------|
| 1 | [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/) | 🟢 Easy | Decreasing stack |
| 2 | [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) | 🟡 Medium | Days until warmer |
| 3 | [Online Stock Span](https://leetcode.com/problems/online-stock-span/) | 🟡 Medium | Previous greater (reverse) |
| 4 | [Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums/) | 🟡 Medium | Next/prev smaller + contribution |
| 5 | [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) | 🔴 Hard | Increasing stack + area calc |
| 6 | [Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/) | 🔴 Hard | Histogram per row of matrix |
| 7 | [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) | 🔴 Hard | Stack or two-pointer both work |

---
---

# Pattern 20 — Monotonic Queue (Deque)

**Category:** Queue · **Difficulty Band:** Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Sliding window maximum or minimum** (fixed-size window that slides)
- Need to efficiently track max/min as a window moves
- Problems where you need the maximum/minimum in a recent window of size K
- Problem says: *"sliding window maximum"*, *"maximum in window"*, *"shortest subarray"*

---

## 🧠 Intuition in Plain English

A regular sliding window tracks content with a hash map. But if you need the *maximum* of the window at all times, a hash map is O(log n) or O(n) to find the max. A monotonic deque maintains a window's maximum in amortized O(1): **the front of the deque is always the current maximum**. Elements that can never be the maximum (because a larger element entered after them) are kicked out the back immediately.

---

## 📐 Core Template

```python
from collections import deque

def sliding_window_maximum(nums, k):
    dq = deque()                       # Stores INDICES, values monotonically decreasing
    result = []

    for i, num in enumerate(nums):
        # Remove elements outside the window from the FRONT
        while dq and dq[0] < i - k + 1:
            dq.popleft()

        # Remove smaller elements from the BACK (they can never be max)
        while dq and nums[dq[-1]] < num:
            dq.pop()

        dq.append(i)

        # Window is fully formed after the first k elements
        if i >= k - 1:
            result.append(nums[dq[0]])  # Front is always the current maximum

    return result
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Insight |
|---|---------|------------|---------|
| 1 | [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) | 🔴 Hard | Classic monotonic deque |
| 2 | [Shortest Subarray with Sum at Least K](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/) | 🔴 Hard | Prefix sum + monotonic deque |
| 3 | [Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum/) | 🔴 Hard | DP + monotonic deque for window max |

---

---

# 📊 Part 2 Quick Reference Card

| Pattern | Core Data Structure | Key Trigger |
|---------|---------------------|-------------|
| BFS | Queue (deque) | Shortest path, level order, minimum steps |
| DFS | Stack / Recursion | All paths, connectivity, cycle detection |
| Tree DFS | Recursion | Post-order: aggregate from children upward |
| Backtracking | Recursion + Undo | Generate all valid combinations |
| DP 1D | Array | Linear recurrence, sequential choices |
| DP Knapsack | Array | Include/exclude items under capacity |
| DP 2D/Strings | 2D Array | Two sequences, grid traversal |
| DP Interval | 2D Array | Split range at pivot, fill by length |
| Monotonic Stack | Stack | Next/prev greater/smaller element |
| Monotonic Queue | Deque | Sliding window max/min |

---

> 📘 **Continue to Part 3** for Patterns 21–30: Heap, Intervals, Graph Algorithms, Union-Find, Trie, Segment Tree, Linked List Manipulation, Matrix Patterns, and more — plus the complete 30-pattern cheat sheet.

---
*Part 2 of 3 — DSA Pattern Mastery Guide*
