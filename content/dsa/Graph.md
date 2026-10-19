---
share: true
---

### 1. Multi-Source, 0-1 & State-Space BFS (10 Problems)
Skip basic single-source grid BFS. Focus on pushing all sources into the queue at t = 0, using a Deque for 0/1 edge weights, and adding an extra dimension to visited[r][c][state].

1. [Rotting Oranges (LC 994)](https://leetcode.com/problems/rotting-oranges/) / [01 Matrix (LC 542)](https://leetcode.com/problems/01-matrix/) — Multi-source BFS initialization (pushing all starting points into the queue simultaneously before level 0).
2. [Word Ladder (LC 127)](https://leetcode.com/problems/word-ladder/) — Implicit graph BFS on strings (and know [Word Ladder II (LC 126)](https://leetcode.com/problems/word-ladder-ii/): BFS to build the shortest-path DAG + DFS backtracking to reconstruct paths without MLE).
3. [Shortest Bridge (LC 934)](https://leetcode.com/problems/shortest-bridge/) — Hybrid traversal: DFS to flood-fill and collect Island 1, followed immediately by Multi-Source BFS to reach Island 2.
4. [Shortest Path in a Grid with Obstacles Elimination (LC 1293)](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/) — 3D State-Space BFS: visited[r][c][k_remaining], plus the pruning trick that if seenK[r][c] >= current_k, you don't re-visit.
5. [Shortest Path to Get All Keys (LC 864)](https://leetcode.com/problems/shortest-path-to-get-all-keys/) — BFS + Bitmask State: dist[r][c][key_mask] where key_mask is a 6-bit integer (2^6 = 64). Huge OA staple.
6. [Minimum Cost to Make at Least One Valid Path in a Grid (LC 1368)](https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/) — 0-1 BFS (Deque): Free edges (following the arrow) go to deque.addFirst() (+0), paid edges go to deque.addLast() (+1). Runs in O(V + E) instead of Dijkstra's O(E \log V).
7. [Minimum Obstacle Removal to Reach Corner (LC 2290)](https://leetcode.com/problems/minimum-obstacle-removal-to-reach-corner/) — Purest 0-1 BFS grid application.
8. [Bus Routes (LC 815)](https://leetcode.com/problems/bus-routes/) — Bipartite Graph Modeling: Don't connect stops to stops (O(N^2) edges TLE); make the bus routes the BFS nodes instead.
9. [Open the Lock (LC 752)](https://leetcode.com/problems/open-the-lock/) / [Minimum Genetic Mutation (LC 433)](https://leetcode.com/problems/minimum-genetic-mutation/) — Bidirectional BFS (expanding from start and target simultaneously to cut O(b^d) search space to O(b^{d/2})).
10. [Snakes and Ladders (LC 909)](https://leetcode.com/problems/snakes-and-ladders/) / [Jump Game IV (LC 1345)](https://leetcode.com/problems/jump-game-iv/) — Clearing the adjacency list (graph.get(val).clear()) immediately after processing a value's first visit to prevent O(N^2) TLE on dense equal-value jumps.

---

### 2. DFS, Bipartition, Implicit Graphs & Functional Graphs (8 Problems)
Whenever a problem says "divide into two groups with no internal conflicts," think Bipartite 2-Coloring. Whenever every node has exactly 1 outgoing edge, it is a Functional Graph (a collection of jellyfish: directed trees rooted on a cycle).

1. [Is Graph Bipartite? (LC 785)](https://leetcode.com/problems/is-graph-bipartite/) — 2-coloring BFS/DFS (detecting odd-length cycles).
2. [Divide Nodes Into the Maximum Number of Groups (LC 2493)](https://leetcode.com/problems/divide-nodes-into-the-maximum-number-of-groups/) — OA Boss: Check Bipartite 2-coloring first (if odd cycle exists \rightarrow -1); if valid, run BFS from every node in each connected component to find the max component diameter.
3. [Evaluate Division (LC 399)](https://leetcode.com/problems/evaluate-division/) — Google Interview Classic: Modeling equations A / B = k as weighted directed edges (A \xrightarrow{k} B and B \xrightarrow{1/k} A) and finding path products.
4. [Accounts Merge (LC 721)](https://leetcode.com/problems/accounts-merge/) — Building equivalence classes on strings via DFS connected components or DSU.
5. [Find Eventual Safe States (LC 802)](https://leetcode.com/problems/find-eventual-safe-states/) — 3-color cycle detection (0 = unvisited, 1 = visiting/on-stack, 2 = safe) or Reverse Graph Kahn's Topological Sort.
6. [Longest Cycle in a Graph (LC 2360)](https://leetcode.com/problems/longest-cycle-in-a-graph/) — Functional Graph (Out-degree \le 1): Finding cycle lengths in O(N) time using entry timestamps timeVisited[u] without recursion stack overhead.
7. [Count Visited Nodes in a Directed Graph (LC 2876)](https://leetcode.com/problems/count-visited-nodes-in-a-directed-graph/) — Functional graph decomposition: solve nodes inside cycles first (answer = cycle length), then propagate backward up the attached trees.
8. [Clone Graph (LC 133)](https://leetcode.com/problems/clone-graph/) — Live interview pointer baseline (Map<Node, Node> deep copy invariant).

---

### 3. Topological Sort, Kahn's Algorithm & Dependency Graphs (8 Problems)
Anytime a problem involves prerequisites, lexicographically smallest valid ordering, alien dictionaries, or building a matrix from row/column rules, use Kahn’s Algorithm (In-degree array + Queue).

1. [Course Schedule II (LC 210)](https://leetcode.com/problems/course-schedule-ii/) — Standard Kahn's BFS + 3-color DFS cycle check.
2. [Alien Dictionary (LC 269 / LintCode 892)](https://leetcode.com/problems/alien-dictionary/) — Extracting directed edges from adjacent words (w1[i] != w2[i]), handling invalid prefix edge cases ("abc" before "ab"), and running Topo Sort (using a PriorityQueue if lexicographically smallest order is required).
3. [Find All Possible Recipes from Given Supplies (LC 2115)](https://leetcode.com/problems/find-all-possible-recipes-from-given-supplies/) — Topological dependency resolution with string nodes.
4. [Build a Matrix With Conditions (LC 2392)](https://leetcode.com/problems/build-a-matrix-with-conditions/) — Decoupling 2D into 1D: Run two independent Topological Sorts (one for row constraints, one for column constraints) and pair (row_pos[x], col_pos[x]).
5. [Sort Items by Groups Respecting Dependencies (LC 1203)](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/) — Two-Level Topological Sort: Topo sort the groups first, then topo sort the items within each group, and stitch them together.
6. [Minimum Height Trees (LC 310)](https://leetcode.com/problems/minimum-height-trees/) — Synced with Tree Sheet: Undirected degree-1 "onion peeling" topological sort to find graph centers.
7. [Largest Color Value in a Directed Graph (LC 1857)](https://leetcode.com/problems/largest-color-value-in-a-directed-graph/) — Synced with DP Sheet: Kahn's Topo Sort + dp[u][26] state propagation.
8. [Strange Printer II (LC 1591)](https://leetcode.com/problems/strange-printer-ii/) — Bounding-box dependency graph: if color B appears inside color A's bounding rectangle, add edge A \to B and check if the graph is a valid DAG.

---

### 4. Shortest Paths: Modified Dijkstra, Bellman-Ford & Floyd-Warshall (11 Problems)
In OAs, you rarely run plain Dijkstra on dist[u]. Instead, you run Dijkstra on a Multi-Dimensional State Graph dist[u][state] (where state is fuel left, stops remaining, parity, or whether a discount coupon was used).

1. [Network Delay Time (LC 743)](https://leetcode.com/problems/network-delay-time/) & [Path with Maximum Probability (LC 1514)](https://leetcode.com/problems/path-with-maximum-probability/) — Standard Dijkstra template (if (d > dist[u]) continue; lazy deletion in Java PriorityQueue).
2. [Path With Minimum Effort (LC 1631)](https://leetcode.com/problems/path-with-minimum-effort/) / [Swim in Rising Water (LC 778)](https://leetcode.com/problems/swim-in-rising-water/) — Minimax Dijkstra: Path cost is \max(\text{edge weights}) instead of \sum(\text{edge weights}).
3. [Cheapest Flights Within K Stops (LC 787)](https://leetcode.com/problems/cheapest-flights-within-k-stops/) — Know both Bellman-Ford (K+1 edge relaxations) and Modified Dijkstra pruning by stops[u] instead of just cost[u].
4. [Flight Discount (CSES 1195)](https://cses.fi/problemset/task/1195) — 2-State Dijkstra (dist[u][0] vs dist[u][1]): Halving the weight of at most one edge on the path. Absolute #1 OA Dijkstra pattern.
5. [Minimum Cost to Reach Destination in Time (LC 1928)](https://leetcode.com/problems/minimum-cost-to-reach-destination-in-time/) — Constrained Dijkstra: minimize cost subject to time <= maxTime by tracking minTime[u] to prune suboptimal states.
6. [Second Minimum Time to Reach Destination (LC 2045)](https://leetcode.com/problems/second-minimum-time-to-reach-destination/) — Maintaining dist1[u] (strictly shortest) and dist2[u] (strictly second shortest) per node + handling red/green traffic light wait math.
7. [Flight Routes (CSES 1196)](https://cses.fi/problemset/task/1196) — K-Shortest Paths Dijkstra: Allowing each node to be popped from the PriorityQueue up to K times (count[u] <= K).
8. [High Score (CSES 1673)](https://cses.fi/problemset/task/1673) — Bellman-Ford Positive/Negative Cycle Detection: Detecting a negative cycle is easy (N-th relaxation), but you must also run BFS/DFS to verify the cycle is actually reachable from source 1 and can reach destination N.
9. [Find the City With the Smallest Number of Neighbors at a Threshold Distance (LC 1334)](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/) — Standard O(V^3) Floyd-Warshall all-pairs shortest path.
10. [Number of Possible Sets of Closing Branches (LC 2959)](https://leetcode.com/problems/number-of-possible-sets-of-closing-branches/) — Bitmask (2^N for N \le 10) + Floyd-Warshall.
11. [Design Graph With Shortest Path Calculator (LC 2642)](https://leetcode.com/problems/design-graph-with-shortest-path-calculator/) — Incremental Floyd-Warshall: When adding a single new directed edge (u, v, w) dynamically, update all pairs in O(V^2) via dist[i][j] = min(dist[i][j], dist[i][u] + w + dist[v][j]) instead of O(V^3).

---

### 5. Disjoint Set Union (DSU) & Offline Query Sorting (10 Problems)
DSU (Union by Size/Rank + Path Compression) is arguably the highest-ROI data structure for OAs. Whenever a problem asks about connectivity, components, or maximum edge weight bottlenecks, think DSU.

1. [Redundant Connection (LC 684)](https://leetcode.com/problems/redundant-connection/) — Basic undirected cycle detection via DSU (and know [Redundant Connection II (LC 685)](https://leetcode.com/problems/redundant-connection-ii/) for handling a directed tree with one extra edge: checking in-degree = 2 vs. directed cycle).
2. [Most Stones Removed with Same Row or Column (LC 947)](https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/) — Bipartite Coordinate DSU: Instead of connecting stone i to stone j in O(N^2), union row_i with col_j + 10001 in O(N \alpha(N)).
3. [Greatest Common Divisor Traversal (LC 2709)](https://leetcode.com/problems/greatest-common-divisor-traversal/) / [Largest Component Size by Common Factor (LC 952)](https://leetcode.com/problems/largest-component-size-by-common-factor/) — Prime Factor DSU: Union each number nums[i] with its prime factors up to \sqrt{\text{val}} via Sieve, never comparing pairs of numbers directly.
4. [Making A Large Island (LC 827)](https://leetcode.com/problems/making-a-large-island/) — Pre-computing component sizes via DSU/DFS, then flipping a single 0 to 1 and summing unique neighboring component IDs.
5. [Last Day Where You Can Still Cross (LC 1970)](https://leetcode.com/problems/last-day-where-you-can-still-cross/) — Reverse Time DSU: Deleting edges/cells is hard; reverse the queries chronologically so deletions become DSU union() additions (or use virtual TOP and BOTTOM nodes).
6. [Maximum Segment Sum After Removals (LC 2382)](https://leetcode.com/problems/maximum-segment-sum-after-removals/) — Reverse-time DSU maintaining the maximum component sum as destroyed elements are added back backward.
7. [Checking Existence of Edge Length Limited Paths (LC 1697)](https://leetcode.com/problems/checking-existence-of-edge-length-limited-paths/) — Offline Query Sorting + DSU: Sort both the queries (by limit) and the edges (by weight), and use a two-pointer sweep to union edges < \text{limit} before answering each query in O(1).
8. [Find All People With Secret (LC 2092)](https://leetcode.com/problems/find-all-people-with-secret/) — Time-batched DSU with Selective Reset (parent[u] = u for people who didn't learn the secret at timestamp T).
9. [Number of Good Paths (LC 2421)](https://leetcode.com/problems/number-of-good-paths/) — Synced with Tree Sheet: Sorting nodes by value and uniting from smallest to largest to bound maximum path values.
10. [Regions Cut By Slashes (LC 959)](https://leetcode.com/problems/regions-cut-by-slashes/) — Splitting each 1 \times 1 grid cell into 4 triangles (North, East, South, West) and uniting adjacent triangles; or counting cycles via Euler's formula F = E - V + C.

---

### 6. Minimum Spanning Tree (MST) & Virtual Node Modeling (5 Problems)
Kruskal’s Algorithm (Sort edges + DSU) is preferred 95% of the time, EXCEPT on complete implicit graphs where Prim’s O(V^2) avoids sorting O(V^2 \log V) edges.

1. [Min Cost to Connect All Points (LC 1584)](https://leetcode.com/problems/min-cost-to-connect-all-points/) — Know both Kruskal's (O(N^2 \log N)) and dense Prim's Algorithm (O(N^2) without a PriorityQueue) for Manhattan distance grids.
2. [Optimize Water Distribution in a Village (LC 1168)](https://leetcode.com/problems/optimize-water-distribution-in-a-village/) — The Virtual Super-Node Trick: Whenever nodes have a "self-creation cost" (building a well at node i costs W_i) AND edge connection costs (pipes between i and j), create a dummy Node 0 and add edges (0, i) with weight W_i, then run standard MST!
3. [Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree (LC 1489)](https://leetcode.com/problems/find-critical-and-pseudo-critical-edges-in-minimum-spanning-tree/) — Testing MST edge necessity by running Kruskal's with edge i forced out vs. forced in.
4. [Remove Max Number of Edges to Keep Graph Fully Traversable (LC 1579)](https://leetcode.com/problems/remove-max-number-of-edges-to-keep-graph-fully-traversable/) — Two-player DSU: greedily process Type-3 (shared) edges first, then clone the DSU state for Alice and Bob.
5. Min Cost to Connect All Points / Manhattan MST — Understanding when to connect only adjacent sorted coordinates (e.g., Cost to Connect 3D Planets in OAs where cost is \min(\vert{}x_1-x_2\vert{}, \vert{}y_1-y_2\vert{}, \vert{}z_1-z_2\vert{}) \rightarrow sort by X, Y, and Z separately to reduce N^2 edges down to 3N edges).

---

### 7. Bridges, Articulation Points, SCCs & Eulerian Paths (7 Problems)
These are the classic competitive-programming graph algorithms that frequently drop as Q4 in Uber, Sprinklr, Rubrik, and Google OAs.

1. [Critical Connections in a Network (LC 1192)](https://leetcode.com/problems/critical-connections-in-a-network/) — Tarjan’s Bridge-Finding Algorithm: Tracking tin[u] (discovery time) and low[u] (lowest reachable ancestor). An edge (u, v) is a bridge iff low[v] > tin[u].
2. [Minimum Number of Days to Disconnect Island (LC 1568)](https://leetcode.com/problems/minimum-number-of-days-to-disconnect-island/) — Observation + Articulation Points: The answer on any 2D grid is always 0, 1, or 2 (a corner cell only has 2 neighbors). Check if initial components \ne 1 (0 days), else check for an Articulation Point (1 day), else return 2.
3. [Planets and Kingdoms (CSES 1683)](https://cses.fi/problemset/task/1683) & [Coin Collector (CSES 1686)](https://cses.fi/problemset/task/1686) — Kosaraju’s / Tarjan’s Strongly Connected Components (SCC): Condensing a directed graph with cycles into a DAG of SCCs so you can run DP on it.
4. [Reconstruct Itinerary (LC 332)](https://leetcode.com/problems/reconstruct-itinerary/) — Hierholzer’s Algorithm for Eulerian Path: Post-order DFS (while (!pq.isEmpty()) dfs(pq.poll()); res.addFirst(u);) to visit every directed edge exactly once.
5. [Valid Arrangement of Pairs (LC 2097)](https://leetcode.com/problems/valid-arrangement-of-pairs/) — Pure Eulerian Path: finding the start node via outDegree[u] - inDegree[u] == 1 + Hierholzer's post-order stack.
6. [Cracking the Safe (LC 753)](https://leetcode.com/problems/cracking-the-safe/) — De Bruijn sequence constructed as an Eulerian circuit over (k^{n-1}) states.
7. [Find Edges in Shortest Paths (LC 3123)](https://leetcode.com/problems/find-edges-in-shortest-paths/) — Forward + Backward Dijkstra: Running Dijkstra from 0 (distFromStart[]) and from N-1 (distFromEnd[]). An edge (u, v, w) lies on a shortest path iff distFromStart[u] + w + distFromEnd[v] == distFromStart[N-1]. (Sometimes paired with Tarjan's to find critical shortest-path bridges!).

---

### 8. Graph + Bitmask, Bipartite Matching & Meet-in-the-Middle (6 Problems)
Rounding out the small-N and matching patterns.

1. [Shortest Path Visiting All Nodes (LC 847)](https://leetcode.com/problems/shortest-path-visiting-all-nodes/) — Synced with DP Sheet: Multi-source BFS on (u, mask) for shortest walk visiting all nodes.
2. [Find the Shortest Superstring (LC 943)](https://leetcode.com/problems/find-the-shortest-superstring/) — Travelling Salesperson Problem (TSP) on string overlaps (dp[mask][last_node]) + parent pointer reconstruction.
3. [Parallel Courses II (LC 1494)](https://leetcode.com/problems/parallel-courses-ii/) — Prerequisite DAG + O(3^N) Bitmask submask enumeration for taking at most K available courses per semester.
4. [Maximum Employees to Be Invited to a Meeting (LC 2127)](https://leetcode.com/problems/maximum-employees-to-be-invited-to-a-meeting/) — The ultimate Functional Graph boss: comparing the largest cycle of length \ge 3 against the sum of all 2-cycles (A <-> B) extended by their longest incoming tree chains.
5. [Maximum Number of Accepted Invitations (LC 1820)](https://leetcode.com/problems/maximum-number-of-accepted-invitations/) / [School Dance (CSES 1696)](https://cses.fi/problemset/task/1696) — Kuhn’s Algorithm for Maximum Bipartite Matching in O(V \cdot E) (30 lines of simple DFS code boolean tryKuhn(int u)—no need to memorize heavy Dinic's Max Flow for OAs).
6. [Maximum Score of a Node Sequence (LC 2242)](https://leetcode.com/problems/maximum-score-of-a-node-sequence/) — Fixed-Length Subgraph Enumeration: To find the max weight path of 4 nodes A - B - C - D, maintain only the top 3 heaviest neighbors at every node, then iterate over the middle edge (B, C) in O(E \cdot 3 \cdot 3) = O(E).

---

### The "OA Graph Cheat Code" (Constraint-to-Algorithm Reflex)
When you open a Graph question in an OA, immediately map the problem structure using these 7 rules:

1. Unweighted edges \rightarrow Plain BFS (O(V + E)).
2. Edge weights are only 0 or 1 \rightarrow 0-1 BFS with ArrayDeque (#6–7).
3. Positive edge weights + "at most K discounts/fuel/stops" \rightarrow State-Space Dijkstra dist[u][state] (#29–31).
4. Path cost is the MAXIMUM edge weight on the path (Bottleneck) \rightarrow Sort queries & edges offline + DSU (#44), OR Minimax Dijkstra (#28).
5. Edges/Cells are being destroyed over time \rightarrow Reverse the timeline and use DSU union() (#42–43).
6. Every node has exactly 1 outgoing edge (edges[i] array) \rightarrow Functional Graph: peel in-degree 0 tree chains with Kahn's BFS; remaining nodes are pure cycles (#16–17, #63).
7. "Does edge (u, v) belong to ANY shortest path?" \rightarrow Run Dijkstra twice (from src and from dest) and check d1[u] + w + d2[v] == d1[dest] (#59).
