
**Core theme:**

**> “We have intervals (start, end). We must select, skip, merge, or minimize overlaps.**”

All these use one of two **greedy patterns**:
1️⃣ Sort by **end time** → to minimize overlaps / pick earliest finishing
2️⃣ Sort by **start time** → to merge / detect overlap ranges

---

## 🎯 PROBLEM 1 — Minimum Number of Arrows to Burst Balloons (LeetCode 452)


### 🔹 Problem

Given intervals `[start, end]`, find the *minimum number of arrows* needed to burst all balloons.
Each arrow can burst all balloons overlapping that point.

---

### 💡 Intuition

* Sort intervals by `end`.
* Shoot one arrow at the `end` of the first balloon.
* Skip all balloons starting before or at that point (overlapping).
* When the next balloon starts *after* current arrow position, shoot a new arrow.

---

### 💻 Java Solution

```java
import java.util.*;

class Solution {
    public int findMinArrowShots(int[][] points) {
        if (points.length == 0) return 0;

        Arrays.sort(points, (a, b) -> Long.compare((long)a[1], (long)b[1]));
        int arrows = 1;
        long arrowPos = points[0][1];

        for (int i = 1; i < points.length; i++) {
            if (points[i][0] > arrowPos) {
                arrows++;
                arrowPos = points[i][1];
            }
        }
        return arrows;
    }
}
```

---

### 🧠 Example

```
Input: [[1,6],[2,8],[7,12],[10,16]]
Sorted: [[1,6],[2,8],[7,12],[10,16]]

Shoot @6 → bursts [1,6],[2,8]
Next start=7>6 → shoot @12 → bursts [7,12],[10,16]
Answer = 2
```

✅ **Output = 2**

---

### ⏱️ Complexity

* Time: **O(n log n)** (sorting)
* Space: **O(1)**

---

### 🧩 Pattern Recognized

✅ Sort by end
✅ Choose earliest finishing interval
✅ Skip overlapping intervals

Same pattern will apply in the next problems too.

---

## 🧩 PROBLEM 2 — Non-overlapping Intervals (LeetCode 435)

---

### 🔹 Problem

Given a set of intervals, remove the *minimum number* of intervals so that the rest do **not overlap**.
Return the number of intervals you must remove.

---

### 💡 Intuition

This is the *same as the balloon problem*, but instead of counting arrows, we count **kept intervals**.
The rest (total - kept) = removed.

1. Sort by end.
2. Keep the first interval.
3. For every next interval:

   * If `start >= prevEnd` → keep it.
   * Else → overlapping → skip (count removal).

---

### 💻 Java Solution

```java
import java.util.*;

class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if (intervals.length == 0) return 0;

        Arrays.sort(intervals, (a, b) -> Integer.compare(a[1], b[1]));

        int count = 1;
        int end = intervals[0][1];

        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] >= end) {
                count++;
                end = intervals[i][1];
            }
        }

        return intervals.length - count; // remove rest
    }
}
```

---

### 🧠 Example

```
Input: [[1,2],[2,3],[3,4],[1,3]]
Sorted by end: [[1,2],[2,3],[1,3],[3,4]]

Keep [1,2] → end=2
[2,3] no overlap → keep
[1,3] overlaps → remove
[3,4] no overlap → keep

Kept = 3, Total = 4 → Removed = 1
```

✅ **Output = 1**

---

### 🧩 Pattern

* Sort by end
* Count max non-overlapping intervals
  → Remove = total - kept

Exactly same structure as **Burst Balloons** but slightly inverted logic.

---

## 🧩 PROBLEM 3 — Activity Selection Problem (Classic)

---

### 🔹 Problem

You’re given start and end times of activities.
Select the **maximum number of activities** you can perform without overlap.

---

### 💡 Intuition

Same as Non-overlapping Intervals — maximize compatible activities.
Greedy: Pick earliest finishing activity that doesn’t overlap with previously picked one.

---

### 💻 Java Solution

```java
import java.util.*;

class ActivitySelection {
    public static int maxActivities(int[][] activities) {
        Arrays.sort(activities, (a, b) -> Integer.compare(a[1], b[1]));
        int count = 1;
        int end = activities[0][1];

        for (int i = 1; i < activities.length; i++) {
            if (activities[i][0] >= end) {
                count++;
                end = activities[i][1];
            }
        }
        return count;
    }
}
```

---

### 🧠 Example

