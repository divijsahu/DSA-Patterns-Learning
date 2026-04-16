# Pattern 27 — Trie (Prefix Tree)

**Category:** String/Tree · **Difficulty Band:** Medium–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Prefix-based** operations: autocomplete, word suggestions
- Search for words with **wildcards** (`.` or `*`)
- **Dictionary** operations: insert, search, starts-with
- Finding **longest common prefix** among strings
- Efficient storage of many strings with shared prefixes
- Problem says: *"prefix"*, *"starts with"*, *"autocomplete"*, *"word search"*, *"add and search"*

---

## 🧠 Intuition in Plain English

A Trie is a tree where each path from root to a node spells a word (or prefix). The root is empty, and each edge represents one character. All words starting with "ca" share the path c → a from the root. This structure makes prefix lookup O(L) where L is the length of the prefix — no matter how many words are in the dictionary. Compare to a hash set: O(L) lookup but no support for prefix queries.

---

## 📐 Core Template

```python
class TrieNode:
    def __init__(self):
        self.children = {}             # char → TrieNode
        self.is_end = False            # True if a complete word ends here
        # Optional: store the word itself at the node for backtracking problems
        self.word = None


class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True
        node.word = word               # Store for backtracking retrieval

    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end             # Must end a complete word

    def starts_with(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True                    # Prefix exists in the trie

    def search_with_wildcard(self, word):
        """Supports '.' as wildcard matching any single character"""
        def dfs(node, i):
            if i == len(word):
                return node.is_end
            char = word[i]
            if char == '.':
                return any(dfs(child, i + 1) for child in node.children.values())
            if char not in node.children:
                return False
            return dfs(node.children[char], i + 1)

        return dfs(self.root, 0)
```

---

## 📝 Problem Set

| # | Problem | Difficulty | Trie Operation |
|---|---------|------------|----------------|
| 1 | [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/) | 🟡 Medium | Build the data structure |
| 2 | [Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/) | 🟡 Medium | Trie + DFS for top 3 suggestions |
| 3 | [Design Add and Search Words](https://leetcode.com/problems/design-add-and-search-words-data-structure/) | 🟡 Medium | Wildcard '.' with DFS |
| 4 | [Replace Words](https://leetcode.com/problems/replace-words/) | 🟡 Medium | Find shortest root prefix |
| 5 | [Palindrome Pairs](https://leetcode.com/problems/palindrome-pairs/) | 🔴 Hard | Reverse words in trie |
| 6 | [Word Search II](https://leetcode.com/problems/word-search-ii/) | 🔴 Hard | Trie + backtracking on grid |
| 7 | [Maximum XOR of Two Numbers](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/) | 🟡 Medium | Binary trie (bit-by-bit) |

---
---
