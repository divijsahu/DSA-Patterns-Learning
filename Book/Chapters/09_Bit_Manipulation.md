# Pattern 9 — Bit Manipulation

**Category:** Math · **Difficulty Band:** Easy–Medium

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- Find a **single number** in an array where others appear twice/three times
- **Toggle, set, or clear** specific bits
- Check if a number is a **power of 2**
- Count the number of **1s in binary representation**
- Problem says: *"XOR"*, *"single number"*, *"bit"*, *"power of two"*, *"count bits"*, *"missing number"*

---

## 🧠 Intuition in Plain English

Bits are nature's toggle switches — a 0 or a 1. XOR is the magician of bit operations: `a XOR a = 0` (same thing cancels itself) and `a XOR 0 = a` (zero doesn't change anything). So XOR-ing a list of numbers where every number appears twice leaves only the unique one. It's the algorithmic equivalent of every person pairing up and sitting down, leaving the lonely person standing.

---

## 📐 Core Bit Operations

```cpp
#include <bits/stdc++.h>
using namespace std;

int singleNumber(const vector<int>& nums) {
    int x = 0;
    for (int num : nums) x ^= num;
    return x;
}

bool isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}

vector<int> countBits(int n) {
    vector<int> dp(n + 1, 0);
    for (int i = 1; i <= n; ++i) {
        dp[i] = dp[i >> 1] + (i & 1);
    }
    return dp;
}
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Bit Trick |
|---|---------|------------|-----------|
| 1 | [Single Number](https://leetcode.com/problems/single-number/) | 🟢 Easy | XOR all elements |
| 2 | [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/) | 🟢 Easy | `n & (n-1)` loop |
| 3 | [Power of Two](https://leetcode.com/problems/power-of-two/) | 🟢 Easy | `n & (n-1) == 0` |
| 4 | [Missing Number](https://leetcode.com/problems/missing-number/) | 🟢 Easy | XOR with indices |
| 5 | [Counting Bits](https://leetcode.com/problems/counting-bits/) | 🟢 Easy | DP with bit trick |
| 6 | [Single Number II](https://leetcode.com/problems/single-number-ii/) | 🟡 Medium | 3-state bit counter |
| 7 | [Reverse Bits](https://leetcode.com/problems/reverse-bits/) | 🟢 Easy | Shift and mask 32 times |

---
---
