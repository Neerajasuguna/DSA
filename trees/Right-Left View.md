
# 🌳 Right View and Left View of a Binary Tree

This repository demonstrates how to compute the **Right View** and **Left View** of a binary tree using **Breadth-First Search (Level Order Traversal)** and **Depth-First Search (DFS)** approaches.

---

## 📌 What is a Tree View?

When a binary tree is observed from a **particular side**, only certain nodes are visible.

* **Right View** → nodes visible when the tree is viewed from the **right**
* **Left View** → nodes visible when the tree is viewed from the **left**

✅ From each **level**, **only one node** contributes to the view.

---

## 🌳 Example

```
        1
       / \
      2   3
       \   \
        5   4
```

| View       | Output      |
| ---------- | ----------- |
| Left View  | `[1, 2, 5]` |
| Right View | `[1, 3, 4]` |

---

## 🧠 Core Intuition (Very Important)

### ✅ Key Observation

* A binary tree has **levels**
* From **each level**, only **one node is visible**
* That visible node depends on the **order of traversal**

---

## ✅ Level Order (BFS) Intuition

### Why BFS works perfectly?

* BFS processes nodes **level by level**
* Once we isolate a level, we can:

  * Take the **first node** (if ordered correctly)
  * Or take the **last node**

### For Left View

* Traverse **left → right**
* Take the **first node** at each level

### For Right View

Two valid strategies:

1. **Left → right** → take **last node**
2. **Right → left** → take **first node**

---

# ✅ Right View of Binary Tree

---

## BFS Approach (Recommended in Interviews)

### ✅ Java Code

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;

        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);

        while (!q.isEmpty()) {
            int size = q.size();

            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();

                // last node in current level
                if (i == size - 1) {
                    res.add(node.val);
                }

                if (node.left != null) q.offer(node.left);
                if (node.right != null) q.offer(node.right);
            }
        }
        return res;
    }
}
```

---

## ⏱️ Time Complexity

```
O(n)
```

## 🧠 Space Complexity

* Queue (worst case): `O(n)`
* Result list: `O(h)`
* **Overall:** `O(n)`

---

# ✅ Left View of Binary Tree

---

## BFS Approach

### ✅ Java Code

```java
class Solution {
    public List<Integer> leftSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;

        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);

        while (!q.isEmpty()) {
            int size = q.size();

            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();

                // first node in current level
                if (i == 0) {
                    res.add(node.val);
                }

                if (node.left != null) q.offer(node.left);
                if (node.right != null) q.offer(node.right);
            }
        }
        return res;
    }
}
```

---

## ⏱️ Time Complexity

```
O(n)
```

## 🧠 Space Complexity

```
O(n)   // queue in worst case
```

---

# ✅ DFS Intuition (Alternate)

### Key Idea

* Traverse **root first**
* Choose direction priority:

  * **Right → Left** → Right View
  * **Left → Right** → Left View
* First node encountered at a level is the visible node

### DFS Space Advantage

* Uses `O(h)` recursion stack
* Better for **deep but narrow trees**

---

## ✅ Right View (DFS)

```java
class Solution {
    List<Integer> res = new ArrayList<>();

    public List<Integer> rightSideView(TreeNode root) {
        dfs(root, 0);
        return res;
    }

    private void dfs(TreeNode node, int level) {
        if (node == null) return;

        if (level == res.size()) {
            res.add(node.val);
        }

        dfs(node.right, level + 1);
        dfs(node.left, level + 1);
    }
}
```

---

## 🎯 Interview Summary (Say This ✅)

> “Tree views work by selecting one node per level. BFS naturally separates levels, making it easy to choose the leftmost or rightmost node. DFS works by prioritizing traversal direction and capturing the first node at each depth.”

---

## ✅ Final Complexity Comparison

| Approach | Time   | Space  | Notes                               |
| -------- | ------ | ------ | ----------------------------------- |
| BFS      | `O(n)` | `O(n)` | Simple & intuitive                  |
| DFS      | `O(n)` | `O(h)` | Memory-efficient for balanced trees |

---

## ✅ Takeaways

* Always think in **levels**
* Views = **one node per level**
* Choose BFS for clarity, DFS for elegance

---
