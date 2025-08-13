### ✅ 1. Problem Description

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it. Your task is to return the values of the nodes you can see from top to bottom.

This means for each level of the tree, you should report the value of the **rightmost** node.

-----

### ✅ 2. Sample Examples

1.  **Standard Example 1 (Complete levels):**

      * Input: `root = [1, 2, 3, null, 5, null, 4]`
        ```
              1  <---
             / \
            2   3  <---
             \   \
              5   4  <---
        ```
      * Output: `[1, 3, 4]`

2.  **Standard Example 2 (Left-leaning):**

      * Input: `root = [1, 2, 3, 4]`
        ```
              1  <---
             / \
            2   3  <---
           /
          4      <---
        ```
      * Output: `[1, 3, 4]`

3.  **Edge Case 1 (Empty Tree):**

      * Input: `root = []` (or `null`)
      * Output: `[]`

4.  **Edge Case 2 (Right-skewed Tree):**

      * Input: `root = [1, null, 2, null, 3]`
        ```
          1      <---
           \
            2    <---
             \
              3  <---
        ```
      * Output: `[1, 2, 3]`

5.  **Tricky Case (Rightmost node is a left child):**

      * Input: `root = [1, 2, 3, null, 4, 5]`
        ```
              1  <---
             / \
            2   3  <---
             \ /
             4 5   <---
        ```
      * Output: `[1, 3, 5]` (At the last level, node 5 is the rightmost, even though it's a left child).

-----

### ✅ 3. Constraints (if applicable)

  * The number of nodes in the tree is in the range `[0, 100]`.
  * Node values are in the range `[-100, 100]`.

-----

### 🔍 4. Topics and Patterns

  * **DSA Concepts:**
      * Binary Tree
      * Recursion
      * Stack (implicit via function call stack)
  * **Problem-Solving Pattern:**
      * **Depth-First Search (DFS):** This solution uses a recursive traversal. The key is to pass the current depth (or `level`) as a parameter to the recursive function and use it to determine which nodes are "visible".

-----

### ⚠️ 5. Edge Cases to Consider

1.  **Null Root:** The algorithm must handle an empty tree by returning an empty list without entering the recursion.
2.  **Traversal Order is Critical:** The logic relies on a specific traversal order: `Root -> Right -> Left`. If you traverse left first, you would get the *left* side view. This must be implemented correctly.
3.  **State Management:** The `result` list's size is used to track the deepest level visited so far. This state must be managed correctly across different recursive branches.

-----

### 📈 6. Time & Space Complexity

| Approach | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **DFS (Recursive)** | $O(N)$ | $O(H)$ |
| **BFS (Level Order Traversal)** | $O(N)$ | $O(W)$ |

  * $N$ is the total number of nodes in the tree.
  * $H$ is the **height** of the tree. The space is used by the recursion call stack.
  * $W$ is the maximum **width** of the tree. The DFS approach is generally more space-efficient than BFS for trees that are very wide and shallow.

-----

### 🧠 7. Key Idea and Thought Process

While BFS is a natural fit for level-by-level problems, we can cleverly adapt DFS to solve this. The core idea is to use the depth of the recursion to our advantage.

**1. The Core Insight**
The right view consists of the **first node we visit at each level**, provided we prioritize visiting the right side of the tree first.

**2. The DFS Strategy**
We will create a recursive helper function, say `solve(node, level, result)`.

1.  **Traversal Order:** To see the rightmost nodes first, we must traverse in a specific order:

    1.  Process the current `node`.
    2.  Recursively call on the `right` child.
    3.  Recursively call on the `left` child.

2.  **The "First Visit" Trick:** How do we know if we're visiting a level for the first time? We can use the size of our `result` list.

      * The `result` list stores the right view from top to bottom, so `result[0]` is level 0, `result[1]` is level 1, and so on.
      * When we are at a `node` at a given `level`, we check if `level == result.size()`.
      * If this condition is true, it means `result` has `level` elements (from index 0 to `level-1`), so we have not yet added an element for the current `level`.
      * Since our `Root -> Right -> Left` traversal ensures we visit the rightmost node at any new level first, this `node` must be the one we're looking for. We add it to `result`.
      * If `level < result.size()`, it means we've already added a node for this level (which would have been a node further to the right). So, we simply ignore the current node and continue the traversal.

**3. Putting it together:**

  * The main function initializes an empty `result` list and calls the recursive helper starting at the `root`, `level = 0`.
  * The helper function checks the base case (`node` is null).
  * It then applies the "first visit" trick.
  * Finally, it makes the two recursive calls, **right child first**, incrementing the level.

-----

### 📄 8. Pseudocode (All Approaches)

**DFS (Recursive) Pseudocode**

```pseudocode
function rightSideView(root):
    result = new List()
    // Start the recursion from the root at level 0
    solve(root, 0, result)
    return result

function solve(node, level, result):
    // Base case: If node is null, stop this path.
    if node is null:
        return
    
    // The core logic: If this level is being visited for the first time,
    // the current node is the rightmost one seen so far.
    if level == result.size():
        result.add(node.value)
        
    // IMPORTANT: Recurse on the right child first.
    solve(node.right, level + 1, result)
    solve(node.left, level + 1, result)
```

-----

### 💻 9. C++ Solution

```cpp
#include <iostream>
#include <vector>

// Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    std::vector<int> rightSideView(TreeNode* root) {
        std::vector<int> result;
        // Initial call to the recursive helper function.
        solve(root, 0, result);
        return result;
    }

private:
    void solve(TreeNode* node, int level, std::vector<int>& result) {
        // Base case: If we've reached a null child, stop.
        if (node == nullptr) {
            return;
        }

        // The key insight: if the current level is equal to the size of our result vector,
        // it means we are visiting this level for the very first time. Since we
        // traverse right-first, this node must be the rightmost one at this level.
        if (level == result.size()) {
            result.push_back(node->val);
        }

        // --- IMPORTANT ---
        // Recurse on the right subtree first to ensure we process the
        // right side of the tree before the left side at any given level.
        solve(node->right, level + 1, result);
        solve(node->left, level + 1, result);
    }
};
```

**Step-by-Step Execution Table**

Let's trace `solve(root, 0, result)` with the input tree: `[1, 2, 3, null, 5]`
Tree Structure:

```
      1
     / \
    2   3
     \
      5
```

Target Right View: `[1, 3, 5]`

| Function Call | `node` | `level` | `result.size()` | `level == result.size()`? | `result` | Explanation |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1. `solve(1, 0, [])` | 1 | 0 | 0 | Yes | `[1]` | `level` == `size`. Add 1. |
| 2. `solve(3, 1, [1])` | 3 | 1 | 1 | Yes | `[1, 3]` | Call `right`. `level` == `size`. Add 3. |
| 3. `solve(null, 2, [1, 3])`| null | 2 | - | - | `[1, 3]` | Call `right`. Base case. Return. |
| 4. `solve(null, 2, [1, 3])`| null | 2 | - | - | `[1, 3]` | Call `left`. Base case. Return. |
| - | 3 | - | - | - | `[1, 3]` | `solve(3)` finishes. Return to `solve(1)`. |
| 5. `solve(2, 1, [1, 3])` | 2 | 1 | 2 | No | `[1, 3]` | Call `left` from node 1. `level` \< `size`. Do nothing. |
| 6. `solve(null, 2, [1, 3])`| null | 2 | - | - | `[1, 3]` | Call `right` from node 2. Base case. Return. |
| 7. `solve(5, 2, [1, 3])` | 5 | 2 | 2 | Yes | `[1, 3, 5]` | Call `left` from node 2. `level` == `size`. Add 5. |
| 8. `solve(null, 3, ...)` | null | 3 | - | - | `[1, 3, 5]` | Calls `right` and `left` on node 5's null children. Return. |
| - | - | - | - | - | `[1, 3, 5]` | All calls finish. Final result is returned. |

-----

### 📊 10. Detailed Complexity Analysis

  * **Time Complexity: $O(N)$**

      * The recursive function `solve` is called for every node in the tree exactly once.
      * The operations inside the function (comparison, vector push\_back) are constant time, $O(1)$.
      * Therefore, the total time complexity is linear with respect to the number of nodes, $N$.

  * **Space Complexity: $O(H)$**

      * The space complexity is determined by the maximum depth of the recursion call stack.
      * In the **worst case**, the tree is skewed (a long chain of nodes), making its height $H = N$. The space complexity would be $O(N)$.
      * In the **best case** of a perfectly balanced tree, the height is $H = \\log N$, making the space complexity $O(\\log N)$.
      * This is often more space-efficient than the BFS approach (which is $O(W)$), especially for complete binary trees where the width $W$ can be up to $N/2$.

-----

### 📚 11. Similar Questions Table

| Question Title | Difficulty | Description | Why It’s Similar |
| :--- | :--- | :--- | :--- |
| **Binary Tree Left Side View** | Medium | Return the values of the leftmost nodes on each level. | Identical DFS logic, but you change the traversal order to `Root -> Left -> Right` to ensure the left nodes are visited first at each new level. |
| **Count Good Nodes in Binary Tree** | Medium | A node is "good" if no node on the path from the root to it has a greater value. | A classic DFS problem where you pass state down the recursion. Instead of `level`, you pass the `max_value_so_far` on the current path. |
| **Path Sum II** | Medium | Find all root-to-leaf paths that sum to a given target. | Solved with a recursive DFS. You pass the current path and current sum down the call stack, backtracking when you return from a call. |
| **Boundary of Binary Tree** | Medium | Find the boundary of a tree (left boundary, leaves, right boundary). | Requires multiple, specialized DFS traversals (one for the left boundary, one for leaves, one for the right boundary), showcasing how DFS can be adapted for complex traversal patterns. |
