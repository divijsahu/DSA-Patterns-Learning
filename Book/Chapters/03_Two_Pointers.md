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

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> two_sum_sorted(const vector<int>& arr, int target) {
    int left = 0, right = (int)arr.size() - 1;
    while (left < right) {
        int current = arr[left] + arr[right];
        if (current == target) return {left, right};
        if (current < target) ++left;
        else --right;
    }
    return {};
}

int remove_duplicates(vector<int>& arr) {
    if (arr.empty()) return 0;
    int write = 1;
    for (int read = 1; read < (int)arr.size(); ++read) {
        if (arr[read] != arr[write - 1]) {
            arr[write++] = arr[read];
        }
    }
    return write;
}

vector<vector<int>> three_sum(vector<int> nums) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> result;

    for (int i = 0; i + 2 < (int)nums.size(); ++i) {
        if (i > 0 && nums[i] == nums[i - 1]) continue;
        int left = i + 1, right = (int)nums.size() - 1;
        while (left < right) {
            long long total = 1LL * nums[i] + nums[left] + nums[right];
            if (total == 0) {
                result.push_back({nums[i], nums[left], nums[right]});
                while (left < right && nums[left] == nums[left + 1]) ++left;
                while (left < right && nums[right] == nums[right - 1]) --right;
                ++left;
                --right;
            } else if (total < 0) {
                ++left;
            } else {
                --right;
            }
        }
    }

    return result;
}
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
