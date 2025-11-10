
💡 Core Theme

**“Given intervals with start and end times, determine how many resources are simultaneously needed or track active usage dynamically.”**

You’ll see this family appear in meetings, trains, cars, and booking systems.

**🏢 1️⃣ Meeting Rooms II (LeetCode 253)**
📄 Problem Statement

You are given an array of meeting time intervals intervals[i] = [start_i, end_i].
Return the minimum number of conference rooms required so that all meetings can take place without conflict.

🧠 Intuition

Count how many meetings overlap at any moment.
Each overlap → 1 more room needed.
As soon as a meeting ends → a room is freed.

💻 Java Solution (Sweep Line)
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        int n = intervals.length;
        int[] start = new int[n];
        int[] end = new int[n];

        for (int i = 0; i < n; i++) {
            start[i] = intervals[i][0];
            end[i] = intervals[i][1];
        }

        Arrays.sort(start);
        Arrays.sort(end);

        int rooms = 0, endPtr = 0;
        for (int s : start) {
            if (s < end[endPtr]) rooms++;   // new meeting overlaps → new room
            else endPtr++;                  // one meeting finished → reuse room
        }
        return rooms;
    }
}

💡 Example
Input: [[0,30],[5,10],[15,20]]
Sorted starts = [0,5,15]
Sorted ends   = [10,20,30]
0<10 → room=1
5<10 → room=2
15≥10 → reuse (endPtr++), compare 15<20 → room=2
✅ Output: 2

**🚆 2️⃣ Minimum Number of Platforms (Train Scheduling)**
📄 Problem Statement

Given arrival and departure times of trains,
find the minimum number of platforms required so that no train has to wait.

💡 Intuition

Each arrival before a departure means a train is at the station → one more platform.
Departure frees a platform.

💻 Java Solution
class GFG {
    static int findPlatform(int arr[], int dep[], int n) {
        Arrays.sort(arr);
        Arrays.sort(dep);

        int platNeeded = 1, maxPlat = 1;
        int i = 1, j = 0;

        while (i < n && j < n) {
            if (arr[i] <= dep[j]) {   // overlap → new platform
                platNeeded++;
                i++;
                maxPlat = Math.max(maxPlat, platNeeded);
            } else {                  // train departed → free platform
                platNeeded--;
                j++;
            }
        }
        return maxPlat;
    }
}

💡 Example
arr = [900, 940, 950, 1100, 1500, 1800]
dep = [910, 1200, 1120, 1130, 1900, 2000]
At 950 → overlap → platforms = 3
✅ Output: 3

**🚗 3️⃣ Car Pooling (LeetCode 1094)**
📄 Problem Statement

You are given a list of trips where trip[i] = [numPassengers, from, to].
Each trip has numPassengers people picked up at from and dropped at to.
Given car capacity, return true if it’s possible to complete all trips without exceeding capacity at any point.

💡 Intuition

Use a difference array:

+numPassengers at from

−numPassengers at to
Then do a prefix sum to track current load.

If at any time load > capacity → not possible.

💻 Java Solution
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        int[] diff = new int[1001]; // given: 0 ≤ location ≤ 1000

        for (int[] t : trips) {
            diff[t[1]] += t[0];   // pickup
            diff[t[2]] -= t[0];   // drop
        }

        int curr = 0;
        for (int i = 0; i < 1001; i++) {
            curr += diff[i];
            if (curr > capacity) return false;
        }
        return true;
    }
}

💡 Example
Trips = [[2,1,5],[3,3,7]], capacity = 4
Index changes:
+2 at 1, -2 at 5
+3 at 3, -3 at 7
Prefix sum: 0,2,2,5,5,3,3,0
At i=3, load=5 > 4 → false
✅ Output: false





🗓️ Problem: MyCalendar (No Double Booking)
Goal

Design a class to manage calendar bookings without overlap (no two events share any time).

Intervals are half-open → [start, end)
(i.e., end is exclusive, so [10, 20) and [20, 30) do not overlap).

Intuition

Each event = an interval on the timeline.
We can only add a new interval if it does not overlap with any already booked interval.

**Two intervals [s1, e1) and [s2, e2) overlap if:**

**max(s1, s2) < min(e1, e2)**




TreeMap (Efficient)

We can optimize overlap check using a sorted map (like TreeMap).

Logic

Store intervals keyed by start.

To check overlap:

Find the previous event (largest start ≤ current start)

Find the next event (smallest start ≥ current start)

Check overlap only with those two neighbors — no need to scan all.

Code (Java)
import java.util.*;

class MyCalendar {
    TreeMap<Integer, Integer> calendar;

    public MyCalendar() {
        calendar = new TreeMap<>();
    }

    public boolean book(int start, int end) {
        Integer prev = calendar.floorKey(start);
        Integer next = calendar.ceilingKey(start);

        if ((prev != null && calendar.get(prev) > start) ||
            (next != null && next < end)) {
            return false; // overlap found
        }

        calendar.put(start, end);
        return true;
    }
}

Time Complexity

Each insert/find operation: O(log N)

Total: O(N log N) for N bookings
Efficient and optimal

Space Complexity

O(N)

Example Walkthrough
["MyCalendar", "book", "book", "book"]
[[], [10,20], [15,25], [20,30]]

Step	Book	Booked Intervals	Check	Result
1	[10,20)	[] → add	no conflict	✅ true
2	[15,25)	compare with [10,20): overlap (15<20)	❌ false	
3	[20,30)	compare with [10,20): 20==20 no overlap	✅ true	

