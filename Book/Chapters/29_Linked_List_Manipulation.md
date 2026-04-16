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

```cpp
#include <bits/stdc++.h>
using namespace std;

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

ListNode* reverse_list(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    while (curr) {
        ListNode* next_node = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next_node;
    }
    return prev;
}

ListNode* reverse_k_group(ListNode* head, int k) {
    ListNode* node = head;
    int count = 0;
    while (node && count < k) {
        node = node->next;
        ++count;
    }
    if (count < k) return head;

    ListNode* prev = nullptr;
    ListNode* curr = head;
    for (int i = 0; i < k; ++i) {
        ListNode* next_node = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next_node;
    }
    head->next = reverse_k_group(curr, k);
    return prev;
}

ListNode* merge_two_sorted(ListNode* l1, ListNode* l2) {
    ListNode dummy(0);
    ListNode* curr = &dummy;
    while (l1 && l2) {
        if (l1->val <= l2->val) {
            curr->next = l1;
            l1 = l1->next;
        } else {
            curr->next = l2;
            l2 = l2->next;
        }
        curr = curr->next;
    }
    curr->next = l1 ? l1 : l2;
    return dummy.next;
}

ListNode* remove_nth_from_end(ListNode* head, int n) {
    ListNode dummy(0);
    dummy.next = head;
    ListNode* fast = &dummy;
    ListNode* slow = &dummy;
    for (int i = 0; i < n + 1; ++i) fast = fast->next;
    while (fast) {
        fast = fast->next;
        slow = slow->next;
    }
    slow->next = slow->next->next;
    return dummy.next;
}
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
