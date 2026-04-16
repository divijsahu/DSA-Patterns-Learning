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

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TrieNode {
    unordered_map<char, TrieNode*> children;
    bool is_end = false;
    string word;
};

class Trie {
public:
    TrieNode* root;

    Trie() : root(new TrieNode()) {}

    void insert(const string& word) {
        TrieNode* node = root;
        for (char ch : word) {
            if (!node->children.count(ch)) node->children[ch] = new TrieNode();
            node = node->children[ch];
        }
        node->is_end = true;
        node->word = word;
    }

    bool search(const string& word) {
        TrieNode* node = root;
        for (char ch : word) {
            if (!node->children.count(ch)) return false;
            node = node->children[ch];
        }
        return node->is_end;
    }

    bool starts_with(const string& prefix) {
        TrieNode* node = root;
        for (char ch : prefix) {
            if (!node->children.count(ch)) return false;
            node = node->children[ch];
        }
        return true;
    }

    bool search_with_wildcard(const string& word) {
        function<bool(TrieNode*, int)> dfs = [&](TrieNode* node, int i) {
            if (i == (int)word.size()) return node->is_end;
            char ch = word[i];
            if (ch == '.') {
                for (auto& entry : node->children) {
                    if (dfs(entry.second, i + 1)) return true;
                }
                return false;
            }
            if (!node->children.count(ch)) return false;
            return dfs(node->children[ch], i + 1);
        };
        return dfs(root, 0);
    }
};
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
