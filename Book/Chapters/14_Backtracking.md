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
