✅ 1. Problem Description
Given the root of a binary tree, your task is to return the top view of the tree. The top view is the set of nodes visible when the tree is viewed from directly above. The nodes should be ordered from left to right based on their horizontal position.
If multiple nodes are at the same horizontal distance, only the one that is closer to the root (i.e., at a higher level) is visible.
✅ 2. Sample Examples
 * Standard Example 1 (Full Tree):
   * Input: root = [1, 2, 3, 4, 5, 6, 7]
           1
     / \
    2   3
   / \ / \
  4  5 6  7
// Horizontal Distances:
// 4: -2, 2: -1, 1: 0, 3: 1, 7: 2

   * Output: [4, 2, 1, 3, 7]
 * Standard Example 2 (Nodes are hidden):
   * Input: root = [1, 2, 3, null, 4, 5, 6, null, null, 7]
           1
     / \
    2   3
     \ / \
      4 5  6
       /
      7
// Node 4 (HD=0) is hidden by Node 1 (HD=0)
// Node 5 (HD=0) is hidden by Node 1 (HD=0)

   * Output: [2, 1, 3, 6]
 * Edge Case 1 (Empty Tree):
   * Input: root = []
   * Output: []
 * Edge Case 2 (Skewed Tree):
   * Input: root = [1, 2, null, 3, null, 4]
       1
 /
