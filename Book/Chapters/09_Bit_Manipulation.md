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

```python
# ─── ESSENTIAL BIT TRICKS ────────────────────────────────────────────────────
x & (x - 1)      # Clears the lowest set bit of x (used to count bits)
x & (-x)         # Isolates the lowest set bit of x
x ^ x            # Always 0 (cancellation)
x ^ 0            # Always x (identity)
x >> 1           # Divide by 2
x << 1           # Multiply by 2
x & 1            # Check if x is odd (last bit)
~x               # Bitwise NOT

# ─── SINGLE NUMBER (XOR cancellation) ────────────────────────────────────────
def single_number(nums):
    result = 0
    for num in nums:
        result ^= num              # All duplicates cancel, unique remains
    return result

# ─── COUNT SET BITS (Hamming Weight) ─────────────────────────────────────────
def count_bits(n):
    count = 0
    while n:
        n &= n - 1                 # Each iteration removes one set bit
        count += 1
    return count

# ─── POWER OF TWO CHECK ───────────────────────────────────────────────────────
def is_power_of_two(n):
    return n > 0 and (n & (n - 1)) == 0   # Powers of 2 have exactly one set bit

# ─── MISSING NUMBER ───────────────────────────────────────────────────────────
def missing_number(nums):
    n = len(nums)
    result = n                     # Start with n
    for i, num in enumerate(nums):
        result ^= i ^ num          # XOR with both index and value
    return result                  # Remaining = missing number
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
