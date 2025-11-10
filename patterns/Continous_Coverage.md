
This group is about **covering a continuous range [0 … target]** using the **fewest intervals or jumps**,
and it includes the three powerhouse problems below 👇

---

# 🧩 GROUP 2 — Continuous Coverage / Range Expansion Family

**Core theme:**

> “We have partial ranges that can cover portions of a line.
> Find the *minimum number of ranges* (or steps) to cover the entire [0 … target] interval.”

---

| Problem                                      | LeetCode # | Concept                                         |
| -------------------------------------------- | ---------- | ----------------------------------------------- |
| **Jump Game II**                             | 45         | Minimum jumps to reach end (array reachability) |
| **Minimum Number of Taps to Water a Garden** | 1326       | Minimum intervals (taps) to cover [0 … n]       |
| **Video Stitching**                          | 1024       | Minimum clips to cover entire time [0 … T]      |

---

## 🎯 1️⃣ Jump Game II (LeetCode 45)

---

### 🔹 Problem

**You’re at index 0 of an array `nums`,
where each element `nums[i]` is the maximum distance you can jump forward.
Find the **minimum number of jumps** required to reach the end.**

---

### 💡 Intuition (Forward Coverage)

Think of each index `i` as covering a *range* `[i, i + nums[i]]`.
You want to **expand coverage forward** with as few jumps as possible.

At each step:

* `curEnd` = end of the range you can reach with current jumps
* `nextEnd` = farthest you can reach by exploring within `curEnd`
* When you reach `curEnd`, increment jump count and move `curEnd = nextEnd`

🧠 It’s like **layer-by-layer BFS** — every time you go beyond `curEnd`, it’s one more jump.

---

### 💻 Java Solution

```java
class Solution {
    public int jump(int[] nums) {
        int jumps = 0, curEnd = 0, nextEnd = 0;

        for (int i = 0; i < nums.length - 1; i++) {
            nextEnd = Math.max(nextEnd, i + nums[i]);
            if (i == curEnd) {
                jumps++;
                curEnd = nextEnd;
            }
        }
        return jumps;
    }
}
```

---

### 🧠 Example

```
nums = [2,3,1,1,4]

Step 1: Start at 0 → can reach [0–2]
  nextEnd = 2, curEnd = 0 → need new jump → jumps=1, curEnd=2
Step 2: Explore [1–2]
  i=1 → reach 4 → nextEnd=4
  i=2 → no farther
Reached curEnd=2 → new jump → jumps=2, curEnd=4
Reached end ✅
```

✅ **Answer = 2**

---

### ⏱️ Complexity

* **Time:** O(n)
* **Space:** O(1)

---

## 💧 2️⃣ Minimum Number of Taps to Water a Garden (LeetCode 1326)

---

### 🔹 Problem

**There’s a garden of length `n`, and `ranges[i]` defines how far the tap at position `i` can water —
it covers `[i − ranges[i], i + ranges[i]]`.
Find the **minimum number of taps** needed to cover `[0 … n]`.**

---

### 💡 Intuition

Exactly same as **Jump Game II**, except we must first **convert ranges into intervals**.
For each left boundary L, track the **farthest reach R**.

Then do a forward greedy sweep:

* `curEnd` = end of range currently covered by opened taps
* `nextEnd` = farthest reach so far
* Every time `i == curEnd`, open one more tap

---

### 💻 Java Solution

```java
class Solution {
    public int minTaps(int n, int[] ranges) {
        int[] maxReach = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            int L = Math.max(0, i - ranges[i]);
            int R = Math.min(n, i + ranges[i]);
            maxReach[L] = Math.max(maxReach[L], R);
        }

        int taps = 0, curEnd = 0, nextEnd = 0;
        for (int i = 0; i <= n; i++) {
            if (i > nextEnd) return -1;  // gap → impossible
            nextEnd = Math.max(nextEnd, maxReach[i]);
            if (i == curEnd && i != n) {
                taps++;
                curEnd = nextEnd;
            }
        }
        return taps;
    }
}
```

