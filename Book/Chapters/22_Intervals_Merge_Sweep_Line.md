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
