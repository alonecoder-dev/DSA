# ✅ 1. Problem Description

Given the root of a binary tree, return the **top view** of the tree — the set of nodes visible from directly above.

Order the nodes from **left to right** based on their horizontal position.

If multiple nodes are at the same horizontal distance (HD), only the one closer to the root is visible.

---

# ✅ 2. Sample Examples

### **Example 1 – Full Tree**

**Input:**

```
root = [1, 2, 3, 4, 5, 6, 7]
      1
     / \\
    2   3
   / \\ / \\
  4  5 6  7
// HD: 4 → -2, 2 → -1, 1 → 0, 3 → 1, 7 → 2

```

**Output:**

```
[4, 2, 1, 3, 7]

```

---

### **Example 2 – Nodes Hidden**

**Input:**

```
root = [1, 2, 3, null, 4, 5, 6, null, null, 7]
      1
     / \\
    2   3
     \\ / \\
      4 5  6
       /
      7
// Node 4 (HD=0) hidden by 1 (HD=0)
// Node 5 (HD=0) hidden by 1 (HD=0)

```

**Output:**

```
[2, 1, 3, 6]

```

---

### **Edge Case 1 – Empty Tree**

**Input:**

```
[]

```

**Output:**

```
[]

```

---

### **Edge Case 2 – Skewed Tree**

**Input:**

```
root = [1, 2, null, 3, null, 4]
      1
     /
    2
   /
  3
 /
4

```

**Output:**

```
[4, 3, 2, 1]

```

---

### **Tricky Case – Top Node at Lower Level**

**Input:**

```
root = [1, 2, 3, 4, 5, 6, null]
      1
     / \\
    2   3
   / \\ /
  4  5 6
// HD(4) = -2, HD(2) = -1, HD(1) = 0
// HD(6) = 0 but hidden by 1

```

**Output:**

```
[4, 2, 1, 3]

```

---

# ✅ 3. Constraints

- **0 ≤ nodes ≤ 2000**
- **0 ≤ node value ≤ 1000**

---

# 🔍 4. Topics & Patterns

**DSA Concepts:**

- Binary Tree
- Queue
- Hash Table / `std::map` (C++)

**Pattern Used:**

- **BFS with Coordinate Tracking**
- Store `(node, HD)` in queue
- Use a map to keep the first node for each HD

---

# ⚠️ 5. Edge Cases to Consider

- Multiple nodes at same HD → keep only the first (topmost)
- Negative HD for left side nodes
- Output sorted by HD from **min → max**

---

# 📈 6. Time & Space Complexity

| Approach | Time Complexity | Space Complexity |
| --- | --- | --- |
| BFS + Map | O(N log HD) | O(N) |

---

# 🧠 7. Key Idea & Thought Process

- Assign HD = 0 for root
- Left child → HD - 1
- Right child → HD + 1
- BFS ensures first node at HD is topmost
- Map ensures sorted order of HD

---

# 📄 8. Pseudocode

```
function topView(root):
    if root == null:
        return []
    view_map = Map()
    queue = Queue()
    queue.enqueue({root, 0})

    while queue not empty:
        node, hd = queue.dequeue()
        if hd not in view_map:
            view_map[hd] = node.value
        if node.left:
            queue.enqueue({node.left, hd - 1})
        if node.right:
            queue.enqueue({node.right, hd + 1})

    return values in view_map sorted by hd

```

---

# 💻 9. C++ Solution

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <map>
#include <utility>

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    std::vector<int> topView(TreeNode* root) {
        std::vector<int> result;
        if (!root) return result;

        std::map<int, int> view_map;
        std::queue<std::pair<TreeNode*, int>> q;
        q.push({root, 0});

        while (!q.empty()) {
            auto [node, hd] = q.front();
            q.pop();

            if (view_map.find(hd) == view_map.end()) {
                view_map[hd] = node->val;
            }

            if (node->left) q.push({node->left, hd - 1});
            if (node->right) q.push({node->right, hd + 1});
        }

        for (auto &[hd, val] : view_map) {
            result.push_back(val);
        }
        return result;
    }
};

```

---

# 🧾 Step-by-Step Execution Example

Tree: `root = [1, 2, 3, null, 4]`

| Step | Queue | Node-HD | Map | Action |
| --- | --- | --- | --- | --- |
| 1 | {1,0} | - | {} | Start |
| 2 | {} | 1,0 | {0:1} | Add 1 |
| 3 | {2,-1}, {3,1} | - | {0:1} | Enq L,R |
| 4 | {3,1}, {4,0} | 2,-1 | {-1:2, 0:1} | Add 2 |
| 5 | {4,0} | 3,1 | {-1:2, 0:1, 1:3} | Add 3 |
| 6 | {} | 4,0 | no change | HD=0 seen |

Final result: `[2, 1, 3]`

---

# 📊 10. Detailed Complexity

- **Time:** `O(N log HD)`
- **Space:** `O(N)` (queue + map)

---

# 📚 11. Similar Questions

| Question | Difficulty | Similarity |
| --- | --- | --- |
| Bottom View of Binary Tree | Medium | Same idea but store last node per HD |
| Vertical Order Traversal | Hard | Store multiple nodes per HD |
| Left/Right Side View | Medium | BFS but track levels instead of HD |
