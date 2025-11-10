
## 🧩 Problem: Maximum Points You Can Obtain from Cards

*(LeetCode 1423)*

You are given an integer array `cardPoints` where each element represents the number of points associated with a card.

In one step, you can take **one card from either the beginning or the end** of the row.
You have to take exactly **k** cards in total.

Your score is the sum of the points of the cards you have taken.
Return the **maximum score** you can obtain.

---

### Example 1

```
Input: cardPoints = [1,2,3,4,5,6,1], k = 3
Output: 12
Explanation:
Take the three cards on the right (6 + 5 + 1 = 12).
```

### Example 2

```
Input: cardPoints = [2,2,2], k = 2
Output: 4
Explanation:
No matter which cards you pick, the sum is always 4.
```

### Example 3

```
Input: cardPoints = [9,7,7,9,7,7,9], k = 7
Output: 55
Explanation:
You have to take all the cards, sum = 55.
```

---

## 💡 Intuition

If you take exactly `k` cards from either end,
it’s equivalent to **leaving behind `n - k` cards** (a continuous block in the middle).

To maximize your total score:

* Maximize the sum of the cards you take
  → same as minimizing the sum of the cards you **don’t take**.

So:

```
maxScore = totalSum(cardPoints) - minSum(subarray of length n - k)
```

We can find that minimum-sum subarray using a **sliding window** in O(n).

---

## 🧠 Approach (Sliding Window)

1. Compute total sum of all cards.
2. Compute the sum of the first `n - k` elements → this is our first “window”.
3. Slide the window through the array while updating its sum and tracking the **minimum** window sum.
4. Answer = `totalSum - minWindowSum`.

---

## ✅ Java Solution (O(n) Time, O(1) Space)

```java
import java.util.*;

public class Solution {
    public int maxScore(int[] cardPoints, int k) {
        int n = cardPoints.length;
        if (k >= n) {
            int total = 0;
            for (int v : cardPoints) total += v;
            return total;
        }

        int windowSize = n - k;
        int total = 0;
        for (int v : cardPoints) total += v;

        int curr = 0;
        for (int i = 0; i < windowSize; i++) curr += cardPoints[i];
        int minWindow = curr;

        for (int i = windowSize; i < n; i++) {
            curr += cardPoints[i];
            curr -= cardPoints[i - windowSize];
            minWindow = Math.min(minWindow, curr);
        }

        return total - minWindow;
    }

    public static void main(String[] args) {
        Solution sol = new Solution();
        System.out.println(sol.maxScore(new int[]{1,2,3,4,5,6,1}, 3)); // 12
        System.out.println(sol.maxScore(new int[]{2,2,2}, 2));         // 4
        System.out.println(sol.maxScore(new int[]{9,7,7,9,7,7,9}, 7)); // 55
    }
}
```

---

## 🕒 Complexity Analysis

| Operation | Complexity             |
| --------- | ---------------------- |
| Time      | **O(n)** (single pass) |
| Space     | **O(1)**               |

---

## ✅ Summary

| Concept          | Description                                                        |
| ---------------- | ------------------------------------------------------------------ |
| Goal             | Maximize score from `k` cards (either end)                         |
| Trick            | Equivalent to removing a subarray of size `n - k` with minimum sum |
| Technique        | Sliding window                                                     |
| Time Complexity  | O(n)                                                               |
| Space Complexity | O(1)                                                               |