✅ Final output → [null, true, false, true]


**Calendar II**
**Problem Statement**

**You are designing a calendar where:**

**Events can overlap once (double booking allowed)**

**But no time should be triple-booked**

Implement class:

MyCalendarTwo()          // initializes calendar
boolean book(int start, int end)


→ Returns true if the event can be booked without causing a triple overlap,
→ else false (and do not add the event).

Example
Input:
["MyCalendarTwo","book","book","book","book","book","book"]
[[], [10,20],[50,60],[10,40],[5,15],[5,10],[25,55]]

Output:
[null,true,true,true,false,true,true]

Step-by-step
Booking	Existing events	Result	Reason
[10,20)	none	✅	no conflict
[50,60)	[10,20)	✅	separate
[10,40)	[10,20) overlaps partially	✅ double allowed	
[5,15)	overlaps with [10,20) and [10,40) → triple (10–15)	❌ reject	
[5,10)	disjoint	✅	
[25,55)	overlaps with [10,40) and [50,60) separately but never triple	✅	
Intuition

We must track two levels of coverage:

Single bookings → all intervals added so far

Double bookings → intervals where two bookings overlap

When booking a new event:

Check if it overlaps with any double booking → if yes → triple overlap → ❌ reject

Else →

For every overlap with existing single booking → record that overlap in double bookings

Then add the event to single bookings

Approach — Two Lists (Greedy & Simple)
Logic

Maintain:

booked → list of all single-booked intervals

overlaps → list of double-booked intervals

When booking [s, e):

Check for triple booking: if overlaps with any interval in overlaps, reject.

For each event (bStart, bEnd) in booked:

If overlap exists (max(s,bStart) < min(e,bEnd)),
add that overlap [max(s,bStart), min(e,bEnd)) to overlaps.

Add [s,e) to booked.

Code (Java)
class MyCalendarTwo {
    List<int[]> booked;
    List<int[]> overlaps;

    public MyCalendarTwo() {
        booked = new ArrayList<>();
        overlaps = new ArrayList<>();
    }

    public boolean book(int start, int end) {
        // Step 1: check if this causes a triple booking
        for (int[] o : overlaps) {
            if (Math.max(o[0], start) < Math.min(o[1], end))
                return false; // triple overlap
        }

        // Step 2: add overlapping parts to overlaps list
        for (int[] b : booked) {
            if (Math.max(b[0], start) < Math.min(b[1], end)) {
                int overlapStart = Math.max(b[0], start);
                int overlapEnd = Math.min(b[1], end);
                overlaps.add(new int[]{overlapStart, overlapEnd});
            }
        }

        // Step 3: add event to booked list
        booked.add(new int[]{start, end});
        return true;
    }
}



Time Complexity

Each booking checks overlaps with all previous bookings → O(N²) worst case.
(Fine since N ≤ 1000 for this problem)

Space: O(N) (store booked + overlap intervals)




**Problem: MyCalendar III — Maximum Overlaps Tracker**
Problem Statement

You are designing a calendar that allows any number of bookings.
Each call to book(start, end) adds an event and must return the maximum number of overlapping events so far.

You don’t reject bookings; you just track how high the overlap stack goes.

Example
Input:
["MyCalendarThree","book","book","book","book","book","book"]
[[], [10,20],[50,60],[10,40],[5,15],[5,10],[25,55]]

Output:
[null,1,1,2,3,3,3]

Step-by-step
Event	Timeline Impact	Max Overlap So Far
[10,20)	one booking	1
[50,60)	separate	1
[10,40)	overlaps [10,20) → [10,20) double	2
[5,15)	overlaps both [10,20) and [10,40) → triple 10–15	3
[5,10)	ends before triple zone	still 3
[25,55)	overlaps some but ≤3	3
Core Idea (Intuition)

This is a sweep-line or difference-array problem on a time axis.

Each event adds +1 to coverage starting at start

and −1 at end

Then, as we move through time in order, the prefix sum gives the active overlap count.

The maximum prefix sum seen so far = the current maximum overlaps.

**Approach 1 — TreeMap Sweep Line (Optimal & Simple)****
Logic

**Use TreeMap<Integer, Integer> to store these +1/−1 “delta” points.**

For each booking:

**Do map.put(start, map.getOrDefault(start, 0) + 1)**

**Do map.put(end, map.getOrDefault(end, 0) - 1)**

**Traverse the map in order, maintain running sum active += delta**

****Track maxActive = max(maxActive, active)**

Return maxActive

Code (Java)
import java.util.*;

class MyCalendarThree {
    private TreeMap<Integer, Integer> timeline;
    private int maxActive;

    public MyCalendarThree() {
        timeline = new TreeMap<>();
        maxActive = 0;
    }

    public int book(int start, int end) {
        timeline.put(start, timeline.getOrDefault(start, 0) + 1);
        timeline.put(end, timeline.getOrDefault(end, 0) - 1);

        int active = 0;
        for (int delta : timeline.values()) {
            active += delta;
            maxActive = Math.max(maxActive, active);
        }
        return maxActive;
    }
}



Each booking:

Do timeline.put(start, +1) and timeline.put(end, −1) → O(log N) each

Traverse all timeline.values() to compute prefix sum → O(N)

Total per booking

→ O(N + log N) ≈ O(N)

For N bookings

→ O(N²) overall





