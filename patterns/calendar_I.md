Problem: MyCalendar (No Double Booking)
Goal

**Design a class to manage calendar bookings without overlap (no two events share any time).**

Intervals are half-open → [start, end)
(i.e., end is exclusive, so [10, 20) and [20, 30) do not overlap).

Intuition

**Each event = an interval on the timeline.
We can only add a new interval if it does not overlap with any already booked interval.**

**Two intervals [s1, e1) and [s2, e2) overlap if:**

**max(s1, s2) < min(e1, e2)**


So, to book a new interval [start, end):

Check every existing interval (s, e)

If it overlaps → return false

Else → insert and return true



**TreeMap (Efficient)**

We can optimize overlap check using a sorted map (like TreeMap).

**Logic**

**Store intervals keyed by start.**

**To check overlap:**

**Find the previous event (largest start ≤ current start)**

**Find the next event (smallest start ≥ current start)**

**Check overlap only with those two neighbors — no need to scan all.**

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
