---
share: true
---

### 1. 1D Linear, State Machine & Modified Kadane DP (12 Problems)

1. **House Robber II** (LC 213) — Circular array linear DP via two-pass split.
2. **Decode Ways** (LC 91) — Conditional 1-step and 2-step counting transitions.
3. **Maximum Product Subarray** (LC 152) — Tracking simultaneous `min` and `max` states for sign flips.
4. **Longest Increasing Subsequence** (LC 300) — Both O(N^2) DP and O(Nlog⁡N) binary search / patience sorting.
5. **Maximum Subarray Sum with One Deletion** (LC 1186) — State-split Kadane (`0` deletions used vs. `1` deletion used).
6. **Maximum Subarray Sum After One Operation** (LC 1746) — State-split Kadane (squaring at most one element).
7. **Maximum Score Of Spliced Array** (LC 2321) — Reducing subarray swap between two arrays to Kadane on difference array.
8. **Best Time to Buy and Sell Stock with Cooldown** (LC 309) — Blueprint 3-state machine (`held`, `sold`, `reset`).
9. **Best Time to Buy and Sell Stock IV** (LC 188) — Generalizing state machines to KKK bounded transactions.
10. **Minimum Swaps To Make Sequences Increasing** (LC 801) — 2-state transitions (`swap[i]` vs. `no_swap[i]`).
11. **Student Attendance Record II** (LC 552) — Multi-state linear counting (`dp[i][absences][consecutive_lates]`).
12. **Paint House III** (LC 1473) — 3D state transitions (`dp[idx][prev_color][neighborhoods]`).

### 2. Grid, Matrix & DAG DP (Topological Sort) (8 Problems)

13. **Maximal Square** (LC 221) — 2D grid transition `1 + min(top, left, top-left)`.
14. **Dungeon Game** (LC 174) — Reverse bottom-up grid DP starting from destination (N−1,M−1)(N-1, M-1)(N−1,M−1) to (0,0)(0, 0)(0,0).
15. **Cherry Pickup** (LC 741) — 3D simultaneous 2-agent grid traversal `dp[r1][c1][c2]` (handling shared cell pickup).
16. **Cherry Pickup II** (LC 1463) — Row-by-row 2-agent grid traversal `dp[row][col1][col2]`.
17. **Array Description** (CSES) — Bounded adjacent value transitions (`x - 1`, `x`, `x + 1`).
18. **Longest Path** (AtCoder DP Contest - Problem G) — Base blueprint for DP on a Directed Acyclic Graph (DAG).
19. **Parallel Courses III** (LC 2050) — Critical path / minimum completion time on a DAG using Kahn's Topological Sort + DP.
20. **Largest Color Value in a Directed Graph** (LC 1857) — Topological Sort + `dp[u][26]` character frequency propagation with cycle detection.

### 3. Knapsack, State Compression & Meet-in-the-Middle (9 Problems)

21. **Coin Change** (LC 322) & **Coin Change II** (LC 518) — Unbounded knapsack: min coins vs. ordered combinations (and **Combination Sum IV** for permutations).
22. **Partition Equal Subset Sum** (LC 416) — 0/1 Subset sum with 1D space optimization.
23. **Target Sum** (LC 494) — Algebraic reduction of +/− assignments to subset sum (S +target)/2
24. **Last Stone Weight II** (LC 1049) — Reducing pairwise cancellations to minimizing subset difference ∣S1−S2∣.
25. **Ones and Zeroes** (LC 474) — Dual-cost 0/1 knapsack (`dp[zeros][ones]`).
26. **Profitable Schemes** (LC 879) — Capping the "at least P profit" dimension at min⁡(P, p +gain).
27. **Tallest Billboard** (LC 956) — State compression: replacing `dp[i][sum1][sum2]` with difference `dp[i][abs(sum1 - sum2)]`.
28. **Closest Subsequence Sum** (LC 1755) — Meet-in-the-Middle (N≤40, values up to 10^9 where standard knapsack TLEs).
29. **Partition Array Into Two Arrays to Minimize Sum Difference** (LC 2035) — Meet-in-the-Middle grouped by subset size + Binary Search.

### 4. Strings, LCS, Palindromes, KMP & Trie DP (13 Problems)

