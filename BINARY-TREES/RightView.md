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
      * Queue
      * Tree Traversal
  * **Problem-Solving Pattern:**
      * **Breadth-First Search (BFS) / Level Order Traversal:** This pattern is ideal for problems that require processing a tree level by level. The core idea is to use a queue to keep track of nodes to visit.

-----

### ⚠️ 5. Edge Cases to Consider

1.  **Null Root:** The input tree can be empty. The function must handle this gracefully by returning an empty list without attempting to access `root`.
2.  **Incomplete Levels:** The rightmost node at a given level is not necessarily a right child. It could be a left child whose sibling is missing. The algorithm must identify the last node processed at each level, regardless of its parentage.
3.  **Single Node Tree:** The tree might consist of only the root node. The loop should execute once for that level and correctly add the root's value to the result.

-----

### 📈 6. Time & Space Complexity

| Approach | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **BFS (Level Order Traversal)** | $O(N)$ | $O(W)$ |
| **DFS (Recursive)** | $O(N)$ | $O(H)$ |

  * $N$ is the total number of nodes in the tree.
  * $W$ is the maximum **width** of the tree (the maximum number of nodes at any single level). In the worst case (a complete binary tree), $W$ can be up to $N/2$, making the space complexity $O(N)$.
  * $H$ is the **height** of the tree. In the worst case (a skewed tree), $H$ is $N$, making the space complexity $O(N)$.

-----

### 🧠 7. Key Idea and Thought Process

The problem asks for the rightmost node of each level. This strongly suggests a **level-by-level** processing strategy, for which **Breadth-First Search (BFS)** is the perfect tool.

**1. The Core Idea**

The right side view is simply a list of the **last node** encountered at each level of the tree.

**2. How to implement with BFS?**

We can use a queue to perform a level order traversal. The main challenge is knowing when one level ends and the next one begins.

1.  **Initialization:** Create an empty `result` list and a `queue`. If the `root` is not null, add it to the `queue`.
2.  **Level-by-Level Loop:** Start a `while` loop that continues as long as the `queue` is not empty. Each iteration of this `while` loop will process exactly one level.
3.  **Process a Single Level:**
      * At the beginning of the `while` loop, capture the current `size` of the queue. This `level_size` tells you exactly how many nodes are on the current level.
      * Start an inner `for` loop that runs from `i = 0` to `level_size - 1`. This loop will dequeue all nodes of the current level and enqueue all nodes of the next level.
      * **Dequeue and Process:** Inside the `for` loop, dequeue a node (`currentNode`).
      * **Identify the Rightmost Node:** Check if this is the last node of the level. This condition is met when the `for` loop index `i` is equal to `level_size - 1`. If it is, this `currentNode` is the one we can "see" from the right. Add its value to our `result` list.
      * **Enqueue Children:** For the `currentNode` just dequeued, add its left and right children (if they exist) to the queue. These will be processed in a future iteration of the outer `while` loop.
4.  **Termination:** Once the queue is empty, all levels have been processed, and the `result` list contains the right side view.

**Alternative Approach: DFS (Depth-First Search)**
While BFS is more intuitive, DFS can also solve this.

1.  Define a recursive function `dfs(node, level)`.
2.  Traverse in a `Root -> Right -> Left` order.
3.  The key trick is to compare the current `level` with the `size` of the `result` list. If `level == result.size()`, it means we are visiting this level for the very first time. Since we are exploring the right subtree first, the first node we encounter at a new level is guaranteed to be the rightmost one. We add it to the `result` list.

-----

### 📄 8. Pseudocode (All Approaches)

**1. BFS (Level Order Traversal) Pseudocode**

```pseudocode
function rightSideView_BFS(root):
    if root is null:
        return an empty List
    
    result = new List()
    queue = new Queue()
    queue.enqueue(root)
    
    while queue is not empty:
        level_size = queue.size()
        
        for i from 0 to level_size - 1:
            currentNode = queue.dequeue()
            
            // If this is the last node of the current level, add it to result
            if i == level_size - 1:
                result.add(currentNode.value)
            
            // Add children to the queue for the next level
            if currentNode.left is not null:
                queue.enqueue(currentNode.left)
            if currentNode.right is not null:
                queue.enqueue(currentNode.right)
                
    return result
```

**2. DFS (Recursive) Pseudocode**

```pseudocode
function rightSideView_DFS(root):
    result = new List()
    helper(root, 0, result)
    return result

function helper(node, level, result):
    if node is null:
        return
    
    // If we are visiting this level for the first time,
    // the current node is the rightmost one seen so far.
    if level == result.size():
        result.add(node.value)
    
    // Recurse on the right subtree first to ensure we see the rightmost nodes first.
    helper(node.right, level + 1, result)
    helper(node.left, level + 1, result)
```

-----

### 💻 9. C++ Solution