2
/
3

   /
   4
   ```
   * Output: [4, 3, 2, 1]
 * Tricky Case (Top node is at a lower level):
   * Input: root = [1, 2, 3, 4, 5, 6, null]
           1
     / \
    2   3
   / \ /
  4  5 6
// HD of 4 is -2. HD of 2 is -1. HD of 1 is 0.
// HD of 6 is 0, but it's hidden by 1. HD of 3 is 1.

   * Output: [4, 2, 1, 3] (Node 6 at HD=0 is hidden by Node 1).
✅ 3. Constraints (if applicable)
 * The number of nodes in the tree is in the range [0, 2000].
 * Node values are in the range [0, 1000].
🔍 4. Topics and Patterns
 * DSA Concepts:
   * Binary Tree
   * Queue
   * Hash Table (specifically std::map in C++ for sorted keys)
 * Problem-Solving Pattern:
   * Level Order Traversal (BFS) with Coordinate Tracking: This problem is a variation of BFS. Instead of just storing nodes in the queue, we store pairs of (node, coordinate) to keep track of their horizontal position. A map is used to ensure we only record the first node encountered at any given horizontal distance.
⚠️ 5. Edge Cases to Consider
 * Multiple nodes at the same horizontal distance: The core of the problem. A node at level L will always obscure a node at level L+1 if they share the same horizontal coordinate. A top-down BFS traversal naturally handles this by design.
 * Handling Horizontal Coordinates: The horizontal distance can be negative (for nodes in the left subtree). The data structure used to store the view must support negative keys. std::map in C++ handles this perfectly.
 * Output Order: The final result must be ordered by horizontal distance from left to right (i.e., from min HD to max HD). Using a std::map automatically sorts the nodes by their horizontal distance (key), simplifying the final step.
📈 6. Time & Space Complexity
| Approach | Time Complexity | Space Complexity |
|---|---|---|
| BFS + Map | O(N \\log HD) | O(N) |
 * N is the total number of nodes in the tree.
 * HD is the number of unique horizontal distances in the tree.
 * Time: The O(N) factor comes from visiting each node once. The O(\\log HD) factor is due to the insertion time for std::map. If an unordered_map were used, the average time complexity would be O(N), but it would require an extra O(HD \\log HD) step to sort the keys for the final output.
 * Space: O(N) in the worst case. The queue can hold up to W (max width) nodes, and the map can hold up to HD entries. In a skewed tree, both can be proportional to N.
🧠 7. Key Idea and Thought Process
The problem requires us to find nodes visible from the top, ordered horizontally. This hints at two key pieces of information we need for each node: its value and its horizontal position.
1. Defining Horizontal Distance (HD)
Let's establish a coordinate system:
 * The root is at horizontal distance (HD) 0.
 * For any node at HD x, its left child is at x - 1.
 * For any node at HD x, its right child is at x + 1.
2. The Core Insight
The top view consists of the first node we encounter at each unique horizontal distance when we traverse the tree from the top down.
3. Why is BFS the right tool?
Breadth-First Search (BFS), or Level Order Traversal, explores the tree level by level. This is perfect because it guarantees that we will visit a node at level L before we visit any node at level L+1. Therefore, the first time we see a node at a specific HD, we are certain it's the highest one, and thus it belongs in the top view.
4. The Algorithm using BFS:
 * Data Structures:
   * A queue to manage the BFS traversal. Since we need to track HD, the queue will store pairs: {TreeNode*, horizontal_distance}.
   * A map<int, int> to store our result. The map's key will be the horizontal_distance, and its value will be the node's value. The map is crucial for two reasons:
     * It allows us to check in O(\\log HD) time if we've already found a node for a given HD.
     * It automatically keeps the entries sorted by HD, which makes constructing the final output trivial.
 * Initialization:
   * If root is null, return an empty list.
   * Create the queue and the map.
   * Push the first element {root, 0} into the queue.
 * Traversal Loop:
   * While the queue is not empty:
     * Dequeue the pair: auto [currentNode, hd] = queue.front().
     * Check for Visibility: See if hd is already a key in our map.
       * if (map.find(hd) == map.end()): This means we have not seen any node at this horizontal distance before. Since this is a BFS, currentNode must be the topmost one.
       * Record it: map[hd] = currentNode->val.
     * Enqueue Children:
       * If currentNode->left exists, push {currentNode->left, hd - 1} into the queue.
       * If currentNode->right exists, push {currentNode->right, hd + 1} into the queue.
 * Final Output:
   * The loop terminates when the queue is empty.
   * The map now holds the complete top view, sorted by HD.
   * Iterate through the map and add each value to a result vector. Return result.
📄 8. Pseudocode (All Approaches)
BFS + Map Pseudocode
function topView(root):
    if root is null:
        return an empty List
    
    result = new List()
    // A map to store the first node found at each horizontal distance
    // Key: Horizontal Distance, Value: Node's Value
    view_map = new Map() 
    
    // A queue to hold pairs of {Node, Horizontal Distance}
    queue = new Queue()
    queue.enqueue({root, 0})
    
    while queue is not empty:
        pair = queue.dequeue()
        node = pair.node
        hd = pair.hd
        
        // If this horizontal distance is not yet in our map, it's a new
        // top-level node. Add it.
        if view_map.doesNotContain(hd):
            view_map.put(hd, node.value)
        
        // Enqueue left and right children with updated horizontal distances
        if node.left is not null:
            queue.enqueue({node.left, hd - 1})
        if node.right is not null:
            queue.enqueue({node.right, hd + 1})
            
    // Extract values from the sorted map to form the final result
    for each key-value in view_map (sorted by key):
        result.add(value)
        
    return result

💻 9. C++ Solution
#include <iostream>
#include <vector>
#include <queue>
#include <map>
#include <utility> // For std::pair

// Definition for a binary tree node.
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
        if (root == nullptr) {
            return result;
        }

        // Map to store the top view nodes. Key: horizontal distance, Value: node's value.
        // std::map keeps keys sorted, which is perfect for this problem.
        std::map<int, int> view_map;
        
        // Queue for level order traversal. Stores pairs of {node, horizontal_distance}.
        std::queue<std::pair<TreeNode*, int>> q;

        // Start with the root at horizontal distance 0.
        q.push({root, 0});

        while (!q.empty()) {
            // Get the node and its horizontal distance from the front of the queue.
            std::pair<TreeNode*, int> current_pair = q.front();
            q.pop();
            TreeNode* node = current_pair.first;
            int hd = current_pair.second;

            // If we haven't recorded a node for this horizontal distance yet,
            // this must be the topmost one. Record it.
            if (view_map.find(hd) == view_map.end()) {
                view_map[hd] = node->val;
            }

            // Enqueue left child with a horizontal distance of hd - 1.
            if (node->left != nullptr) {
                q.push({node->left, hd - 1});
            }

            // Enqueue right child with a horizontal distance of hd + 1.
            if (node->right != nullptr) {
                q.push({node->right, hd + 1});
            }
        }

        // The map is already sorted by key (horizontal distance).
        // Populate the result vector from the map.
        for (auto const& [hd, val] : view_map) {
            result.push_back(val);
        }

        return result;
    }
};

Step-by-Step Execution Table
Let's trace the algorithm with the input tree: root = [1, 2, 3, null, 4]
Tree Structure: 1 -> (L:2, R:3), 2 -> (R:4). HD of 4 is 0-1+1=0.
| Step | q (front -> back) | current_pair | view_map | Explanation |
|---|---|---|---|---|
| 1 | [{1, 0}] | - | {} | Initial state: push {root, 0}. |
| 2 | [] | {1, 0} | {0: 1} | Dequeue {1, 0}. hd=0 is not in map. Add it. |
| 3 | [{2, -1}, {3, 1}] | {1, 0} | {0: 1} | Enqueue left child {2, -1} and right {3, 1}. |
| 4 | [{3, 1}] | {2, -1} | {-1: 2, 0: 1} | Dequeue {2, -1}. hd=-1 not in map. Add it. |
| 5 | [{3, 1}, {4, 0}] | {2, -1} | {-1: 2, 0: 1} | Enqueue right child {4, 0}. |
| 6 | [{4, 0}] | {3, 1} | {-1: 2, 0: 1, 1: 3} | Dequeue {3, 1}. hd=1 not in map. Add it. |
| 7 | [{4, 0}] | {3, 1} | {-1: 2, 0: 1, 1: 3} | Node 3 has no children to enqueue. |
| 8 | [] | {4, 0} | {-1: 2, 0: 1, 1: 3} | Dequeue {4, 0}. hd=0 is in map. Do nothing. |
| 9 | [] | - | {-1: 2, 0: 1, 1: 3} | Queue is empty. Loop terminates. |
| 10 | - | - | - | Iterate through map: val at key -1 is 2, at 0 is 1, at 1 is 3. |
Final Result: [2, 1, 3]
📊 10. Detailed Complexity Analysis
 * Time Complexity: O(N \\log HD)
   * The while loop runs N times, as each node is enqueued and dequeued exactly once.
   * Inside the loop, the dominant operation is view_map.find() and the potential insertion view_map[hd] = .... For std::map (a balanced binary search tree), these operations take O(\\log K) time, where K is the number of elements in the map. The maximum number of elements is the number of unique horizontal distances, HD.
   * Thus, the total time is N \\times O(\\log HD) = O(N \\log HD).
 * Space Complexity: O(N)
   * Queue: In the worst case (a complete binary tree), the maximum number of nodes in the queue at one time is the width of the tree, W, which can be up to N/2. So, queue space is O(N).
   * Map: The map stores one entry for each unique horizontal distance. In the worst case (a skewed tree), every node has a unique horizontal distance, so the map can store up to N entries.
   * Combining these, the total space complexity is O(N).
📚 11. Similar Questions Table
| Question Title | Difficulty | Description | Why It’s Similar |
|---|---|---|---|
| Bottom View of Binary Tree | Medium | Find the nodes visible when viewing the tree from the bottom. | Almost identical logic. Use BFS with horizontal distance. The only change is you always update the map entry for a given HD, so the last node seen at that HD (the lowest one) is stored. |
| Vertical Order Traversal | Hard | Group nodes by horizontal distance and return them, sorted top-to-bottom, and by value if at the same position. | An extension of Top/Bottom View. Requires storing a list of nodes for each HD, not just one. Also requires careful sorting. |
| Binary Tree Right/Left Side View | Medium | Find the nodes visible from the right or left side of the tree. | Also a "view" problem, but uses levels/depth as the primary coordinate instead of horizontal distance. Solved with BFS or DFS. |
| Diagonal Traversal of a Binary Tree | Medium | Group nodes that fall on the same diagonal line. | Similar pattern of tracking coordinates. The "diagonal distance" is often calculated as row - col or row + col, and a map is used to group nodes by this distance. |