30. **Longest Common Subsequence** (LC 1143) — Core 2D string DP + 2-row O(min⁡(N,M)) space reduction.
31. **Edit Distance** (LC 72) — Insert, delete, and replace state transitions.
32. **Distinct Subsequences** (LC 115) — Counting occurrences of string T as a subsequence of S.
33. **Distinct Subsequences II** (LC 940) — Counting unique subsequences using `last_seen[char]` subtraction.
34. **Shortest Common Supersequence** (LC 1092) — String reconstruction by backtracking through the DP table.
35. **Interleaving String** (LC 97) — 2D boolean reachability DP on two string prefixes.
36. **Regular Expression Matching** (LC 10) — Zero-or-more `*` wildcard state transitions.
37. **Longest Palindromic Subsequence** (LC 516) — Subsequence palindrome base (and N−LPS for min insertions).
38. **Palindrome Partitioning II** (LC 132) — Precomputing O(N^2) `isPal[i][j]` table + O(N^2) partition cuts.
39. **Restore The Array** (LC 1416) — 1D string partitioning to count valid integer splits ≤K.
40. **Extra Characters in a String** (LC 2707) — Partition DP accelerated by walking a **Trie** to eliminate substring hashing overhead.
41. **Number of Ways to Form a Target String Given a Dictionary** (LC 1639) — Column frequency precomputation + 2D counting DP.
42. **Find All Good Strings** (LC 1397) — String Automaton DP: Digit DP lexicographical bounds + **KMP `lps` array** state tracking to avoid forbidden substrings.

### 5. DP on Trees (12 Problems)

43. **House Robber III** (LC 337) — Archetype 1: Subtree Include / Exclude (`[not_robbed, robbed]`).
44. **Binary Tree Maximum Path Sum** (LC 124) — Archetype 2: Returning max single branch to parent while updating global LCA path.
45. **Binary Tree Cameras** (LC 968) — 3-state bottom-up DP (`UNCOVERED`, `HAS_CAMERA`, `COVERED_NO_CAM`).
46. **Maximum Score After Applying Operations on a Tree** (LC 2925) — Subtree sum vs. min bottleneck kept to keep root-to-leaf paths valid.
47. **Find the Maximum Sum of Node Values** (LC 3068) — Tree XOR parity DP (tracking even/odd number of XORed nodes in subtrees).
48. **Tree Diameter** (CSES) — Longest path in an N- ary adjacency-list tree.
49. **Tree Distances I** (CSES) — Archetype 3 (Rerooting): Tracking top-2 longest downward child paths at every node.
50. **Tree Distances II** (CSES / LC 834) — Rerooting distance sums via subtree sizes: `ans[v] = ans[u] - sz[v] + (N - sz[v])`.
51. **Minimum Edge Reversals So Every Node Is Reachable** (LC 2858) — Rerooting on directed graphs using 0/1 edge weights.
52. **Distance in Tree** (Codeforces 161D) — Archetype 4 (Subtree Merging): Counting node pairs at distance KKK via `dp[u][k]`.
53. **Binary Tree With Factors** (LC 823) — Bottom-up tree construction DP sorted by node values (`root = left * right`).
54. **Minimum Edge Weight Equilibrium Queries in a Tree** (LC 2846) — **Binary Lifting as DP** (`up[u][j]` and prefix frequency counts from root).

### 6. Interval, Partition & Insertion DP (6 Problems)

55. **Burst Balloons** (LC 312) — Reverse interval thinking: iterate over the **last** item processed in range [L…R].
56. **Minimum Cost to Cut a Stick** (LC 1547) — Interval boundary sorting + O(M^3) partition DP.
57. **Strange Printer** (LC 664) — Character-merging interval DP (`s[i] == s[k]` optimization).
58. **Minimum Cost to Merge Stones** (LC 1000) — K-way pile merging with prefix sums and modulo length feasibility checks.
59. **Split Array Largest Sum** (LC 410) — Compare O(N^2 * K) Partition DP against O(Nlog(∑)) Binary Search on Answer.
60. **Number of Ways to Rearrange Sticks With K Sticks Visible** (LC 1866) — **Insertion DP** (placing the shortest stick first into growing permutations).

