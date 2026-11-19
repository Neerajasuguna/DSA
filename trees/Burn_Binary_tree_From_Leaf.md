Burning Tree / Maximum Distance from a Target Node
What the code in the screenshot does
The screenshot shows a BFS-based solution for finding the maximum
distance from a target node to any other node in a binary tree. This
is commonly known as the Burning Tree problem: the target node starts
burning, and in each time unit the fire spreads to its left child, right
child, and parent.

The answer is the number of BFS levels needed to reach every node
(equivalently, the maximum distance from the target).
Assumption: node values are unique, so the target can be located by
its value. If duplicate values are allowed, pass the actual target
node reference instead.

Java implementation
import java.util.*;

class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int val) {
        this.val = val;
    }
}

class Solution {

    // Maps every node to its parent and returns the target node.
    private TreeNode mapParentsAndFindTarget(
            TreeNode root,
            Map<TreeNode, TreeNode> parent,
            int targetValue) {

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        TreeNode target = null;
        parent.put(root, null);

        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();

            if (node.val == targetValue) {
                target = node;
            }

            if (node.left != null) {
                parent.put(node.left, node);
                queue.offer(node.left);
            }

            if (node.right != null) {
                parent.put(node.right, node);
                queue.offer(node.right);
            }
        }

        return target;
    }

    // BFS from target, moving through left, right, and parent links.
    private int findMaxDistance(
            TreeNode target,
            Map<TreeNode, TreeNode> parent) {

        Queue<TreeNode> queue = new LinkedList<>();
        Set<TreeNode> visited = new HashSet<>();

        queue.offer(target);
        visited.add(target);

        int maxDistance = 0;

        while (!queue.isEmpty()) {
            int size = queue.size();
            boolean spreadThisLevel = false;

            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();

                // Spread to the left child.
                if (node.left != null && visited.add(node.left)) {
                    queue.offer(node.left);
                    spreadThisLevel = true;
                }

                // Spread to the right child.
                if (node.right != null && visited.add(node.right)) {
                    queue.offer(node.right);
                    spreadThisLevel = true;
                }

                // Spread to the parent.
                TreeNode p = parent.get(node);
                if (p != null && visited.add(p)) {
                    queue.offer(p);
                    spreadThisLevel = true;
                }
            }

            // Count one time unit only if fire reached new node(s).
            if (spreadThisLevel) {
                maxDistance++;
            }
        }

        return maxDistance;
    }

    public int timeToBurnTree(TreeNode root, int targetValue) {
        if (root == null) {
            return 0;
        }

        Map<TreeNode, TreeNode> parent = new HashMap<>();

        TreeNode target =
                mapParentsAndFindTarget(root, parent, targetValue);

        // Target value was not found in the tree.
        if (target == null) {
            return -1;
        }

        return findMaxDistance(target, parent);
    }
}

public class Main {
    public static void main(String[] args) {
        /*
                  1
                 / \\
                2   3
               / \\   \\
              4   5   6
        */

        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);
        root.right.right = new TreeNode(6);

        Solution solution = new Solution();

        System.out.println(solution.timeToBurnTree(root, 5));
        // Output: 4
    }
}
Explanation
1. Why do we need a parent map?
A normal binary tree node has links only to its left and right children.
But the fire can also spread upwards to the parent.

So we build a map:

parent.put(child, parentNode)

For the example tree:
          1
         / \\
        2   3
       / \\   \\
      4   5   6
The parent map contains:

  Node   Parent
  1      null
2      1
3      1
4      2
5      2
6      3

We also locate and return the target node (node 5 in this example).
2. Why BFS?
BFS explores nodes level by level. Each level represents one time unit
of fire spreading.

Starting at target node 5:
Time 0:        5

Time 1:      2

Time 2:    4   1

Time 3:          3

Time 4:          6
Therefore, the farthest node (6) is four edges away from node 5, and
the answer is 4.
3. Why do we need a visited set?
The tree is traversed as an undirected graph during the burning process:
- A node can reach its child.
- A child can reach its parent.
Without visited, traversal could go back and forth indefinitely. The
set ensures that each node is processed only once.
4. Why capture queue.size() before the loop?
int size = queue.size();

for (int i = 0; i < size; i++) {
    // Process the current level.
}
This processes only the nodes that were already in the queue at the
start of the level. Newly discovered nodes belong to the next time unit.
5. Why increment the time conditionally?
if (spreadThisLevel) {
    maxDistance++;
}
We increment time only when at least one new node catches fire. This
avoids counting an extra time unit after the final level.
Complexity analysis
Let N be the number of nodes.

  Operation                                Time   Extra space
  Build parent map and find target         O(N)          O(N)
BFS from target                          O(N)          O(N)
Total                            O(N)      O(N)

The parent map, visited set, and BFS queue use O(N) extra space.
Interview summary
The solution has two BFS traversals:
1. First BFS: build the child-to-parent map and find the target
   node.
2. Second BFS: start from the target and visit left, right, and
   parent nodes level by level.
The number of levels traversed (counting only levels that discover new
nodes) is the minimum time required to burn the entire tree.
