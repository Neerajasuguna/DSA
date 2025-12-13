You are given an array of 
n

 points with integer coordinates on a 2D plane, points, where points[i] = [xi, yi]. Your task is to determine the minimum time in seconds required to visit all the points in the given order.

Movement rules:

In one second, you can perform any one of the following:

Move vertically by one unit.

Move horizontally by one unit.

Move diagonally (1 unit vertically and 1 unit horizontally in 1 second).

You must visit the points in the exact sequence listed in the array.

You may pass through points that appear later in the order, but they will not count as visits.

***********Algorithm********


Core Insight (Very Important)

Because you can move diagonally (x + 1, y + 1) in 1 second, the time to go from one point to another is:

Maximum of horizontal distance and vertical distance

Mathematically, between two points
(x1, y1) → (x2, y2):

time = max(|x2 − x1|, |y2 − y1|)


************This is known as Chebyshev distance.************

🧠 Why this works (Intuition)

Diagonal moves reduce both x and y at the same time

You should use as many diagonal moves as possible

Once one direction finishes, continue with straight moves


********java Solution************
public static int minTimeToVisitAllPoints(int[][] points) {
        int result = 0;
        for (int i = 1; i < points.length; i++) {
            int xDiff = Math.abs(points[i][0] - points[i - 1][0]);
            int yDiff = Math.abs(points[i][1] - points[i - 1][1]);
            result += Math.max(xDiff, yDiff);
        }
        return result;
    }

********Follow Up **********

what if  only straight movemetnts are allowed and not the Diagonal Moves?

Key Insight (Straight movement only)

Since you cannot reduce x and y together, you must:

Finish horizontal movement

Finish vertical movement

So the time becomes the sum of distances, not the max.

📐 Distance Formula (Straight moves only)

From point
(x1, y1) → (x2, y2):

time = |x2 − x1| + |y2 − y1|


**********This is called Manhattan distance.***********
