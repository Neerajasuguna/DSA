# Valid Square

## Problem

Given four points in a 2D plane, determine whether they form a **valid square**.

Each point is represented as:

```text
[x, y]
```

A square has:

- 4 equal sides
- 2 equal diagonals
- Side length must be greater than `0`

We can determine this using the **squared distance** between every pair of points.

---

## Examples

### Example 1

```text
Input:
p1 = [0, 0]
p2 = [1, 1]
p3 = [1, 0]
p4 = [0, 1]

Output:
true
```

These four points form a square.

---

### Example 2

```text
Input:
p1 = [0, 0]
p2 = [1, 0]
p3 = [1, 1]
p4 = [2, 1]

Output:
false
```

The four points do not form a square because all four sides are not equal.

---

## Approach

There are **4 points**, so there are:

```text
4 × 3 / 2 = 6
```

unique pairs of points.

We calculate the squared distance for all 6 pairs.

For a valid square, after sorting these 6 distances:

```text
side², side², side², side², diagonal², diagonal²
```

Therefore:

```text
d[0] = d[1] = d[2] = d[3]
```

and

```text
d[4] = d[5]
```

Also, the smallest distance must be greater than `0`.

The `> 0` check is important because if two points are the same, their distance is `0`. Such points cannot form a valid square.

---

## Why Squared Distance?

The normal Euclidean distance formula is:

```text
distance = √((x1 - x2)² + (y1 - y2)²)
```

We don't need the actual distance.

Instead, we use:

```text
distance² = (x1 - x2)² + (y1 - y2)²
```

This avoids the unnecessary `sqrt()` operation.

For example:

```text
p1 = [0, 0]
p2 = [1, 0]
```

The squared distance is:

```text
(1 - 0)² + (0 - 0)²
= 1
```

For the diagonal:

```text
p1 = [0, 0]
p3 = [1, 1]
```

we get:

```text
(1 - 0)² + (1 - 0)²
= 2
```

So for this square:

```text
Sides:    1, 1, 1, 1
Diagonals: 2, 2
```

---

## Step-by-Step Example

Consider:

```text
p1 = [0, 0]
p2 = [1, 0]
p3 = [1, 1]
p4 = [0, 1]
```

Calculate all 6 distances:

```text
p1 → p2 = 1
p1 → p3 = 2
p1 → p4 = 1

p2 → p3 = 1
p2 → p4 = 2

p3 → p4 = 1
```

So:

```text
distances = [1, 2, 1, 1, 2, 1]
```

After sorting:

```text
[1, 1, 1, 1, 2, 2]
```

Now check:

```text
d[0] == d[1] == d[2] == d[3]
```

```text
1 == 1 == 1 == 1
```

and:

```text
d[4] == d[5]
```

```text
2 == 2
```

Since `d[0] > 0`, the points form a valid square.

---

## Algorithm

```text
1. Calculate the squared distance between every pair of points.
2. Store the 6 distances in an array.
3. Sort the distances.
4. Check that the four smallest distances are equal.
5. Check that the two largest distances are equal.
6. Check that the smallest distance is greater than 0.
7. If all conditions are satisfied, return true.
8. Otherwise, return false.
```

---

## Java Solution

```java
import java.util.Arrays;

public class Solution {

    public static int distanceSquared(int[] point1, int[] point2) {
        return (point1[0] - point2[0]) * (point1[0] - point2[0]) +
               (point1[1] - point2[1]) * (point1[1] - point2[1]);
    }

    public static boolean validSquare(
            int[] p1,
            int[] p2,
            int[] p3,
            int[] p4) {

        int[] distances = {
            distanceSquared(p1, p2),
            distanceSquared(p1, p3),
            distanceSquared(p1, p4),
            distanceSquared(p2, p3),
            distanceSquared(p2, p4),
            distanceSquared(p3, p4)
        };

        Arrays.sort(distances);

        return distances[0] > 0 &&
               distances[0] == distances[1] &&
               distances[1] == distances[2] &&
               distances[2] == distances[3] &&
               distances[4] == distances[5];
    }
}
```

---

## Understanding the Final Condition

The most important part of the solution is:

```java
return distances[0] > 0 &&
       distances[0] == distances[1] &&
       distances[1] == distances[2] &&
       distances[2] == distances[3] &&
       distances[4] == distances[5];
```

After sorting, we expect:

```text
Index:       0  1  2  3  4  5
             ↓  ↓  ↓  ↓  ↓  ↓
Expected:   S  S  S  S  D  D
```

Where:

```text
S = side²
D = diagonal²
```

Therefore:

```text
S = S = S = S
```

and:

```text
D = D
```

We don't explicitly check:

```text
D > S
```

because for four points satisfying the four equal side distances and two equal remaining distances, the diagonal is necessarily larger than the side for a non-degenerate square.

---

## Why Do We Need `distances[0] > 0`?

Consider:

```text
p1 = [0, 0]
p2 = [0, 0]
```

Then:

```text
distanceSquared(p1, p2) = 0
```

The sorted distances could potentially contain four equal zeros, but these points clearly cannot form a square.

Therefore:

```java
distances[0] > 0
```

ensures that no two points that are supposed to be distinct occupy the same position.

---

## Time Complexity

There are always exactly **6 distances** to calculate.

Sorting 6 elements technically takes:

```text
O(6 log 6)
```

which is constant.

Therefore, the overall complexity is:

```text
Time:  O(1)
Space: O(1)
```

---

## Key Interview Takeaways

- Four points produce exactly **6 unique pairs**.
- A square has **4 equal sides** and **2 equal diagonals**.
- Calculate **squared distances** instead of actual distances.
- Sorting makes it easy to identify the 4 sides and 2 diagonals.
- The expected sorted pattern is:

```text
[S, S, S, S, D, D]
```

- `S > 0` is necessary to prevent duplicate points.
- No `sqrt()` is required.
- Since the number of points is fixed, both time and space complexity are **O(1)**.
