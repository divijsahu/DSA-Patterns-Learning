# Pattern 29 — Linked List Manipulation

**Category:** Linked List · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Reverse** a linked list (fully or partially)
- **Merge** two sorted linked lists
- Remove **Nth node from end**
- Find **intersection** of two linked lists
- Sort a linked list
- Problem says: *"reverse linked list"*, *"merge"*, *"nth from end"*, *"rotate"*, *"flatten"*

---

## 🧠 Intuition in Plain English

Linked list problems require careful pointer choreography. The key is to always sketch the pointer state before and after the operation. Reversing requires three pointers (prev, curr, next). Merging uses the classic two-pointer merge from merge sort. Most linked list problems are about pointer re-wiring, not data — visualize the arrows, not the values.

---

## 📐 Core Template

```python
# ─── REVERSE LINKED LIST ──────────────────────────────────────────────────────
def reverse_list(head):
    prev, curr = None, head

    while curr:
        next_node = curr.next          # Save next before overwriting
        curr.next = prev               # Reverse the pointer
        prev = curr                    # Move prev forward
        curr = next_node               # Move curr forward

    return prev                        # New head is the old tail


# ─── REVERSE IN K-GROUPS ──────────────────────────────────────────────────────
def reverse_k_group(head, k):
    # Check if there are k nodes left
    count, node = 0, head
    while node and count < k:
        node = node.next
        count += 1
    if count < k:
        return head                    # Fewer than k nodes: don't reverse

    # Reverse k nodes
    prev, curr = None, head
    for _ in range(k):
        next_node = curr.next
        curr.next = prev
        prev = curr
        curr = next_node

    # head is now the tail of reversed group
    head.next = reverse_k_group(curr, k)   # Recursively handle rest
    return prev                            # Return new head of this group


# ─── MERGE TWO SORTED LISTS ───────────────────────────────────────────────────
def merge_two_sorted(l1, l2):
    dummy = ListNode(0)
    curr = dummy

    while l1 and l2:
        if l1.val <= l2.val:
            curr.next = l1
            l1 = l1.next
        else:
            curr.next = l2
            l2 = l2.next
        curr = curr.next

    curr.next = l1 or l2               # Attach remaining nodes
    return dummy.next


# ─── NTH NODE FROM END ────────────────────────────────────────────────────────
def remove_nth_from_end(head, n):
    dummy = ListNode(0)
    dummy.next = head
    fast = slow = dummy

    # Move fast n+1 steps ahead
    for _ in range(n + 1):
        fast = fast.next

    # Move both until fast reaches end
    while fast:
        fast = fast.next
        slow = slow.next

    slow.next = slow.next.next         # Remove the nth node
    return dummy.next
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Operation |
|---|---------|------------|-----------|
| 1 | [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) | 🟢 Easy | Basic reversal |
| 2 | [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) | 🟢 Easy | Two-pointer merge |
| 3 | [Remove Nth Node From End](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) | 🟡 Medium | Two pointers n apart |
| 4 | [Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/) | 🟡 Medium | Partial reversal with reconnection |
| 5 | [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/) | 🟡 Medium | Hash map or interleave clones |
| 6 | [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/) | 🟡 Medium | Digit-by-digit with carry |
| 7 | [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/) | 🔴 Hard | Reverse groups recursively |
| 8 | [Sort List](https://leetcode.com/problems/sort-list/) | 🟡 Medium | Merge sort on linked list |

---
---
