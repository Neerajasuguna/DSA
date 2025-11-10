**Problem: MyCalendar II — Allow Double Booking but No Triple**
Problem Statement

You are designing a calendar where:

**Events can overlap once (double booking allowed)**

But no time should be triple-booked

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


**Intuition**

**We must track two levels of coverage:**

**Single bookings → all intervals added so far**

**Double bookings → intervals where two bookings overlap**

**When booking a new event:**

**Check if it overlaps with any double booking → if yes → triple overlap → ❌ reject**

**Else →**

**For every overlap with existing single booking → record that overlap in double bookings**

**Then add the event to single bookings**

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

Dry Run
Step	Book	Overlaps found	Added to overlaps	Result
[10,20)	none	none	none	✅
[50,60)	none	none	✅	
[10,40)	overlap with [10,20) → [10,20)	add [10,20) to overlaps	✅	
[5,15)	overlap with overlaps [10,20) → triple in [10,15)	❌		
[25,55)	overlap with [10,40) → [25,40), overlap with [50,60) → [50,55)	add those two	✅	
Time Complexity

Each booking checks overlaps with all previous bookings → O(N²) worst case.
(Fine since N ≤ 1000 for this problem)

Space: O(N) (store booked + overlap intervals)
