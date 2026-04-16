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
