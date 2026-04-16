# ⚡ THE COMPLETE DSA PATTERN MASTERY GUIDE
## Part 2 of 3 — Intermediate Patterns (Patterns 11–20)

> *"Trees and graphs aren't scary. They're just recursion wearing a Halloween costume."*

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

```cpp
#include <bits/stdc++.h>
using namespace std;

void dfs(const unordered_map<int, vector<int>>& graph, int node, unordered_set<int>& visited) {
    visited.insert(node);
    auto it = graph.find(node);
    if (it == graph.end()) return;
    for (int neighbor : it->second) {
        if (!visited.count(neighbor)) dfs(graph, neighbor, visited);
    }
}

void dfs_iterative(const unordered_map<int, vector<int>>& graph, int start) {
    stack<int> st;
    unordered_set<int> visited;
    st.push(start);
    visited.insert(start);

    while (!st.empty()) {
        int node = st.top(); st.pop();
        auto it = graph.find(node);
        if (it == graph.end()) continue;
        for (int neighbor : it->second) {
            if (!visited.count(neighbor)) {
                visited.insert(neighbor);
                st.push(neighbor);
            }
        }
    }
}

bool has_cycle_directed(const vector<vector<int>>& graph) {
    int n = graph.size();
    vector<int> state(n, 0);

    function<bool(int)> dfs_cycle = [&](int node) {
        state[node] = 1;
        for (int neighbor : graph[node]) {
            if (state[neighbor] == 1) return true;
            if (state[neighbor] == 0 && dfs_cycle(neighbor)) return true;
        }
        state[node] = 2;
        return false;
    };

    for (int i = 0; i < n; ++i) {
        if (state[i] == 0 && dfs_cycle(i)) return true;
    }
    return false;
}

void dfs_grid(vector<vector<int>>& grid, int r, int c) {
    int rows = grid.size(), cols = grid[0].size();
    if (r < 0 || r >= rows || c < 0 || c >= cols || grid[r][c] != 1) return;
    grid[r][c] = 0;
    dfs_grid(grid, r + 1, c);
    dfs_grid(grid, r - 1, c);
    dfs_grid(grid, r, c + 1);
    dfs_grid(grid, r, c - 1);
}
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

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int diameterOfBinaryTree(TreeNode* root) {
    int best = 0;
    function<int(TreeNode*)> height = [&](TreeNode* node) {
        if (!node) return 0;
        int left_h = height(node->left);
        int right_h = height(node->right);
        best = max(best, left_h + right_h);
        return 1 + max(left_h, right_h);
    };
    height(root);
    return best;
}

TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root || root == p || root == q) return root;
    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    TreeNode* right = lowestCommonAncestor(root->right, p, q);
    if (left && right) return root;
    return left ? left : right;
}

bool hasPathSum(TreeNode* root, int targetSum) {
    if (!root) return false;
    if (!root->left && !root->right) return root->val == targetSum;
    return hasPathSum(root->left, targetSum - root->val) || hasPathSum(root->right, targetSum - root->val);
}

int maxPathSum(TreeNode* root) {
    int best = INT_MIN;
    function<int(TreeNode*)> gain = [&](TreeNode* node) {
        if (!node) return 0;
        int left_gain = max(gain(node->left), 0);
        int right_gain = max(gain(node->right), 0);
        best = max(best, node->val + left_gain + right_gain);
        return node->val + max(left_gain, right_gain);
    };
    gain(root);
    return best;
}
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

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<vector<int>> subsets(const vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> current;
    function<void(int)> backtrack = [&](int start) {
        result.push_back(current);
        for (int i = start; i < (int)nums.size(); ++i) {
            current.push_back(nums[i]);
            backtrack(i + 1);
            current.pop_back();
        }
    };
    backtrack(0);
    return result;
}

vector<vector<int>> permutations(const vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> current;
    vector<bool> used(nums.size(), false);
    function<void()> backtrack = [&]() {
        if ((int)current.size() == (int)nums.size()) {
            result.push_back(current);
            return;
        }
        for (int i = 0; i < (int)nums.size(); ++i) {
            if (used[i]) continue;
            used[i] = true;
            current.push_back(nums[i]);
            backtrack();
            current.pop_back();
            used[i] = false;
        }
    };
    backtrack();
    return result;
}

vector<vector<int>> combinationSum(vector<int> candidates, int target) {
    sort(candidates.begin(), candidates.end());
    vector<vector<int>> result;
    vector<int> current;
    function<void(int, int)> backtrack = [&](int start, int remaining) {
        if (remaining == 0) {
            result.push_back(current);
            return;
        }
        for (int i = start; i < (int)candidates.size(); ++i) {
            if (candidates[i] > remaining) break;
            current.push_back(candidates[i]);
            backtrack(i, remaining - candidates[i]);
            current.pop_back();
        }
    };
    backtrack(0, target);
    return result;
}

vector<vector<string>> solveNQueens(int n) {
    vector<vector<string>> result;
    vector<string> board(n, string(n, '.'));
    unordered_set<int> cols, pos_diag, neg_diag;

    function<void(int)> backtrack = [&](int row) {
        if (row == n) {
            result.push_back(board);
            return;
        }
        for (int col = 0; col < n; ++col) {
            if (cols.count(col) || pos_diag.count(row + col) || neg_diag.count(row - col)) continue;
            cols.insert(col);
            pos_diag.insert(row + col);
            neg_diag.insert(row - col);
            board[row][col] = 'Q';
            backtrack(row + 1);
            board[row][col] = '.';
            cols.erase(col);
            pos_diag.erase(row + col);
            neg_diag.erase(row - col);
        }
    };
    backtrack(0);
    return result;
}
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

```cpp
#include <bits/stdc++.h>
using namespace std;

