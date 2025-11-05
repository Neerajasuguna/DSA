Two City Scheduling

Problem Statement:

A recruiter plans to hire n people and conducts their interviews at two different company locations: City A and City B. For each candidate i, 
you are given the cost of flying them to City A (costs[i][0]) and City B (costs[i][1]). **The recruiter wants to invite exactly n/2 candidates to City A and n/2 candidates to City B, 
minimizing the total cost of flying all candidates.**

Input:

An array costs where costs[i] = [aCost_i, bCost_i] represents the cost of sending the ith candidate to City A and City B respectively.
costs.length is even.
Output:

The minimum total cost to fly all candidates such that exactly half go to City A and half go to City B.



Consider the costs array:

[[100, 10], [1000, 10], [500, 50], [100, 1]]
We want to invite half the people to City A and the other half to City B.

The cost differences (aCost_i - bCost_i) for each candidate are:

[90, 990, 450, 99]
Sort the costs array by these differences in ascending order:

[[100, 10], [100, 1], [500, 50], [1000, 10]]
Now, invite the first half (2 candidates) to City A and the second half to City B:

City A: candidates 1 and 2 → costs = 100 + 100 = 200
City B: candidates 3 and 4 → costs = 50 + 10 = 60
Total minimum cost = 200 + 60 = 260



This greedy approach sorts candidates by the cost difference between City A and City B. 
Candidates with smaller differences (or negative differences) are cheaper to send to City A, and those with larger differences are cheaper to send to City B.

By sending the first half to City A and the second half to City B after sorting, we ensure the total cost is minimized.

This works because a large positive difference means City B is much cheaper, so those candidates go to City B, and vice versa.




****This greedy approach sorts candidates by the cost difference between City A and City B. Candidates with smaller differences (or negative differences) are cheaper to send to City A, and those with larger differences are cheaper to send to City B**.
**By sending the first half to City A and the second half to City B after sorting, we ensure the total cost is minimized.**

**This works because a large positive difference means City B is much cheaper, so those candidates go to City B, and vice versa.**




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






