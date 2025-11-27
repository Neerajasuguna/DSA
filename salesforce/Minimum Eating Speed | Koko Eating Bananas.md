 Minimum Eating Speed | Koko Eating Bananas
🧩 Problem Statement

Koko loves bananas and wants to eat all the bananas within h hours.

Given:

piles[] where piles[i] = number of bananas in pile i

integer h (number of hours)

Each hour, Koko eats k bananas from a single pile.

✅ Find the minimum integer eating speed k so she can finish all bananas within h hours.

✅ Example
Input:
piles = [3,6,7,11]
h = 8

Output:
4

🧠 Key Insight (Binary Search on Answer)

Minimum speed = 1

Maximum speed = max(piles)

If Koko can eat all bananas at speed k, she can do so at any k' > k

This monotonic behavior enables binary search

✅ Time Calculation Formula

For each pile:

hours += ceil(pile / k)


Implemented using:

(pile + k - 1) / k
 Java Implementation
 
    class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int left = 1;
        int right = 0;

        for (int pile : piles) {
            right = Math.max(right, pile);
        }

        int ans = right;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (canFinish(piles, h, mid)) {
                ans = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return ans;
    }

    private boolean canFinish(int[] piles, int h, int k) {
        int hours = 0;
        for (int pile : piles) {
            hours += (pile + k - 1) / k;
        }
        return hours <= h;
    }
}

⏱️ Complexity Analysis
Metric	Value
Time	O(n log M)
Space	O(1)

Where:

n = number of piles

M = maximum pile size

📝 Notes

Uses Binary Search on Answer

(pile + k - 1) / k ensures correct ceiling division

Common interview favorite due to monotonic condition
