Problem Statement — LeetCode 305. Number of Islands II

**You are given an m x n grid initially filled with water (0).
You are also given a list of positions where land (1) is added one by one.**

After each addition, you must return the number of islands.

An island is a group of 1s connected 4-directionally (up, down, left, right).

🧠 Intuition

Start with all water → no islands.

Each time we add a land, it may create a new island, or it may merge with adjacent lands (reducing island count).

So, the trick is:
Each addition = +1 island → but if it connects with an existing island, we merge (union) → -1 per merge.

This is where Union-Find (Disjoint Set Union) comes in.

⚙️ Approach

**Represent grid cells as 1D indices
id = r * n + c**

So we can use DSU arrays easily.

Union-Find operations

find(x) → find parent

union(x, y) → connect two lands if they belong to different islands

Maintain a count of distinct roots (islands)

For each position (r, c):

  **If already land → skip.**

  **Mark as land and increment island count.**

  **Check 4 neighbors — if neighbor is land → union both and decrement count if merged.**

🧮 Example
m = 3, n = 3
positions = [[0,0],[0,1],[1,2],[2,1]]

Step 1: Add (0,0) → 1 island
Step 2: Add (0,1) → connects to (0,0) → still 1 island
Step 3: Add (1,2) → isolated → 2 islands
Step 4: Add (2,1) → isolated → 3 islands
Final Output = [1,1,2,3]

💻 Java Solution
class Solution {
    int[] parent, rank;
    int count = 0; // number of islands

    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        List<Integer> res = new ArrayList<>();
        parent = new int[m * n];
        rank = new int[m * n];
        Arrays.fill(parent, -1);

        int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};

        for (int[] pos : positions) {
            int r = pos[0], c = pos[1];
            int id = r * n + c;
            if (parent[id] != -1) {  // already land
                res.add(count);
                continue;
            }

            parent[id] = id;
            count++;

            for (int[] d : dirs) {
                int nr = r + d[0], nc = c + d[1];
                int nid = nr * n + nc;
                if (nr < 0 || nc < 0 || nr >= m || nc >= n || parent[nid] == -1)
                    continue;
                if (union(id, nid))
                    count--;
            }

            res.add(count);
        }

        return res;
    }

    private int find(int x) {
        if (x != parent[x])
            parent[x] = find(parent[x]);
        return parent[x];
    }

    private boolean union(int x, int y) {
        int rootX = find(x), rootY = find(y);
        if (rootX == rootY) return false;

        if (rank[rootX] > rank[rootY]) parent[rootY] = rootX;
        else if (rank[rootX] < rank[rootY]) parent[rootX] = rootY;
        else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }

        return true;
    }
}

⏱️ Time Complexity

For each position, we perform up to 4 unions → O(k * α(n))
where k = positions.length, α = inverse Ackermann function (almost constant).

💾 Space Complexity

O(m * n) for Union-Find arrays.



**The Conversion Formula**

Given a grid with dimensions m x n:

m → number of rows

n → number of columns

**For any cell (r, c) (0-indexed):**

**id = r * n + c**


This gives a unique 1D index for every 2D coordinate.

🎯 Intuition

Think of the grid cells as being laid out row by row in a single line:

Grid (3x3):
(0,0) (0,1) (0,2)
(1,0) (1,1) (1,2)
(2,0) (2,1) (2,2)


Flattened (1D representation):

2D Cell	1D 
(0,0)	0
(0,1)	1
(0,2)	2
(1,0)	3
(1,1)	4

