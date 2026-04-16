# Pattern 13 — Tree DFS Patterns

**Category:** Tree · **Difficulty Band:** Easy–Hard

---

## 🎯 Pattern Fingerprint

**Spot this pattern when you see:**
- **Path problems** in trees (path sum, path with max value)
- **Depth / height** calculations
- **Diameter, LCA** (Lowest Common Ancestor)
- Comparing left and right subtrees
- Serializing or reconstructing trees
- Problem says: *"path sum"*, *"height"*, *"depth"*, *"diameter"*, *"symmetric"*, *"lowest common ancestor"*, *"balanced"*

---

## 🧠 Intuition in Plain English

Trees are recursion's natural habitat. Every tree problem can be reduced to: "Given what my left child tells me and what my right child tells me, what do I tell my parent?" This post-order thinking (children first, then parent) solves the vast majority of tree problems. The key discipline: **identify what each recursive call should return**, and the code writes itself.

---

## 🔮 Pattern Prediction Checklist

- [ ] Does the answer depend on **combining** left and right subtree results? → Tree DFS (post-order)
- [ ] Do we need to pass information **downward** from parent to children? → Tree DFS (pre-order)
- [ ] Is there a **global maximum** (like diameter) that needs updating at each node? → Use a nonlocal/instance variable

**The 3 Tree DFS Orders:**
- **Pre-order** (root → left → right): Serialize, build, top-down path problems
- **In-order** (left → root → right): BST problems (gives sorted sequence)
- **Post-order** (left → right → root): Height, diameter, bottom-up aggregation

---

## 📐 Core Template

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int diameterOfBinaryTree(TreeNode* root) {
    int best = 0;
    function<int(TreeNode*)> height = [&](TreeNode* node) {
        if (!node) return 0;
        int left_h = height(node->left);
        int right_h = height(node->right);
        best = max(best, left_h + right_h);
        return 1 + max(left_h, right_h);
    };
    height(root);
    return best;
}

TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root || root == p || root == q) return root;
    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    TreeNode* right = lowestCommonAncestor(root->right, p, q);
    if (left && right) return root;
    return left ? left : right;
}

bool hasPathSum(TreeNode* root, int targetSum) {
    if (!root) return false;
    if (!root->left && !root->right) return root->val == targetSum;
    return hasPathSum(root->left, targetSum - root->val) || hasPathSum(root->right, targetSum - root->val);
}

int maxPathSum(TreeNode* root) {
    int best = INT_MIN;
    function<int(TreeNode*)> gain = [&](TreeNode* node) {
        if (!node) return 0;
        int left_gain = max(gain(node->left), 0);
        int right_gain = max(gain(node->right), 0);
        best = max(best, node->val + left_gain + right_gain);
        return node->val + max(left_gain, right_gain);
    };
    gain(root);
    return best;
}
```

---

## 🔀 Variants by Return Type

| What to return from DFS call | Example Problems |
|------------------------------|-----------------|
| **Height (int)** | Max depth, diameter, balanced check |
| **Boolean** | Path sum exists, symmetric tree |
| **Node** | LCA, search in BST |
| **(max, min, both) tuple** | Camera coverage, path problems |

---

## ⏱️ Complexity

| Operation | Time | Space |
|-----------|------|-------|
| All tree DFS | O(n) | O(h) — h = tree height, O(n) worst case for skewed trees |

---

## ❌ Anti-patterns & Traps

- **Forgetting the null base case:** Always handle `if not node: return <base_value>` first.
- **Mutating a global max:** In Python, use a list `[value]` or instance variable, not a plain integer (can't assign to outer scope variables directly).
- **Confusing "path through node" vs "path to parent":** In diameter/max-path-sum, the global answer might use both left and right, but what you return to the parent is only the better single side.

---

## 📝 Problem Set

| # | Problem | Difficulty | DFS Return Value |
|---|---------|------------|-----------------|
| 1 | [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) | 🟢 Easy | Height (int) |
| 2 | [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/) | 🟢 Easy | Height or -1 if unbalanced |
| 3 | [Path Sum](https://leetcode.com/problems/path-sum/) | 🟢 Easy | Boolean |
| 4 | [Symmetric Tree](https://leetcode.com/problems/symmetric-tree/) | 🟢 Easy | Recursive mirror check |
| 5 | [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/) | 🟢 Easy | Height + global max update |
| 6 | [Lowest Common Ancestor of a BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) | 🟡 Medium | Use BST ordering |
| 7 | [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/) | 🟡 Medium | Last node at each BFS level |
| 8 | [Path Sum II](https://leetcode.com/problems/path-sum-ii/) | 🟡 Medium | Collect all root-to-leaf paths |
| 9 | [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) | 🔴 Hard | Gain function + global max |
| 10 | [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) | 🔴 Hard | Pre-order DFS with null markers |

---
---