```
Activities = [[1,2],[3,4],[0,6],[5,7],[8,9],[5,9]]
Sorted by end → [[1,2],[3,4],[0,6],[5,7],[8,9],[5,9]]

Pick [1,2] → end=2
[3,4] ok → pick
[0,6] skip
[5,7] ok → pick
[8,9] ok → pick
✅ 4 activities
```

✅ **Output = 4**

---

### 🧩 Pattern

Same as **Non-overlapping Intervals**, but counts *kept activities* rather than *removed* ones.

---

## 🧩 PROBLEM 4 — Meeting Rooms II (LeetCode 253)

---

### 🔹 Problem

Given meeting intervals `[start, end]`, find the **minimum number of rooms** required so that no meetings overlap in the same room.

---

### 💡 Intuition

We are not removing overlaps — we are **counting** how many overlaps exist at a time.
That gives the number of *rooms* needed.

### Two main ways:

1. **Min-Heap (priority queue)** — track ongoing meetings by end time.
2. **Sweep line technique** — use two sorted arrays of starts & ends.

---

### 💻 Java (Sweep Line, O(n log n))

```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        int n = intervals.length;
        int[] starts = new int[n];
        int[] ends = new int[n];

        for (int i = 0; i < n; i++) {
            starts[i] = intervals[i][0];
            ends[i] = intervals[i][1];
        }

        Arrays.sort(starts);
        Arrays.sort(ends);

        int rooms = 0, endPtr = 0;
        for (int start : starts) {
            if (start < ends[endPtr]) rooms++;  // overlap → need new room
            else endPtr++;  // one meeting finished → reuse room
        }
        return rooms;
    }
}
```

---

### 🧠 Example

```
Meetings: [0,30],[5,10],[15,20]
starts = [0,5,15]
ends   = [10,20,30]

0<10 → room=1  
5<10 → overlap → room=2  
15≥10 → reuse room (endPtr++) → now 15<20 → overlap → room=2  
✅ Answer = 2
```

✅ **Output = 2**

---

### 🧩 Pattern

* Sort starts & ends separately
* Track active intervals
  → Max concurrent intervals = number of rooms

---




**✅ 1️⃣ Interval Scheduling Maximization (a.k.a. Activity Selection generalized)**
🔹 Problem

Given a list of intervals [start, end], choose the maximum number of non-overlapping intervals.

This is the classic greedy foundation — sometimes appears as:

“Schedule as many compatible meetings as possible.”

💡 Intuition

This is exactly the same as the Activity Selection Problem or Non-overlapping Intervals (LeetCode 435), but we maximize kept instead of minimizing removals.

Greedy rule:

Always pick the interval that finishes earliest among those that start after or at the last selected interval’s end.

💻 Java Solution
import java.util.*;

class IntervalScheduling {
    public int maxNonOverlappingIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[1], b[1]));
        int count = 0;
        int lastEnd = Integer.MIN_VALUE;

        for (int[] interval : intervals) {
            if (interval[0] >= lastEnd) {
                count++;
                lastEnd = interval[1];
            }
        }
        return count;
    }
}

🧠 Example
Intervals = [ [1,3], [2,5], [4,6], [6,7], [5,9] ]
Sorted by end → [1,3], [4,6], [6,7], [2,5], [5,9]

Pick [1,3] → end=3  
Next [4,6] → ok  
Next [6,7] → ok  
Next [2,5] → skip (overlaps)
Next [5,9] → skip (overlaps)

✅ Answer = 3

🧩 Pattern Summary
Step	Action
Sort by end	To choose earliest finishing
Pick interval if start >= lastEnd	Avoid overlaps
Count selections	Maximize compatible tasks

✅ Same as Activity Selection, just often named differently in system-design-like scheduling questions.

**✅ 2️⃣ Merge Intervals (LeetCode 56)**
🔹 Problem

Given a list of intervals, merge all overlapping ones and return the resulting list.

💡 Intuition

Unlike the others, this one doesn’t minimize or count anything — it combines intervals that overlap.

Rule:

Sort by start.
Traverse and merge whenever the next interval’s start ≤ current end.

💻 Java Solution
import java.util.*;

class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals.length == 0) return new int[0][0];

        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        List<int[]> merged = new ArrayList<>();

        int[] curr = intervals[0];
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] <= curr[1]) {
                curr[1] = Math.max(curr[1], intervals[i][1]);
            } else {
                merged.add(curr);
                curr = intervals[i];
            }
        }
        merged.add(curr);

        return merged.toArray(new int[merged.size()][]);
    }
}

