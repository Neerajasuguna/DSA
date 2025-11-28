

---

## 1️⃣ Preorder Traversal (Root → Left → Right)

### 🔹 When to use

* Copying a tree
* Serialize / deserialize
* Parent should be processed before children

### ✅ Idea

Visit current node **before** its subtrees.

### ✅ Java Code

```java
class Solution {
    void preorder(TreeNode root, List<Integer> res) {
        if (root == null) return;

        res.add(root.val);          // Root
        preorder(root.left, res);   // Left
        preorder(root.right, res);  // Right
    }
}
```

### ⏱️ Time Complexity

```
O(n)   // each node visited once
```

### 🧠 Space Complexity

* Auxiliary (recursion stack): `O(h)`
* Worst case (skewed tree): `O(n)`
* Balanced tree: `O(log n)`
* Output list: `O(n)`

---

## 2️⃣ Inorder Traversal (Left → Root → Right)

### 🔹 When to use

* Binary Search Tree (BST) → **gives sorted order**
* Expression tree evaluation

### ✅ Idea

Process left subtree first, then node, then right subtree.

### ✅ Java Code

```java
class Solution {
    void inorder(TreeNode root, List<Integer> res) {
        if (root == null) return;

        inorder(root.left, res);    // Left
        res.add(root.val);          // Root
        inorder(root.right, res);   // Right
    }
}
```

### ⏱️ Time Complexity

```
O(n)
```

### 🧠 Space Complexity

* Auxiliary stack: `O(h)`
* Worst case: `O(n)`
* Balanced tree: `O(log n)`
* Output list: `O(n)`

---

## 3️⃣ Postorder Traversal (Left → Right → Root)

### 🔹 When to use

* Deleting/freeing a tree
* Bottom-up calculations (e.g., subtree sizes, heights)

### ✅ Idea

Process children **before** the parent.

### ✅ Java Code

```java
class Solution {
    void postorder(TreeNode root, List<Integer> res) {
        if (root == null) return;

        postorder(root.left, res);   // Left
        postorder(root.right, res);  // Right
        res.add(root.val);           // Root
    }
}
```

### ⏱️ Time Complexity

```
O(n)
```

### 🧠 Space Complexity

* Auxiliary stack: `O(h)`
* Worst case: `O(n)`
* Balanced tree: `O(log n)`
* Output list: `O(n)`

---

## 📌 Comparison Summary (Interview Gold ✅)

| Traversal | Order    | Time   | Space (Aux) | Key Use Case     |
| --------- | -------- | ------ | ----------- | ---------------- |
| Preorder  | Root-L-R | `O(n)` | `O(h)`      | Copy / serialize |
| Inorder   | L-Root-R | `O(n)` | `O(h)`      | BST → sorted     |
| Postorder | L-R-Root | `O(n)` | `O(h)`      | Delete tree      |

---

## 🎯 One-liner for Interviews

> “All DFS traversals run in `O(n)` time and use `O(h)` auxiliary space due to recursion, where `h` is the tree height. In the worst-case skewed tree, space becomes `O(n)`.”

