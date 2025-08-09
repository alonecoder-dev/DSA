# TREES

---
The Primitives:
 * Node Representation: First, understand the building block. A node with data, a left pointer, and a right pointer. Internalize this.
 * Depth-First Search (DFS) Traversal:
   * In-order (Left, Root, Right):
     * Action: Implement it recursively.
     * Action: Implement it iteratively using a stack. This is non-negotiable. It will force you to understand how recursion's call stack actually works.
   * Pre-order (Root, Left, Right):
     * Action: Implement it recursively.
     * Action: Implement it iteratively using a stack.
   * Post-order (Left, Right, Root):
     * Action: Implement it recursively.
     * Action: Implement it iteratively. This one is trickier. You can use two stacks, or one stack with a small modification. Master both ways.
 * Breadth-First Search (BFS) / Level Order Traversal:
   * Action: Implement it using a queue. This is the foundation for all "level-by-level" problems.
For EACH of these four traversals (In, Pre, Post, Level-Order), your standard of "mastery" is:
 * You can code the recursive and iterative versions from scratch in under 5 minutes.
 * You can take any sample tree on paper and manually trace the output, showing the state of the stack or queue at each step.

---

Phase 2: Applying the Primitives - Classic Binary Tree (BT) Problems
Now, and only now, do you start solving problems. You will notice that each problem is a slight variation of the traversals you just mastered.
The Order of Attack:
 * Height of a Binary Tree: Recognize this as a post-order traversal pattern. The logic needs info from the left and right children before it can compute the result for the current node.
 * Diameter of a Binary Tree: A more complex post-order application. For each node, you need the height of its left and right subtrees.
 * Level Order Traversal Variations:
   * Zigzag Level Order Traversal
   * Reverse Level Order Traversal
 * Tree Views: These are direct applications of DFS and BFS.
   * Right/Left View: Think about what you see first at each level (BFS) or during a traversal (DFS).
   * Top/Bottom View: This requires horizontal distances. Think about how to track that during a traversal.
   * Vertical Order Traversal: A combination of level-order and tracking horizontal distance.
 * Lowest Common Ancestor (LCA): A classic problem that solidifies your understanding of recursive paths.
 * Boundary Traversal: A composite problem. It's (Left boundary) + (All leaves) + (Reverse of Right boundary). This tests your ability to break a problem down and reuse traversal logic.

---

Phase 3: Specialization - The Binary Search Tree (BST)
A BST is a Binary Tree with a specific rule (the BST-invariant). This rule enables optimizations.
The Order of Attack:
 * The BST Invariant: left_child < root < right_child. Burn this into your brain. Everything depends on it.
 * Search in a BST: Implement recursively and iteratively. Understand how it's faster than a regular BT search. The time complexity changes from O(n) to O(\log n) on a balanced tree. Why? Answer that.
 * Insertion in a BST: Implement recursively and iteratively.
 * Validate a BST: This is a key problem. A naive check at each node is not enough. The entire subtree must satisfy the invariant. This can be solved with an in-order traversal or by passing min/max bounds down the recursion. Master both.
 * Kth Smallest / Largest Element in a BST: Think about your traversals. Which one naturally visits nodes in sorted order? In-order. This is a direct application.
 * Find Floor/Ceil in a BST: Tests your ability to intelligently navigate the tree using the BST invariant.
 * Deletion from a BST: This is the most complex BST operation. Handle the three cases: deleting a leaf, a node with one child, and a node with two children.
.