```cpp
#include <iostream>
#include <vector>
#include <queue>

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
        if (root == nullptr) {
            return result;
        }

        std::queue<TreeNode*> q;
        q.push(root);

        // This loop processes the tree level by level.
        while (!q.empty()) {
            // Get the number of nodes at the current level.
            int level_size = q.size();

            // Process all nodes of the current level.
            for (int i = 0; i < level_size; ++i) {
                TreeNode* currentNode = q.front();
                q.pop();

                // If this is the last node of the level (i.e., the rightmost one),
                // add its value to our result list.
                if (i == level_size - 1) {
                    result.push_back(currentNode->val);
                }

                // Add children to the queue for the next level.
                if (currentNode->left != nullptr) {
                    q.push(currentNode->left);
                }
                if (currentNode->right != nullptr) {
                    q.push(currentNode->right);
                }
            }
        }
        return result;
    }
};

```

**Step-by-Step Execution Table**

Let's trace the algorithm with the input tree: `root = [1, 2, 3, null, 5, null, 4]`

| Step | `queue` (front -\> back) | `level_size` | `i` | `currentNode` | `result` | Explanation |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | `[1]` | - | - | - | `[]` | Initial state: push root. |
| 2 | `[1]` | 1 | - | - | `[]` | Start `while`. `level_size` = 1. |
| 3 | `[]` | 1 | 0 | `1` | `[]` | Dequeue 1. `i` (0) == `level_size-1` (0). |
| 4 | `[2, 3]` | 1 | 0 | `1` | `[1]` | Add 1 to `result`. Enqueue children 2 and 3. |
| 5 | `[2, 3]` | 2 | - | - | `[1]` | Start next level. `level_size` = 2. |
| 6 | `[3]` | 2 | 0 | `2` | `[1]` | Dequeue 2. `i` (0) \!= `level_size-1` (1). Enqueue child 5. |
| 7 | `[3, 5]` | 2 | 0 | `2` | `[1]` | Queue is now `[3, 5]`. |
| 8 | `[5]` | 2 | 1 | `3` | `[1]` | Dequeue 3. `i` (1) == `level_size-1` (1). |
| 9 | `[5, 4]` | 2 | 1 | `3` | `[1, 3]` | Add 3 to `result`. Enqueue child 4. |
| 10 | `[5, 4]` | 2 | - | - | `[1, 3]` | Start next level. `level_size` = 2. |
| 11 | `[4]` | 2 | 0 | `5` | `[1, 3]` | Dequeue 5. `i` (0) \!= `level_size-1` (1). No children. |
| 12 | `[]` | 2 | 1 | `4` | `[1, 3]` | Dequeue 4. `i` (1) == `level_size-1` (1). |
| 13 | `[]` | 2 | 1 | `4` | `[1, 3, 4]` | Add 4 to `result`. No children. |
| 14 | `[]` | - | - | - | `[1, 3, 4]` | Queue is empty. `while` loop terminates. Return `result`. |

-----

### 📊 10. Detailed Complexity Analysis

  * **Time Complexity: $O(N)$**

      * The algorithm is a Breadth-First Search, which visits every node and every edge exactly once.
      * The outer `while` loop runs once for each level, and the inner `for` loop runs once for each node in that level. Summing this up over all levels means each of the $N$ nodes is processed (enqueued and dequeued) exactly once.
      * Therefore, the time complexity is linear in the number of nodes.

  * **Space Complexity: $O(W)$**

      * The space complexity is determined by the maximum number of nodes stored in the `queue` at any given time.
      * The queue holds nodes for at most two consecutive levels, but its maximum size is dictated by the widest level in the tree. Let $W$ be the maximum width.
      * In the worst-case scenario for space, you have a **complete binary tree**, where the last level contains approximately $N/2$ nodes. In this case, $W$ is proportional to $N$, making the space complexity $O(N)$.
      * In the best-case scenario (a skewed tree), the width is always 1, making the space complexity $O(1)$.

-----

### 📚 11. Similar Questions Table

| Question Title | Difficulty | Description | Why It’s Similar |
| :--- | :--- | :--- | :--- |
| **Binary Tree Left Side View** | Medium | Return the values of the leftmost nodes on each level. | Identical BFS logic. Instead of picking the last node (`i == level_size - 1`), you pick the first node (`i == 0`). |
| **Binary Tree Level Order Traversal** | Medium | Return all nodes of the tree, level by level. | This is the foundational algorithm. Right Side View is a specific application of it where you only store one node per level. |
| **Find Bottom Left Tree Value** | Medium | Find the value of the leftmost node on the last level of the tree. | Solved using BFS. You traverse level by level and the first node of the very last level processed is the answer. |
| **Maximum Level Sum in a Binary Tree** | Medium | Find the level in a binary tree that has the maximum sum of its node values. | Also requires a level-by-level processing using BFS. For each level, you sum the node values and keep track of the maximum sum found. |
