An **axis-aligned rectangle** is represented by four values  
[x1, y1, x2, y2], where **(x1, y1)** is the bottom-left corner and **(x2, y2)** is the top-right corner. The rectangle’s sides are parallel to the X-axis and Y-axis.

Two rectangles are considered to **overlap** only if their intersection has a **positive area**. If they touch only at an edge or a corner, they are **not overlapping**.

The key idea is that two rectangles **do not overlap** if one of them is completely separated from the other along either the X-axis or the Y-axis. There are four such non-overlapping scenarios: one rectangle lies entirely to the left, right, above, or below the other. If any of these conditions is true, the rectangles do not overlap. Otherwise, they must overlap.

Mathematically, let:
- rec1 = [x1, y1, x2, y2]
- rec2 = [x3, y3, x4, y4]

The rectangles do **not overlap** if:
- x2 ≤ x3 (rec1 is to the left of rec2), or  
- x1 ≥ x4 (rec1 is to the right of rec2), or  
- y2 ≤ y3 (rec1 is below rec2), or  
- y1 ≥ y4 (rec1 is above rec2)

If none of the above conditions holds, the rectangles overlap and form a region with positive area.

Below is a simple and efficient Java implementation based on this logic:

```java
class Solution {
    public boolean isRectangleOverlap(int[] rec1, int[] rec2) {
        int x1 = rec1[0], y1 = rec1[1], x2 = rec1[2], y2 = rec1[3];
        int x3 = rec2[0], y3 = rec2[1], x4 = rec2[2], y4 = rec2[3];

        // Check non-overlapping conditions
        if (x2 <= x3 || x1 >= x4 || y2 <= y3 || y1 >= y4) {
            return false;
        }

        return true;
    }
}




 ----
This solution runs in O(1) time and uses O(1) extra space, making it optimal for checking rectangle overlap.
