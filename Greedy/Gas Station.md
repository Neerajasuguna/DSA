Gas Stations

There are 
n gas stations along a circular route, where the amount of gas at the i th station is gas[i].

We have a car with an unlimited gas tank, and it costs cost[i] of gas to travel from the ith station to the next (i+1) th
 station. We begin the journey with an empty tank at one of the gas stations.

Find the index of the gas station in the integer array gas such that if we start from that index we may return to the same index by traversing through all the elements, collecting gas[i] and consuming cost[i].

If it is not possible, return -1.

If there exists such index, it is guaranteed to be unique.







**Solution summary**


If the sum of all values in the gas array is less than the sum of all values in the cost array, we return -1, because there will be no gas station from which we can start our journey to complete the round trip.

**Traverse the gas list with startingIndex initially set to 0.**

****We calculate currentGas left by incrementing currentGas (initially 0 at the starting point) to (gas[i] - cost[i]).**

**If at any point, our currentGas is less than 0, it means we can not start from this index. Therefore, we increment startingIndex to i+1 and reset currentGas to 0.**

**Return startingIndex at the end of the traversal.****

Time complexity
The time complexity is 
O(n)
, since there are n elements in the array and we only visit each element once.

Space complexity
The space complexity is 
O(1)
, since we don’t use any additional data structures.





**
**In the Gas Station problem, once you find a starting index where the current tank (currentGas) never goes negative while traversing forward, 
you don’t need to explicitly make a circular traversal again. This is because:**

**The problem guarantees that if a solution exists, it is unique.
When you reset the start index after the tank goes negative, you effectively discard all stations before the new start because they cannot be starting points**.**
By the time you reach the end of the array, if the total gas is at least the total cost, the start index you have is the valid starting point.
Thus, the single forward pass with resetting start and tank is sufficient to confirm the unique valid start without looping back explicitly.



public static int gasStationJourney(int[] gas, int[] cost) {
        
        if (Arrays.stream(cost).sum() > Arrays.stream(gas).sum()) {
            return -1;
        }

        int currentGas = 0;
        int startingIndex = 0;

        for (int i = 0; i < gas.length; i++) {
            currentGas += gas[i] - cost[i];

            if (currentGas < 0) {
                currentGas = 0;
                startingIndex = i + 1;
            }
        }

        return startingIndex;
    }