🧠 Example
Input: [ [1,3], [2,6], [8,10], [9,11] ]
Sorted: [ [1,3], [2,6], [8,10], [9,11] ]

Merge [1,3] + [2,6] → [1,6]
Next [8,10] + [9,11] → [8,11]
✅ Output: [ [1,6], [8,11] ]

🧩 Pattern Summary
Sort by	Merge Condition	Output
Start	next.start ≤ curr.end	new merged intervals

🧠 Common follow-up:

“Given merged meetings, find free time between them” → leads to the next problem 👇

**✅ 3️⃣ Employee Free Time (LeetCode 759)**
🔹 Problem

You are given employees’ working hours as intervals:

[[[1,2],[5,6]], [[1,3]], [[4,10]]]


Find common free time intervals across all employees.

💡 Intuition

Flatten all employees’ intervals into one list.

Merge overlapping intervals → get the union of all busy times.

Free times = gaps between merged intervals.

💻 Java Solution
import java.util.*;

class Solution {
    public List<int[]> employeeFreeTime(List<List<int[]>> schedule) {
        List<int[]> all = new ArrayList<>();
        for (List<int[]> emp : schedule) all.addAll(emp);

        // Step 1: sort by start
        all.sort((a, b) -> Integer.compare(a[0], b[0]));

        // Step 2: merge busy intervals
        List<int[]> merged = new ArrayList<>();
        int[] curr = all.get(0);
        for (int i = 1; i < all.size(); i++) {
            if (all.get(i)[0] <= curr[1]) {
                curr[1] = Math.max(curr[1], all.get(i)[1]);
            } else {
                merged.add(curr);
                curr = all.get(i);
            }
        }
        merged.add(curr);

        // Step 3: find gaps (free time)
        List<int[]> free = new ArrayList<>();
        for (int i = 1; i < merged.size(); i++) {
            int start = merged.get(i - 1)[1];
            int end = merged.get(i)[0];
            if (start < end) free.add(new int[]{start, end});
        }

        return free;
    }
}

🧠 Example
Input:
Employee 1: [1,2],[5,6]
Employee 2: [1,3]
Employee 3: [4,10]

All intervals = [1,2],[5,6],[1,3],[4,10]
Sorted = [1,2],[1,3],[4,10],[5,6]
Merged busy = [1,3],[4,10]
Free gaps = [3,4]
✅ Output: [ [3,4] ]


| Problem                                                       | Goal                               | Sort By     | Greedy Choice / Logic                   | Output / Result Type     |
| ------------------------------------------------------------- | ---------------------------------- | ----------- | --------------------------------------- | ------------------------ |
| **Burst Balloons (452)**                                      | Fewest points to hit all intervals | End         | Shoot at end of earliest balloon        | Min points (arrows)      |
| **Non-overlapping Intervals (435)**                           | Remove overlaps                    | End         | Keep earliest finishing interval        | Min removals             |
| **Activity Selection** / **Interval Scheduling Maximization** | Maximize compatible tasks          | End         | Pick earliest finishing non-overlapping | Max tasks                |
| **Meeting Rooms II (253)**                                    | Count concurrent overlaps          | Start & End | Sweep overlap count using starts/ends   | Min rooms                |
| **Merge Intervals (56)**                                      | Combine overlapping intervals      | Start       | Merge while next.start ≤ curr.end       | List of merged intervals |
| **Employee Free Time (759)**                                  | Find common gaps between schedules | Start       | Merge busy intervals, find gaps         | List of free intervals   |


| Type                                   | Common Pattern                            | Example Problems                          |
| -------------------------------------- | ----------------------------------------- | ----------------------------------------- |
| **Minimize overlaps / removals**       | Sort by **end** → keep earliest finishing | Burst Balloons, Non-overlapping Intervals |
| **Maximize non-conflicting intervals** | Sort by **end** → pick earliest finish    | Activity Selection, Interval Scheduling   |
| **Count overlaps**                     | Sort **start & end** separately → sweep   | Meeting Rooms II                          |
| **Merge / Combine overlaps**           | Sort by **start**, merge if overlapping   | Merge Intervals                           |
| **Find gaps / free slots**             | Merge first, then find in-between gaps    | Employee Free Time                        |