---

### 🧠 Example

```
n = 5
ranges = [3,4,1,1,0,0]
Tap intervals:
0→[0,3], 1→[0,5], 2→[1,3], 3→[2,4], 4→[4,4], 5→[5,5]

maxReach = [5,3,4,0,4,5]
Sweep:
x=0 → nextEnd=5 → curEnd=0 → open tap1 → curEnd=5
✅ Covered 0–5 with 1 tap
```

✅ **Answer = 1**

---

### ⏱️ Complexity

* **Time:** O(n)
* **Space:** O(n)

---

## 🎬 3️⃣ Video Stitching (LeetCode 1024)

---

### 🔹 Problem

You’re given clips `[start, end]` and a target `T`.
You must pick the **minimum number of clips** to cover the entire `[0 … T]` interval.

---

### 💡 Intuition

Identical logic to **Water Garden**:

1. Preprocess each possible start → store the **farthest end** reachable from that start.
2. Then do a greedy forward coverage sweep.

---

### 💻 Java Solution

```java
class Solution {
    public int videoStitching(int[][] clips, int T) {
        int[] maxReach = new int[T + 1];
        for (int[] clip : clips) {
            if (clip[0] <= T)
                maxReach[clip[0]] = Math.max(maxReach[clip[0]], Math.min(T, clip[1]));
        }

        int clipsUsed = 0, curEnd = 0, nextEnd = 0;
        for (int i = 0; i <= T; i++) {
            if (i > nextEnd) return -1;
            nextEnd = Math.max(nextEnd, maxReach[i]);
            if (i == curEnd && i != T) {
                clipsUsed++;
                curEnd = nextEnd;
            }
        }
        return clipsUsed;
    }
}
```

---

### 🧠 Example

```
clips = [[0,2],[4,6],[8,10],[1,9],[1,5],[5,9]]
T = 10
After preprocessing:
maxReach[0]=2, [1]=9, [4]=6, [5]=9, [8]=10

Greedy sweep:
i=0 → nextEnd=2 → new clip (count=1)
i=1 → nextEnd=9
i=2 → curEnd=2 → new clip (count=2, curEnd=9)
i=8 → nextEnd=10 → curEnd=9 → new clip (count=3)
✅ Covered [0–10]
```

✅ **Answer = 3**

---

### ⏱️ Complexity

* **Time:** O(T + n)
* **Space:** O(T)

---

## 🧩 Common Greedy Template

```java
int minStepsToCover(int target, int[] maxReach) {
    int steps = 0, curEnd = 0, nextEnd = 0;

    for (int i = 0; i <= target; i++) {
        if (i > nextEnd) return -1;
        nextEnd = Math.max(nextEnd, maxReach[i]);
        if (i == curEnd && i != target) {
            steps++;
            curEnd = nextEnd;
        }
    }
    return steps;
}
```

✅ You can reuse this *exact same logic* for all 3 problems —
only the way you **build `maxReach[]`** changes.

---

## ⚖️ Comparison Summary — Group 2: Continuous Coverage

| Problem                    | Goal                           | Input Form            | Preprocess            | Greedy Logic                    | Result    |
| -------------------------- | ------------------------------ | --------------------- | --------------------- | ------------------------------- | --------- |
| **Jump Game II (45)**      | Reach end in min jumps         | Array of jumps        | Direct indices        | Expand reach while `i ≤ curEnd` | Min jumps |
| **Water Garden (1326)**    | Cover `[0 … n]` with min taps  | `ranges[i]` intervals | Build maxReach[L]     | Expand forward coverage         | Min taps  |
| **Video Stitching (1024)** | Cover `[0 … T]` with min clips | `[start, end]` clips  | Build maxReach[start] | Expand forward coverage         | Min clips |

---

### 🧠 Shared Intuition

> “As long as your current range covers the current position,
> keep extending `nextEnd`.
> The moment you reach `curEnd`, you must open another (tap/clip/jump).
> This ensures continuous coverage with minimum selections.”

---


