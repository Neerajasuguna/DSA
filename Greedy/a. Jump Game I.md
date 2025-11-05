Statement
You are given an integer array nums, where each element represents the maximum number of steps you can move forward from that position. You always start at index 0
 (the first element), and at each step, you may jump to any of the next positions within the range allowed by the current element’s value. Return TRUE if you can reach the last index, or FALSE otherwise.

Input: nums = [3, 2, 1, 0, 4]
Output: FALSE

Explanation:
- Start at index 0 with value 3, you can jump to index 1, 2, or 3.
- At index 3, the value is 0, so you cannot move forward anymore.
- The last index (4) is unreachable, so the output is FALSE.


Input: nums = [2, 3, 1, 1, 4]
Output: TRUE

Explanation:
- Start at index 0 with value 2, so you can jump to index 1 or 2.
- Jump to index 1 (value 3), from here you can jump up to 3 steps ahead.
- From index 1, you can jump directly to the last index (index 4).
- Since you can reach the last index, the output is TRUE.




**Naive approach**
The naive approach **explores all possible jump sequences from the starting position to the end of the array**. It begins at the first index and attempts to jump to every reachable position from the current index, 
recursively repeating this process for each new position. If a path successfully reaches the last index, the algorithm returns success. If it reaches a position without further moves, it backtracks to try a different path.

While this method guarantees that all possible paths are considered, it is highly inefficient, as it explores many redundant or dead-end routes. The time complexity of this backtracking approach is exponential, 
making it impractical for large inputs.




**Optimized approach using the greedy pattern**
An optimized way to solve this problem is using a greedy approach that works in reverse. Instead of trying every possible forward jump, **we flip the logic: start from the end and ask, 
“Can we reach this position from any earlier index?”**

**We begin with the last index as our initial target—the position we want to reach. Then, we move backward through the array, checking whether the current index has a jump value large enough 
to reach or go beyond the current target. If it can, we update the target to that index. This means that reaching this earlier position would eventually allow us to reach the end. 
By continuously updating the target, we’re effectively identifying the leftmost position from which the end is reachable.**

This process continues until we reach the first index or determine that no earlier index can reach the current target. If we finish with the target at index 0

, it means the start of the array can lead to the end, so we return TRUE. If the target remains beyond index 0

, then no path exists from the start to the end, and we return FALSE.



**Solution summary**

Set the last index of the array as the target index.
Traverse the array backward and verify if we can reach the target index from any of the previous indexes.
If we can reach it, we update the target index with the index that allows us to jump to the target index.

We repeat this process until we’ve traversed the entire array.

Return TRUE if, through this process, we can reach the first index of the array. Otherwise, return FALSE.
Time complexity
The time complexity of the above solution is 
O(n)

, since we traverse the array only once, where n
 is the number of elements in the array.

Space complexity
The space complexity of the above solution is 
O(1)
, because we do not use any extra space.



