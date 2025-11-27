📌 Minimum Length Subarray with K Unique Elements (Indices)
🧩 Problem Statement

Given an integer array nums and an integer k, find the minimum length subarray that contains exactly k unique elements.

Return the starting and ending indices of such a subarray.
If no such subarray exists, return [-1, -1].

✅ Example
Input:
nums = [2, 1, 2, 3, 1, 2, 3]
k = 3

Output:
[1, 3]   // subarray [1, 2, 3]

🧠 Approach (Sliding Window)

Use two pointers (left, right)

Maintain a frequency map for elements in the window

Expand window by moving right

Shrink window when unique count exceeds k

When window has exactly k unique elements, update minimum window

✅ Java Implementation
import java.util.*;

public class MinSubarrayKDistinct {

    public static int[] minSubarrayIndices(int[] nums, int k) {
        Map<Integer, Integer> freqMap = new HashMap<>();
        int left = 0, minLen = Integer.MAX_VALUE;
        int start = -1, end = -1;

        for (int right = 0; right < nums.length; right++) {
            freqMap.put(nums[right], freqMap.getOrDefault(nums[right], 0) + 1);

            while (freqMap.size() > k) {
                int leftVal = nums[left];
                freqMap.put(leftVal, freqMap.get(leftVal) - 1);
                if (freqMap.get(leftVal) == 0) {
                    freqMap.remove(leftVal);
                }
                left++;
            }

            if (freqMap.size() == k && right - left + 1 < minLen) {
                minLen = right - left + 1;
                start = left;
                end = right;
            }
        }

        return new int[]{start, end};
    }
}

⏱️ Complexity Analysis
Metric	Value
Time	O(n)
Space	O(k)
📝 Notes

Works only for exactly k unique elements

Sliding window ensures optimal performance

Indices allow direct subarray extraction
