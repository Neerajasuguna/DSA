There are n cities connected by some number of flights. You are given an array flights where flights[i] = [fromi, toi, pricei] indicates that there is a flight from city fromi to city toi with cost pricei.

You are also given three integers src, dst, and k, return the cheapest price from src to dst with at most k stops. If there is no such route, return -1.

 

Example 1:


Input: n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]], src = 0, dst = 3, k = 1
Output: 700
Explanation:
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 3 is marked in red and has cost 100 + 600 = 700.
Note that the path through cities [0,1,2,3] is cheaper but is invalid because it uses 2 stops.
Example 2:


Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
Output: 200
Explanation:
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 2 is marked in red and has cost 100 + 100 = 200.
Example 3:


Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0
Output: 500
Explanation:
The graph is shown above.
The optimal path with no stops from city 0 to 2 is marked in red and has cost 500.
 

Constraints:

2 <= n <= 100
0 <= flights.length <= (n * (n - 1) / 2)
flights[i].length == 3
0 <= fromi, toi < n
fromi != toi
1 <= pricei <= 104
There will not be any multiple flights between two cities.
0 <= src, dst, k < n
src != dst



We’re finding the shortest path (minimum cost) but limited to k+1 flights (edges).

Normal Dijkstra finds the cheapest path regardless of stops.
Here, we must ensure we don’t exceed k stops —
so we track (cost, city, stops) in the priority queue.

At each step:

If we’ve used ≤ k stops, we can continue.

Otherwise, skip that route.

This works like Dijkstra’s but with an extra dimension for “number of stops used”.

✅ Java Code (Dijkstra-like with K Stops Constraint)
import java.util.*;

class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        // Step 1: Build adjacency list
        Map<Integer, List<int[]>> graph = new HashMap<>();
        for (int[] flight : flights) {
            graph.computeIfAbsent(flight[0], x -> new ArrayList<>())
                 .add(new int[]{flight[1], flight[2]}); // to, price
        }

        // Step 2: Min-heap sorted by current total cost
        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        // Each element: [costSoFar, currentCity, stopsUsed]
        pq.offer(new int[]{0, src, 0});

        // Step 3: Distance array to track min cost for (city, stops)
        int[][] dist = new int[n][k + 2];
        for (int[] d : dist) Arrays.fill(d, Integer.MAX_VALUE);
        dist[src][0] = 0;

        // Step 4: Dijkstra with stop constraint
        while (!pq.isEmpty()) {
            int[] cur = pq.poll();
            int cost = cur[0], city = cur[1], stops = cur[2];

            if (city == dst) return cost; // found cheapest route

            if (stops > k) continue; // cannot take more than k stops

            if (!graph.containsKey(city)) continue;
            for (int[] next : graph.get(city)) {
                int nextCity = next[0], price = next[1];
                int newCost = cost + price;
                if (newCost < dist[nextCity][stops + 1]) {
                    dist[nextCity][stops + 1] = newCost;
                    pq.offer(new int[]{newCost, nextCity, stops + 1});
                }
            }
        }

        return -1;
    }

    // For testing
    public static void main(String[] args) {
        Solution sol = new Solution();
        int n = 4;
        int[][] flights = {
            {0, 1, 100},
            {1, 2, 100},
            {2, 0, 100},
            {1, 3, 600},
            {2, 3, 200}
        };
        int src = 0, dst = 3, k = 1;
        int result = sol.findCheapestPrice(n, flights, src, dst, k);
        System.out.println(result); // Output: 700
    }
}

🧠 Step-by-Step Example
Flights = [[0,1,100],[1,2,100],[1,3,600],[2,3,200]]
src=0, dst=3, k=1


Start: pq = [(0,0,0)]

Pop (0,0,0):
→ from 0 → 1 (100), push (100,1,1)

Pop (100,1,1):
→ from 1 → 3 (600), push (700,3,2)
→ from 1 → 2 (100), push (200,2,2)

Pop (200,2,2): stops > k → ignore

Pop (700,3,2): destination found ✅ → return 700.

🕒 Time and Space Complexity
Operation	Complexity
Build graph	O(E)
Dijkstra traversal	O(E log V)
Total Time	O(E log V)
Space	O(V × (K+1))






**Optimal is BellmanFord Algorithm**




Intuition

We can perform relaxation up to k + 1 times.
Each relaxation allows one more flight to be used.

Idea:

Initialize all distances as ∞ except src = 0.

Repeat k + 1 times:

Create a new temp[] array as a copy of dist[].

For every flight (u → v with price w):
If dist[u] + w < temp[v], update temp[v] = dist[u] + w.

After finishing this round, set dist = temp.

After k + 1 iterations, dist[dst] will contain the minimum cost using ≤ k + 1 flights.







import java.util.*;

class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        int[] dist = new int[n];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[src] = 0;

        // Relax up to k + 1 times (for k stops)
        for (int i = 0; i <= k; i++) {
            int[] temp = Arrays.copyOf(dist, n);

            for (int[] flight : flights) {
                int u = flight[0], v = flight[1], w = flight[2];
                if (dist[u] == Integer.MAX_VALUE) continue; // unreachable so far
                if (dist[u] + w < temp[v]) {
                    temp[v] = dist[u] + w;
                }
            }

            dist = temp;
        }

        return dist[dst] == Integer.MAX_VALUE ? -1 : dist[dst];
    }

    // Example usage
    public static void main(String[] args) {
        Solution sol = new Solution();
        int n = 4;
        int[][] flights = {
            {0, 1, 100},
            {1, 2, 100},
            {2, 0, 100},
            {1, 3, 600},
            {2, 3, 200}
        };
        int src = 0, dst = 3, k = 1;
        System.out.println(sol.findCheapestPrice(n, flights, src, dst, k)); // 700
    }
}




| Operation                 | Complexity                        |
| ------------------------- | --------------------------------- |
| Relax edges (k + 1 times) | O(k × E)                          |
| **Total Time**            | **O(k × E)**                      |
| **Space**                 | **O(V)** (two arrays: dist, temp) |



















