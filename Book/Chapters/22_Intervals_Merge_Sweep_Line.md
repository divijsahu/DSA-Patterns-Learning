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
