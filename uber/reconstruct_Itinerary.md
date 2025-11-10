You are given a list of airline tickets where tickets[i] = [fromi, toi] represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from "JFK", thus, the itinerary must begin with "JFK". If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

For example, the itinerary ["JFK", "LGA"] has a smaller lexical order than ["JFK", "LGB"].
You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

 

Example 1:


Input: tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
Output: ["JFK","MUC","LHR","SFO","SJC"]
Example 2:


Input: tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"] but it is larger in lexical order.
 

Constraints:

1 <= tickets.length <= 300
tickets[i].length == 2
fromi.length == 3
toi.length == 3
fromi and toi consist of uppercase English letters.
fromi != toi


Intuition 

**We need to find a path that uses all edges exactly once →
that’s an Eulerian Path in graph theory.**



Core Idea

We can reconstruct the itinerary using Hierholzer’s algorithm — used to find an Eulerian path.

Key idea:

**Always take the smallest lexicographical next destination first (so use a min-heap or sorted list).**

**Perform DFS until no more outgoing flights.**

**Then backtrack (add airport to result in reverse order).**

**⚙️ Steps**

Build adjacency list:

Map: from -> list of destinations

Sort destinations (lexicographically ascending).

DFS from "JFK":

While current node has destinations:

Take the smallest destination (lexicographically smallest first).

DFS on that destination.

Once no more outgoing edges → append current airport to result (postorder).

Reverse the result to get the final itinerary.





import java.util.*;

class Solution {
    Map<String, PriorityQueue<String>> graph = new HashMap<>();
    LinkedList<String> result = new LinkedList<>();

    public List<String> findItinerary(List<List<String>> tickets) {
        // Step 1: Build graph
        for (List<String> ticket : tickets) {
            String from = ticket.get(0);
            String to = ticket.get(1);
            graph.computeIfAbsent(from, k -> new PriorityQueue<>()).offer(to);
        }

        // Step 2: DFS from "JFK"
        dfs("JFK");

        // Step 3: result is built in reverse
        return result;
    }

    private void dfs(String airport) {
        PriorityQueue<String> dests = graph.get(airport);
        while (dests != null && !dests.isEmpty()) {
            dfs(dests.poll());
        }
        result.addFirst(airport);
    }
}


Why addFirst()?

We’re doing a postorder DFS — once an airport has no more outgoing flights,
we add it to the result.
This builds the itinerary in reverse, so addFirst() reverses it naturally.

⏱️ Time & Space Complexity

Time: O(E log E)

Sorting via PriorityQueue per edge.

Space: O(E) for adjacency list and recursion stack.

Where E = number of tickets.