int climbStairs(int n) {
    if (n <= 2) return n;
    int prev2 = 1, prev1 = 2;
    for (int i = 3; i <= n; ++i) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}

int houseRobber(const vector<int>& nums) {
    if (nums.size() == 1) return nums[0];
    int prev2 = 0, prev1 = 0;
    for (int num : nums) {
        int curr = max(prev1, prev2 + num);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}

bool wordBreak(const string& s, const vector<string>& wordDict) {
    unordered_set<string> wordSet(wordDict.begin(), wordDict.end());
    int n = s.size();
    vector<bool> dp(n + 1, false);
    dp[0] = true;
    for (int i = 1; i <= n; ++i) {
        for (int j = 0; j < i; ++j) {
            if (dp[j] && wordSet.count(s.substr(j, i - j))) {
                dp[i] = true;
                break;
            }
        }
    }
    return dp[n];
}

int numDecodings(const string& s) {
    if (s.empty() || s[0] == '0') return 0;
    int n = s.size();
    vector<int> dp(n + 1, 0);
    dp[0] = 1;
    dp[1] = 1;
    for (int i = 2; i <= n; ++i) {
        int one_digit = s[i - 1] - '0';
        int two_digits = stoi(s.substr(i - 2, 2));
        if (one_digit != 0) dp[i] += dp[i - 1];
        if (10 <= two_digits && two_digits <= 26) dp[i] += dp[i - 2];
    }
    return dp[n];
}
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

```cpp
#include <bits/stdc++.h>
using namespace std;

int knapsack_01(const vector<int>& weights, const vector<int>& values, int capacity) {
    int n = weights.size();
    vector<vector<int>> dp(n + 1, vector<int>(capacity + 1, 0));
    for (int i = 1; i <= n; ++i) {
        int w = weights[i - 1], v = values[i - 1];
        for (int cap = 0; cap <= capacity; ++cap) {
            dp[i][cap] = dp[i - 1][cap];
            if (cap >= w) dp[i][cap] = max(dp[i][cap], dp[i - 1][cap - w] + v);
        }
    }
    return dp[n][capacity];
}

bool canPartition(const vector<int>& nums) {
    int total = accumulate(nums.begin(), nums.end(), 0);
    if (total % 2 != 0) return false;
    int target = total / 2;
    vector<bool> dp(target + 1, false);
    dp[0] = true;
    for (int num : nums) {
        for (int j = target; j >= num; --j) {
            dp[j] = dp[j] || dp[j - num];
        }
    }
    return dp[target];
}

int coinChange(const vector<int>& coins, int amount) {
    const int INF = 1e9;
    vector<int> dp(amount + 1, INF);
    dp[0] = 0;
    for (int coin : coins) {
        for (int i = coin; i <= amount; ++i) {
            dp[i] = min(dp[i], dp[i - coin] + 1);
        }
    }
    return dp[amount] == INF ? -1 : dp[amount];
}
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

```cpp
#include <bits/stdc++.h>
using namespace std;

int longestCommonSubsequence(const string& text1, const string& text2) {
    int m = text1.size(), n = text2.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    for (int i = 1; i <= m; ++i) {
        for (int j = 1; j <= n; ++j) {
            if (text1[i - 1] == text2[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
            else dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
        }
    }
    return dp[m][n];
}

int editDistance(const string& word1, const string& word2) {
    int m = word1.size(), n = word2.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    for (int i = 0; i <= m; ++i) dp[i][0] = i;
    for (int j = 0; j <= n; ++j) dp[0][j] = j;
    for (int i = 1; i <= m; ++i) {
        for (int j = 1; j <= n; ++j) {
            if (word1[i - 1] == word2[j - 1]) dp[i][j] = dp[i - 1][j - 1];
            else dp[i][j] = 1 + min({dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]});
        }
    }
    return dp[m][n];
}

bool isInterleave(const string& s1, const string& s2, const string& s3) {
    int m = s1.size(), n = s2.size();
    if (m + n != (int)s3.size()) return false;
    vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
    dp[0][0] = true;
    for (int i = 0; i <= m; ++i) {
        for (int j = 0; j <= n; ++j) {
            if (i > 0) dp[i][j] = dp[i][j] || (dp[i - 1][j] && s1[i - 1] == s3[i + j - 1]);
            if (j > 0) dp[i][j] = dp[i][j] || (dp[i][j - 1] && s2[j - 1] == s3[i + j - 1]);
        }
    }
    return dp[m][n];
}
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

```cpp
#include <bits/stdc++.h>
using namespace std;

int burstBalloons(vector<int> nums) {
    int n = nums.size();
    nums.insert(nums.begin(), 1);
    nums.push_back(1);
    vector<vector<int>> dp(n + 2, vector<int>(n + 2, 0));

    for (int len = 2; len < n + 2; ++len) {
        for (int left = 0; left + len < n + 2; ++left) {
            int right = left + len;
            for (int last = left + 1; last < right; ++last) {
                dp[left][right] = max(dp[left][right], dp[left][last] + nums[left] * nums[last] * nums[right] + dp[last][right]);
            }
        }
    }
    return dp[0][n + 1];
}
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

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> nextGreaterElement(const vector<int>& nums) {
    vector<int> result(nums.size(), -1);
    stack<int> st;
    for (int i = 0; i < (int)nums.size(); ++i) {
        while (!st.empty() && nums[i] > nums[st.top()]) {
            result[st.top()] = nums[i];
            st.pop();
        }
        st.push(i);
    }
    return result;
}

vector<int> dailyTemperatures(const vector<int>& temperatures) {
    vector<int> result(temperatures.size(), 0);
    stack<int> st;
    for (int i = 0; i < (int)temperatures.size(); ++i) {
        while (!st.empty() && temperatures[i] > temperatures[st.top()]) {
            int idx = st.top(); st.pop();
            result[idx] = i - idx;
        }
        st.push(i);
    }
    return result;
}

int largestRectangleArea(const vector<int>& heights) {
    vector<int> h = heights;
    h.push_back(0);
    stack<int> st;
    int best = 0;
    for (int i = 0; i < (int)h.size(); ++i) {
        while (!st.empty() && h[i] < h[st.top()]) {
            int height = h[st.top()];
            st.pop();
            int left = st.empty() ? -1 : st.top();
            best = max(best, height * (i - left - 1));
        }
        st.push(i);
    }
    return best;
}
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

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> maxSlidingWindow(const vector<int>& nums, int k) {
    deque<int> dq;
    vector<int> result;
    for (int i = 0; i < (int)nums.size(); ++i) {
        while (!dq.empty() && dq.front() <= i - k) dq.pop_front();
        while (!dq.empty() && nums[dq.back()] <= nums[i]) dq.pop_back();
        dq.push_back(i);
        if (i >= k - 1) result.push_back(nums[dq.front()]);
    }
    return result;
}
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
