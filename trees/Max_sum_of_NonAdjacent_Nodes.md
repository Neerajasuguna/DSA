Maximum Sum of Non-Adjacent Nodes in a Binary Tree
Topic: Dynamic Programming on Trees
Pattern: Postorder DFS
Related problem: House Robber III (LeetCode 337)
1. Problem statement
Given the root of a binary tree, find the maximum sum of node values
such that no two directly connected nodes (a parent and its child) are
selected together.

A valid selection may contain a node and its grandchildren, but it
cannot contain both a node and either of its children.
2. Example
          10
         /  \
        1    2
       / \    \
      4   5    6
One optimal selection is 10, 4, 5, 6.
- Select 10; therefore, do not select its children 1 and 2.
- Since 1 is not selected, its children 4 and 5 can be selected.
- Since 2 is not selected, its child 6 can be selected.
Maximum sum: 10 + 4 + 5 + 6 = 25.

Answer: 25
3. Approach: Tree DP using postorder DFS
At each node, there are two possibilities:
1. Include the current node: its children cannot be included.
2. Exclude the current node: each child can independently be
   included or excluded, whichever gives the larger sum.
We return two DP states from every node:
- include: maximum sum when the current node is selected.
- exclude: maximum sum when the current node is not selected.
Let left and right be the DP results from the left and right
children.
Recurrence
Include current node

If we select the current node, both children must be excluded:
include = node.val + left.exclude + right.exclude
Exclude current node

If we do not select the current node, each child can be selected or
excluded:
exclude = max(left.include, left.exclude)
        + max(right.include, right.exclude)
For a null node:
include = 0
exclude = 0
The final answer is:
max(root.include, root.exclude)
We use postorder traversal because we need the answers from both
children before calculating the current node's states.
4. Java solution using an int array
dp[0] stores exclude, and dp[1] stores include.
class Solution {

    public int getMaxSum(TreeNode root) {
        int[] result = solve(root);

        return Math.max(result[0], result[1]);
    }

    private int[] solve(TreeNode root) {

        if (root == null) {
            return new int[]{0, 0};
        }

        // Solve left and right subtrees first.
        int[] left = solve(root.left);
        int[] right = solve(root.right);

        // State 0: Exclude the current node.
        int exclude = Math.max(left[0], left[1])
                    + Math.max(right[0], right[1]);

        // State 1: Include the current node.
        // Both children must be excluded.
        int include = root.val + left[0] + right[0];

        return new int[]{exclude, include};
    }
}
For LeetCode 337, use the method signature
public int rob(TreeNode root) instead of getMaxSum. The DP logic
remains the same.

5. Java solution using a Pair class
A small class makes the meaning of the two states explicit and can be
easier to read during interviews.
class Pair {
    int include;
    int exclude;

    Pair(int include, int exclude) {
        this.include = include;
        this.exclude = exclude;
    }
}

class Solution {

    public int getMaxSum(TreeNode root) {
        Pair result = solve(root);

        return Math.max(result.include, result.exclude);
    }

    private Pair solve(TreeNode root) {

        if (root == null) {
            return new Pair(0, 0);
        }

        Pair left = solve(root.left);
        Pair right = solve(root.right);

        int include = root.val
                    + left.exclude
                    + right.exclude;

        int exclude = Math.max(left.include, left.exclude)
                    + Math.max(right.include, right.exclude);

        return new Pair(include, exclude);
    }
}
6. Dry run
Use this tree:
          10
         /  \
        1    2
       / \    \
      4   5    6
We process nodes in postorder: left subtree, right subtree, then current
node.

  Node     Exclude   Include
  4              0         4
5              0         5
1              9         1
6              0         6
2              6         2
10            15        25
Calculation at node 1
Its children are 4 and 5.
exclude = max(0, 4) + max(0, 5)
        = 4 + 5
        = 9

include = 1 + 0 + 0
        = 1
Return: (include = 1, exclude = 9).
Calculation at node 2
Its left child is null and its right child is 6.
exclude = max(0, 0) + max(0, 6)
        = 6

include = 2 + 0 + 0
        = 2
Return: (include = 2, exclude = 6).
Calculation at root 10
exclude = max(1, 9) + max(2, 6)
        = 9 + 6
        = 15

include = 10 + 9 + 6
        = 25
Final answer: max(15, 25) = 25.
7. Why are there two states?
The parent needs to know whether the current node was selected.
- If the parent is selected, the current node must be excluded.
- If the parent is excluded, the current node may be either included
  or excluded.
That is why returning only one maximum value from a subtree is not
enough. We return both possibilities so the parent can make the correct
decision.
8. Complexity analysis
Let N be the number of nodes and H be the height of the tree.

  Complexity        Value    Reason
  Time              O(N)   Each node is processed once.
Auxiliary space   O(H)   Recursion stack depth is the tree height.

Each recursive call returns only two values. The active recursion stack
uses O(H) space.
9. Edge cases
Empty tree
root = null
answer = 0
Single node
    5

answer = 5
All negative values
       -3
       / \
     -2  -5

answer = 0
If selecting no nodes is allowed, the maximum sum is zero. The
recurrence handles this because excluding a node can produce zero.
10. Interview revision notes
  Tree DP problem         Information returned to Separate answer update?
parent                  
  Diameter of binary tree Height                  Yes, update diameter

  Maximum path sum        Best one-sided path     Yes, update global
gain                    maximum
Maximum sum of          Include and exclude     No, take max at root
non-adjacent nodes      states
Remember the two formulas
// Select current node: children must be excluded.
include = root.val + left.exclude + right.exclude;

// Exclude current node: choose the best state for each child.
exclude = Math.max(left.include, left.exclude)
        + Math.max(right.include, right.exclude);
Key takeaway: This is a classic Dynamic Programming on Trees
problem. Use postorder DFS and return two states---`include` and
exclude---for every node.