### 7. Bitmask, Submask (3N3^N3N) & SOS DP (9 Problems)

61. **Partition to K Equal Sum Subsets** (LC 698) — Bitmask state `(mask, current_bucket_sum)`.
62. **Shortest Path Visiting All Nodes** (LC 847) — Multi-source BFS + Bitmask DP `(u, mask)` for shortest Hamiltonian walk.
63. **Minimum Cost to Connect Two Groups of Points** (LC 1595) — Bipartite matching with bitmask state.
64. **Number of Ways to Wear Different Hats to Each Other** (LC 1434) — **Axis Swapping:** Iterate over 40 hats and bitmask the 10 people (2^10 vs. 2^40).
65. **Elevator Rides** (CSES) — Storing a tuple `(min_rides, min_last_weight)` inside `dp[mask]` to eliminate a state dimension.
66. **Maximum Students Taking Exam** (LC 1349) — Broken profile / row-by-row bitmask validation (`(mask & (mask >> 1)) == 0`).
67. **Fair Distribution of Cookies** (LC 2305) — **O(3^N) Submask Enumeration:** `for (int sub = mask; sub > 0; sub = (sub - 1) & mask)`.
68. **Maximum Product of the Length of Two Palindromic Subsequences** (LC 2002) — Disjoint bitmask enumeration (`mask` and `((1 << N) - 1) ^ mask`).
69. **Compatible Numbers** (Codeforces 165E) — **SOS (Sum Over Subsets) DP** in O(N⋅2^N) when N=20 and O(3^N) TLEs.

### 8. Digit DP (3 Problems)

70. **Numbers At Most N Given Digit Set** (LC 902) — Foundation of `tight` and `isLeadingZero` flags. 
71. **Count Special Integers** (LC 2376) — Digit DP + 10-bit used-digit mask. 
72. **Number of Beautiful Integers in the Range** (LC 2827) — Digit DP tracking `(even_odd_diff, running_val_mod_K)`.

### 9. Game Theory & Probability DP (5 Problems)

73. **Predict the Winner** (LC 486) — Minimax score difference: `dp[L][R] = max(nums[L] - dp[L+1][R], nums[R] - dp[L][R-1])`.
74. **Stone Game II** (LC 1140) — Suffix sum minimax with dynamic step limit M.
75. **Can I Win** (LC 464) — Top-down minimax over a bitmask state space.
76. **Knight Probability in Chessboard** (LC 688) — Floating-point Markov chain grid transitions.
77. **New 21 Game** (LC 837) — Probability DP optimized from O(N⋅W)to O(N) using a sliding window sum.

### 10. DP + Data Structures / Optimization Tricks (8 Problems)

78. **Maximum Profit in Job Scheduling** (LC 1235) — **DP + Binary Search:** Sorting intervals + jumping to the next non-overlapping state.
79. **Maximum Number of Events That Can Be Attended II** (LC 1751) — 2D DP (`dp[i][k]`) + Binary Search on sorted intervals.
80. **Constrained Subsequence Sum** (LC 1425) — **DP + Monotonic Deque:** Reducing range maximum transition max⁡ of j=i−K to i−1 (dp[j]) to amortized O(1).
81. **K Inverse Pairs Array** (LC 629) — **DP + Prefix Sum:** Optimizing range sum transitions           dp [i] [j]  =∑ (k=j−i+1to k=j (dp [i−1] [k] )) to O(1) per state.
82. **Longest Arithmetic Subsequence of Given Difference** (LC 1218) — Replacing inner transition search with an O(1) `HashMap` lookup.
83. **Russian Doll Envelopes** (LC 354) — Custom comparator `(width ASC, height DESC)` to reduce 2D box nesting to 1D O(Nlog⁡N) LIS.
84. **Longest Increasing Subsequence II** (LC 2407) — **Value-Indexed DP + Segment Tree:** Range max query on `[val - K, val - 1]` in O( Nlog⁡ ( max⁡A )).
85. **Maximum Balanced Subsequence Sum** (LC 2926) — Algebraic inequality rewrite (`nums[i] - i <= nums[j] - j`) + Coordinate Compression + Fenwick/Segment Tree.