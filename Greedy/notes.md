

Greedy is an algorithmic paradigm that **builds up a solution piece by piece.** It makes **a series of choices, each time picking the option that seems best at the moment**, the most greedy choice, 
with the goal of finding an overall optimal solution. 



************When to use Greedy Approach ****************

Yes, if both of these conditions are fulfilled:

Optimization problem: The problem is an optimization problem, where we are looking to find the best solution under a given set of constraints. This could involve minimizing or maximizing some value, such as cost, distance, time, or profit.

Making local choices leads to a global solution: The problem can be solved by making simple decisions based on the current option or state without needing to look ahead or consider many future possibilities.

No, if any of these conditions is fulfilled:

Local choices lead to sub-optimal solutions: Our analysis shows that making local greedy choices leads us to a sub-optimal solution.
Problem lacks clear local optima: If the problem doesn’t naturally break down into a series of choices where we can identify the best option at each step, a greedy algorithm might not be applicable.
