Given an array of integers nums and an integer limit, return the size of the longest non-empty subarray such that the absolute difference between any two elements of this subarray is less than or equal to limit.

 

**Example 1:**

Input: nums = [8,2,4,7], limit = 4
Output: 2 
Explanation: All subarrays are: 
[8] with maximum absolute diff |8-8| = 0 <= 4.
[8,2] with maximum absolute diff |8-2| = 6 > 4. 
[8,2,4] with maximum absolute diff |8-2| = 6 > 4.
[8,2,4,7] with maximum absolute diff |8-2| = 6 > 4.
[2] with maximum absolute diff |2-2| = 0 <= 4.
[2,4] with maximum absolute diff |2-4| = 2 <= 4.
[2,4,7] with maximum absolute diff |2-7| = 5 > 4.
[4] with maximum absolute diff |4-4| = 0 <= 4.
[4,7] with maximum absolute diff |4-7| = 3 <= 4.
[7] with maximum absolute diff |7-7| = 0 <= 4. 
Therefore, the size of the longest subarray is 2.
Example 2:

Input: nums = [10,1,2,4,7,2], limit = 5
Output: 4 
Explanation: The subarray [2,4,7,2] is the longest since the maximum absolute diff is |2-7| = 5 <= 5.
Example 3:

Input: nums = [4,2,2,2,4,4,2,2], limit = 0
Output: 3




Approach - 

## 🧠 Algorithm: Longest Subarray Within Limit

### Initialization
- Initialize two heaps:
  - `maxHeap` → to track the maximum element in the current window.
  - `minHeap` → to track the minimum element in the current window.
- Initialize `left = 0` to represent the start of the sliding window.
- Initialize `maxLength = 0` to store the length of the longest valid subarray.

---

### Iteration
Iterate through the array `nums` from left to right using a pointer `right`:

#### Step 1: Add current element
- Add `nums[right]` and its index to both `maxHeap` and `minHeap`.

#### Step 2: Check window validity
- While the absolute difference between the maximum value (from `maxHeap`)  
  and the minimum value (from `minHeap`) is greater than `limit`:
  - The window is invalid.
  - Move the `left` pointer to the right to shrink the window.
  - Specifically, set `left` to one more than the **smaller index**  
    between the top elements of `maxHeap` and `minHeap`.

#### Step 3: Remove out-of-window elements
- While the index of the top element in `maxHeap` is less than `left`, pop it.
- While the index of the top element in `minHeap` is less than `left`, pop it.

#### Step 4: Update result
- Update `maxLength` as:
  maxLength = max(maxLength, right - left + 1)

  Final Step
- After iterating through all elements, return `maxLength`.

---

### 🧩 Summary
This algorithm maintains a sliding window where the difference between  
the maximum and minimum elements does not exceed `limit`.  
Using two heaps ensures that the maximum and minimum of the window  
can be accessed in **O(1)** time and updated in **O(log n)** time.

**Time Complexity:** `O(n log n)`  
**Space Complexity:** `O(n)`


class Solution {

    public int longestSubarray(int[] nums, int limit) {
        PriorityQueue<int[]> maxHeap = new PriorityQueue<>(
            (a, b) -> b[0] - a[0]
        );
        PriorityQueue<int[]> minHeap = new PriorityQueue<>(
            Comparator.comparingInt(a -> a[0])
        );

        int left = 0, maxLength = 0;

        for (int right = 0; right < nums.length; ++right) {
            maxHeap.offer(new int[] { nums[right], right });
            minHeap.offer(new int[] { nums[right], right });

            // Check if the absolute difference between the maximum and minimum values in the current window exceeds the limit
            while (maxHeap.peek()[0] - minHeap.peek()[0] > limit) {
                // Move the left pointer to the right until the condition is satisfied.
                // This ensures we remove the element causing the violation
                left = Math.min(maxHeap.peek()[1], minHeap.peek()[1]) + 1;

                // Remove elements from the heaps that are outside the current window
                while (maxHeap.peek()[1] < left) {
                    maxHeap.poll();
                }
                while (minHeap.peek()[1] < left) {
                    minHeap.poll();
                }
            }

            // Update maxLength with the length of the current valid window
            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}




Approah 2 




**We’ll maintain two monotonic deques:**

maxDeque → stores elements in decreasing order (front = largest)

minDeque → stores elements in increasing order (front = smallest)

Then we expand the window (move right) and shrink from left if condition breaks.

**🪄 Algorithm Steps**

Initialize left = 0, right = 0, maxLen = 0.

For each new element nums[right]:

Update both deques:

Remove smaller elements from maxDeque’s back → maintain decreasing order.

Remove larger elements from minDeque’s back → maintain increasing order.

Add nums[right] to both.

If the current window violates the condition:

maxDeque.peekFirst() - minDeque.peekFirst() > limit


→ shrink window by moving left forward and removing nums[left] from deques if needed.

Update maxLen with right - left + 1.


import java.util.*;

class Solution {
    public int longestSubarray(int[] nums, int limit) {
        Deque<Integer> maxDeque = new ArrayDeque<>();
        Deque<Integer> minDeque = new ArrayDeque<>();
        int left = 0, maxLen = 0;

        for (int right = 0; right < nums.length; right++) {
            int num = nums[right];

            // maintain decreasing deque for max
            while (!maxDeque.isEmpty() && nums[maxDeque.peekLast()] < num)
                maxDeque.pollLast();
            maxDeque.offerLast(right);

            // maintain increasing deque for min
            while (!minDeque.isEmpty() && nums[minDeque.peekLast()] > num)
                minDeque.pollLast();
            minDeque.offerLast(right);

            // shrink window if condition breaks
            while (nums[maxDeque.peekFirst()] - nums[minDeque.peekFirst()] > limit) {
                left++;
                if (maxDeque.peekFirst() < left) maxDeque.pollFirst();
                if (minDeque.peekFirst() < left) minDeque.pollFirst();
            }

            maxLen = Math.max(maxLen, right - left + 1);
        }

        return maxLen;
    }
}



Time Complexity - O(n)
Space COmplexity - O(n)



