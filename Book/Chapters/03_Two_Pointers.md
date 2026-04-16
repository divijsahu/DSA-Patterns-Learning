# Pattern 3 — Two Pointers

**Category:** Array/String · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Array or string is **sorted** (or sorting would help)
- Find a **pair, triplet, or group** that satisfies a numeric condition
- Need to perform **in-place** operations (reverse, remove, partition)
- Compare elements from **both ends** toward the middle
- Merge two sorted arrays/lists
- Problem says: *"two sum in sorted"*, *"palindrome"*, *"remove duplicates in-place"*, *"3Sum"*, *"container with most water"*

**This pattern is NOT for:**
- Unsorted arrays where you can't reason about which pointer to move (use Hash Map)
- Subarray problems (use Sliding Window)

---

## 🧠 Intuition in Plain English

Picture two people walking toward each other on a number line. The left person represents "too small" and the right person represents "too large." Because the array is sorted, moving left rightward increases the sum, and moving right leftward decreases it. This navigation eliminates the need for nested loops — you converge on the answer in one pass. It's like a binary search but for pairs.

---

## 🔮 Pattern Prediction Checklist

- [ ] Is the array **sorted** (or should I sort it first)? → Two Pointers viable
- [ ] Am I finding a **pair/triplet** that meets a sum/difference constraint? → Two Pointers
- [ ] Is the operation **in-place** without extra space? → Two Pointers (write pointer variant)
- [ ] Am I comparing elements from **both ends**? → Two Pointers (inward converging)

---

## 📐 Core Template

```python
# ─── VARIANT 1: CONVERGING (find pair with target sum) ───────────────────────
def two_sum_sorted(arr, target):
    left, right = 0, len(arr) - 1

    while left < right:
        current = arr[left] + arr[right]

        if current == target:
            return [left, right]
        elif current < target:
            left += 1          # Sum too small → move left pointer right
        else:
            right -= 1         # Sum too big → move right pointer left

    return []

# ─── VARIANT 2: SAME DIRECTION (write pointer for in-place ops) ──────────────
def remove_duplicates(arr):
    if not arr:
        return 0

    write = 1                  # Points to where next unique element goes

    for read in range(1, len(arr)):
        if arr[read] != arr[write - 1]:    # New unique element found
            arr[write] = arr[read]
            write += 1

    return write               # Length of deduplicated array

# ─── VARIANT 3: 3SUM (fix one, two-pointer the rest) ─────────────────────────
def three_sum(nums):
    nums.sort()
    result = []

    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i - 1]:   # Skip duplicates for fixed element
            continue

        left, right = i + 1, len(nums) - 1

        while left < right:
            total = nums[i] + nums[left] + nums[right]

            if total == 0:
                result.append([nums[i], nums[left], nums[right]])
                while left < right and nums[left] == nums[left + 1]:
                    left += 1                   # Skip duplicates
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1
                left += 1
                right -= 1
            elif total < 0:
                left += 1
            else:
                right -= 1

    return result
```

---

## 🔀 Variants

| Variant | Description | Direction |
|---------|-------------|-----------|
| **Converging** | Find pair from both ends | →←  |
| **Same Direction** | Read/write pointer for in-place | →→ |
| **3Sum / kSum** | Fix outer, two-pointer inner | Fix + →← |
| **Dutch National Flag** | Partition into 3 groups | 3 pointers |

---

## ⏱️ Complexity

| | Brute Force | Two Pointers |
|-|-------------|--------------|
| **Time (pair)** | O(n²) | O(n) |
| **Time (triplet)** | O(n³) | O(n²) |
| **Space** | O(1) | O(1) |

---

## ❌ Anti-patterns & Traps

- **Forgetting to sort:** Two pointers only work on sorted data (or data where pointer direction has a predictable effect).
- **Duplicate handling in 3Sum:** Skip over duplicate values after finding a valid triplet, or you'll output duplicate triplets.
- **`while left < right` vs `while left <= right`:** Use `<` for pair finding (stop before they cross), `<=` when single pointer can be valid.

---

## 📝 Problem Set

| # | Problem | Difficulty | Key Insight |
|---|---------|------------|-------------|
| 1 | [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) | 🟢 Easy | Compare chars from both ends |
| 2 | [Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) | 🟢 Easy | Classic converging pointers |
| 3 | [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) | 🟢 Easy | Write pointer pattern |
| 4 | [Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/) | 🟢 Easy | Place larger square from the back |
| 5 | [3Sum](https://leetcode.com/problems/3sum/) | 🟡 Medium | Fix one + two-pointer rest |
| 6 | [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) | 🟡 Medium | Always move the shorter wall |
| 7 | [Sort Colors (Dutch Flag)](https://leetcode.com/problems/sort-colors/) | 🟡 Medium | Three-pointer partition |
| 8 | [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) | 🔴 Hard | Track max from both sides as you go |

---
---
