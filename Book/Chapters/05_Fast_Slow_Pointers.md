# Pattern 5 — Fast & Slow Pointers

**Category:** Linked List · **Difficulty Band:** Easy–Medium

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Linked list** with possible **cycle detection**
- Find the **middle** of a linked list
- Find the **start of a cycle** in a linked list
- Find if a sequence eventually reaches a **repeated state** (like Happy Number)
- Problem says: *"cycle"*, *"circular"*, *"middle"*, *"nth from end"*, *"meeting point"*

**This pattern is NOT for:**
- Array cycle detection (use visited set or index math)
- Shortest path (use BFS)

---

## 🧠 Intuition in Plain English

Think of a race track. A fast runner (2 steps) and a slow runner (1 step) start together. If there's a loop in the track, the fast runner will eventually lap the slow runner and they'll meet. If there's no loop, the fast runner just reaches the finish line first. This is Floyd's Tortoise and Hare algorithm — elegant, O(1) space, O(n) time.

For finding the middle: when the fast pointer reaches the end, the slow pointer is exactly at the halfway point (since fast traveled twice as far).

---

## 🔮 Pattern Prediction Checklist

- [ ] Is the input a **linked list**? → Consider Fast & Slow
- [ ] Does the problem involve a **cycle** or **repeated state**? → Fast & Slow
- [ ] Do I need to find the **middle** without knowing the length? → Fast & Slow
- [ ] Do I need O(1) **extra space**? → Fast & Slow (over storing visited nodes)

---

## 📐 Core Template

```python
# ─── CYCLE DETECTION ──────────────────────────────────────────────────────────
def has_cycle(head):
    slow = fast = head

    while fast and fast.next:
        slow = slow.next           # Move 1 step
        fast = fast.next.next      # Move 2 steps

        if slow == fast:
            return True            # They met inside a cycle

    return False                   # Fast reached None → no cycle

# ─── FIND CYCLE START ─────────────────────────────────────────────────────────
def detect_cycle(head):
    slow = fast = head

    # Phase 1: Detect meeting point inside cycle
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            break
    else:
        return None                # No cycle

    # Phase 2: Find cycle start
    # Mathematical proof: distance(head → start) == distance(meeting → start)
    slow = head
    while slow != fast:
        slow = slow.next
        fast = fast.next

    return slow                    # Both pointers are now at cycle start

# ─── FIND MIDDLE ──────────────────────────────────────────────────────────────
def find_middle(head):
    slow = fast = head

    # When fast reaches end, slow is at middle
    # Even length: slow ends at second middle node
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

    return slow
```

---

## ⏱️ Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Cycle detection | O(n) | O(1) |
| Cycle start | O(n) | O(1) |
| Find middle | O(n) | O(1) |

Brute force (storing visited nodes): O(n) time but **O(n) space** — Fast & Slow gives O(1) space.

---

## ❌ Anti-patterns & Traps

- **Null check before accessing `.next.next`:** Always check `fast and fast.next`, not just `fast`.
- **Phase 2 misunderstanding:** After detecting the cycle, move `slow` back to `head` and advance **both** at speed 1.
- **Missing the Happy Number pattern:** The sequence of digit-sum computations forms a virtual linked list — use fast/slow on the *values*, not actual nodes.

---

## 📝 Problem Set

| # | Problem | Difficulty | Key Insight |
|---|---------|------------|-------------|
| 1 | [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) | 🟢 Easy | Basic fast/slow detection |
| 2 | [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/) | 🟢 Easy | Slow stops at middle when fast ends |
| 3 | [Happy Number](https://leetcode.com/problems/happy-number/) | 🟢 Easy | Digit-sum as virtual linked list |
| 4 | [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) | 🟡 Medium | Two-phase Floyd's algorithm |
| 5 | [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/) | 🟡 Medium | Array index treated as next pointer |
| 6 | [Reorder List](https://leetcode.com/problems/reorder-list/) | 🟡 Medium | Find middle + reverse second half + merge |

---
---
