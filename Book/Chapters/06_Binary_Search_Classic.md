# Pattern 6 — Binary Search (Classic)

**Category:** Search · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Data is **sorted** (array, matrix with sorted rows, etc.)
- Need O(log n) lookup
- Find **first/last occurrence** of a value
- Find if a value **exists** in a sorted structure
- Problem says: *"search in sorted"*, *"find position"*, *"rotated sorted array"*, *"first occurrence"*

**This pattern is NOT for:**
- Unsorted data (sort first, then binary search if O(n log n) is acceptable)
- When you need the answer's exact index in the original (might need Pattern 7 instead)

---

## 🧠 Intuition in Plain English

You're looking for a name in a phone book. You don't start from page 1 — you open the middle, see if the name comes before or after, and discard half the book. Repeat. After log₂(n) flips, you've found it. Binary search is the algorithmic embodiment of this: **every comparison eliminates half the remaining search space**.

---

## 🔮 Pattern Prediction Checklist

- [ ] Is the array **sorted**? → Binary Search
- [ ] Am I looking for an **exact value** or **first/last occurrence**? → Binary Search
- [ ] Is the required time complexity **O(log n)**? → Binary Search is almost certainly the answer
- [ ] Is the array **rotated sorted**? → Modified Binary Search (one half is always sorted)

---

## 📐 Core Template

```python
# ─── CLASSIC BINARY SEARCH ────────────────────────────────────────────────────
def binary_search(arr, target):
    left, right = 0, len(arr) - 1

    while left <= right:
        mid = left + (right - left) // 2     # NEVER use (left + right) // 2
                                              # → integer overflow in other languages

        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1                    # Target is in RIGHT half
        else:
            right = mid - 1                   # Target is in LEFT half

    return -1                                 # Not found

# ─── FIND LEFTMOST (FIRST) OCCURRENCE ─────────────────────────────────────────
def find_first(arr, target):
    left, right = 0, len(arr) - 1
    result = -1

    while left <= right:
        mid = left + (right - left) // 2

        if arr[mid] == target:
            result = mid           # Record, but keep searching LEFT
            right = mid - 1
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return result

# ─── ROTATED SORTED ARRAY ─────────────────────────────────────────────────────
def search_rotated(arr, target):
    left, right = 0, len(arr) - 1

    while left <= right:
        mid = left + (right - left) // 2

        if arr[mid] == target:
            return mid

        # KEY INSIGHT: One half is ALWAYS sorted in a rotated array
        if arr[left] <= arr[mid]:                    # Left half is sorted
            if arr[left] <= target < arr[mid]:
                right = mid - 1                      # Target is in left half
            else:
                left = mid + 1                       # Target is in right half
        else:                                        # Right half is sorted
            if arr[mid] < target <= arr[right]:
                left = mid + 1                       # Target is in right half
            else:
                right = mid - 1                      # Target is in left half

    return -1
```

---

## ⏱️ Complexity

| | Time | Space |
|-|------|-------|
| Classic | O(log n) | O(1) |
| First/Last occurrence | O(log n) | O(1) |

---

## ❌ Anti-patterns & Traps

- **Infinite loop when `left = mid` or `right = mid`:** Always do `left = mid + 1` and `right = mid - 1` in classic search.
- **Integer overflow:** Use `mid = left + (right - left) // 2`, not `(left + right) // 2`.
- **Off-by-one on `while` condition:** `left <= right` for exact search, `left < right` for finding boundaries.

---

## 📝 Problem Set

| # | Problem | Difficulty | Key Insight |
|---|---------|------------|-------------|
| 1 | [Binary Search](https://leetcode.com/problems/binary-search/) | 🟢 Easy | Pure classic template |
| 2 | [Search Insert Position](https://leetcode.com/problems/search-insert-position/) | 🟢 Easy | Return `left` if not found |
| 3 | [First Bad Version](https://leetcode.com/problems/first-bad-version/) | 🟢 Easy | Find leftmost `true` |
| 4 | [Find First and Last Position](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) | 🟡 Medium | Two binary searches |
| 5 | [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) | 🟡 Medium | One half always sorted |
| 6 | [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) | 🟡 Medium | Compare mid to right boundary |
| 7 | [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/) | 🟡 Medium | Treat matrix as flat sorted array |
| 8 | [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) | 🔴 Hard | Binary search on partition index |

---
---
