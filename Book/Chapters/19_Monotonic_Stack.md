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
