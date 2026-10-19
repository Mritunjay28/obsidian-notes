---
share: true
---

### 1. 1D Linear, State Machine & Modified Kadane DP (12 Problems)

1. [House Robber II (LC 213)](https://leetcode.com/problems/house-robber-ii/) — Circular array linear DP via two-pass split.
2. [Decode Ways (LC 91)](https://leetcode.com/problems/decode-ways/) — Conditional 1-step and 2-step counting transitions.
	1. [Maximum Product Subarray (LC 152)](https://leetcode.com/problems/maximum-product-subarray/) — Tracking simultaneous min and max states for sign flips.
3. [Longest Increasing Subsequence (LC 300)](https://leetcode.com/problems/longest-increasing-subsequence/) — Both O(N^2) DP and O(Nlog⁡N) binary search / patience sorting.
4. [Maximum Subarray Sum with One Deletion (LC 1186)](https://leetcode.com/problems/maximum-subarray-sum-with-one-deletion/) — State-split Kadane (0 deletions used vs. 1 deletion used).
5. [Maximum Subarray Sum After One Operation (LC 1746)](https://leetcode.com/problems/maximum-subarray-sum-after-one-operation/) — State-split Kadane (squaring at most one element).
6. [Maximum Score Of Spliced Array (LC 2321)](https://leetcode.com/problems/maximum-score-of-spliced-array/) — Reducing subarray swap between two arrays to Kadane on difference array.
7. [Best Time to Buy and Sell Stock with Cooldown (LC 309)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) — Blueprint 3-state machine (held, sold, reset).
8. [Best Time to Buy and Sell Stock IV (LC 188)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/) — Generalizing state machines to K bounded transactions.
9. [Minimum Swaps To Make Sequences Increasing (LC 801)](https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing/) — 2-state transitions (swap[i] vs. no_swap[i]).
10. [Student Attendance Record II (LC 552)](https://leetcode.com/problems/student-attendance-record-ii/) — Multi-state linear counting (dp[i][absences][consecutive_lates]).
11. [Paint House III (LC 1473)](https://leetcode.com/problems/paint-house-iii/) — 3D state transitions (dp[idx][prev_color][neighborhoods]).

---

### 2. Grid, Matrix & DAG DP (Topological Sort) (8 Problems)

1. [Maximal Square (LC 221)](https://leetcode.com/problems/maximal-square/) — 2D grid transition 1 + min(top, left, top-left).
2. [Dungeon Game (LC 174)](https://leetcode.com/problems/dungeon-game/) — Reverse bottom-up grid DP starting from destination (N-1, M-1) to (0, 0).
3. [Cherry Pickup (LC 741)](https://leetcode.com/problems/cherry-pickup/) — 3D simultaneous 2-agent grid traversal dp[r1][c1][c2] (handling shared cell pickup).
4. [Cherry Pickup II (LC 1463)](https://leetcode.com/problems/cherry-pickup-ii/) — Row-by-row 2-agent grid traversal dp[row][col1][col2].
5. [Array Description (CSES)](https://cses.fi/problemset/task/1746) — Bounded adjacent value transitions (x - 1, x, x + 1).
6. [Longest Path (AtCoder DP Contest - Problem G)](https://atcoder.jp/contests/dp/tasks/dp_g) — Base blueprint for DP on a Directed Acyclic Graph (DAG).
7. [Parallel Courses III (LC 2050)](https://leetcode.com/problems/parallel-courses-iii/) — Critical path / minimum completion time on a DAG using Kahn's Topological Sort + DP.
8. [Largest Color Value in a Directed Graph (LC 1857)](https://leetcode.com/problems/largest-color-value-in-a-directed-graph/) — Topological Sort + dp[u][26] character frequency propagation with cycle detection.

---

### 3. Knapsack, State Compression & Meet-in-the-Middle (9 Problems)

1. [Coin Change (LC 322)](https://leetcode.com/problems/coin-change/) & [Coin Change II (LC 518)](https://leetcode.com/problems/coin-change-ii/) — Unbounded knapsack: min coins vs. ordered combinations (and [Combination Sum IV (LC 377)](https://leetcode.com/problems/combination-sum-iv/) for permutations).
2. [Partition Equal Subset Sum (LC 416)](https://leetcode.com/problems/partition-equal-subset-sum/) — 0/1 Subset sum with 1D space optimization.
3. [Target Sum (LC 494)](https://leetcode.com/problems/target-sum/) — Algebraic reduction of +/- assignments to subset sum (S + target) / 2.
4. [Last Stone Weight II (LC 1049)](https://leetcode.com/problems/last-stone-weight-ii/) — Reducing pairwise cancellations to minimizing subset difference |S1 - S2|.
5. [Ones and Zeroes (LC 474)](https://leetcode.com/problems/ones-and-zeroes/) — Dual-cost 0/1 knapsack (dp[zeros][ones]).
6. [Profitable Schemes (LC 879)](https://leetcode.com/problems/profitable-schemes/) — Capping the "at least P profit" dimension at min(P, p + gain).
7. [Tallest Billboard (LC 956)](https://leetcode.com/problems/tallest-billboard/) — State compression: replacing dp[i][sum1][sum2] with difference dp[i][abs(sum1 - sum2)].
8. [Closest Subsequence Sum (LC 1755)](https://leetcode.com/problems/closest-subsequence-sum/) — Meet-in-the-Middle (N ≤ 40, values up to 10^9 where standard knapsack TLEs).
9. [Partition Array Into Two Arrays to Minimize Sum Difference (LC 2035)](https://leetcode.com/problems/partition-array-into-two-arrays-to-minimize-sum-difference/) — Meet-in-the-Middle grouped by subset size + Binary Search.

---

### 4. Strings, LCS, Palindromes, KMP & Trie DP (13 Problems)

1. [Longest Common Subsequence (LC 1143)](https://leetcode.com/problems/longest-common-subsequence/) — Core 2D string DP + 2-row O(min(N, M)) space reduction.
2. [Edit Distance (LC 72)](https://leetcode.com/problems/edit-distance/) — Insert, delete, and replace state transitions.
3. [Distinct Subsequences (LC 115)](https://leetcode.com/problems/distinct-subsequences/) — Counting occurrences of string T as a subsequence of S.
4. [Distinct Subsequences II (LC 940)](https://leetcode.com/problems/distinct-subsequences-ii/) — Counting unique subsequences using last_seen[char] subtraction.
5. [Shortest Common Supersequence (LC 1092)](https://leetcode.com/problems/shortest-common-supersequence/) — String reconstruction by backtracking through the DP table.
6. [Interleaving String (LC 97)](https://leetcode.com/problems/interleaving-string/) — 2D boolean reachability DP on two string prefixes.
7. [Regular Expression Matching (LC 10)](https://leetcode.com/problems/regular-expression-matching/) — Zero-or-more * wildcard state transitions.
8. [Longest Palindromic Subsequence (LC 516)](https://leetcode.com/problems/longest-palindromic-subsequence/) — Subsequence palindrome base (and N - LPS for min insertions).
9. [Palindrome Partitioning II (LC 132)](https://leetcode.com/problems/palindrome-partitioning-ii/) — Precomputing O(N^2) isPal[i][j] table + O(N^2) partition cuts.
10. [Restore The Array (LC 1416)](https://leetcode.com/problems/restore-the-array/) — 1D string partitioning to count valid integer splits ≤ K.
11. [Extra Characters in a String (LC 2707)](https://leetcode.com/problems/extra-characters-in-a-string/) — Partition DP accelerated by walking a Trie to eliminate substring hashing overhead.
12. [Number of Ways to Form a Target String Given a Dictionary (LC 1639)](https://leetcode.com/problems/number-of-ways-to-form-a-target-string-given-a-dictionary/) — Column frequency precomputation + 2D counting DP.
13. [Find All Good Strings (LC 1397)](https://leetcode.com/problems/find-all-good-strings/) — String Automaton DP: Digit DP lexicographical bounds + KMP lps array state tracking to avoid forbidden substrings.

---

### 5. DP on Trees (12 Problems)

1. [House Robber III (LC 337)](https://leetcode.com/problems/house-robber-iii/) — Archetype 1: Subtree Include / Exclude ([not_robbed, robbed]).
2. [Binary Tree Maximum Path Sum (LC 124)](https://leetcode.com/problems/binary-tree-maximum-path-sum/) — Archetype 2: Returning max single branch to parent while updating global LCA path.
3. [Binary Tree Cameras (LC 968)](https://leetcode.com/problems/binary-tree-cameras/) — 3-state bottom-up DP (UNCOVERED, HAS_CAMERA, COVERED_NO_CAM).
4. [Maximum Score After Applying Operations on a Tree (LC 2925)](https://leetcode.com/problems/maximum-score-after-applying-operations-on-a-tree/) — Subtree sum vs. min bottleneck kept to keep root-to-leaf paths valid.
5. [Find the Maximum Sum of Node Values (LC 3068)](https://leetcode.com/problems/find-the-maximum-sum-of-node-values/) — Tree XOR parity DP (tracking even/odd number of XORed nodes in subtrees).
6. [Tree Diameter (CSES)](https://cses.fi/problemset/task/1131) — Longest path in an N-ary adjacency-list tree.
7. [Tree Distances I (CSES)](https://cses.fi/problemset/task/1132) — Archetype 3 (Rerooting): Tracking top-2 longest downward child paths at every node.
8. [Tree Distances II (CSES / LC 834)](https://leetcode.com/problems/sum-of-distances-in-tree/) — Rerooting distance sums via subtree sizes: ans[v] = ans[u] - sz[v] + (N - sz[v]).
9. [Minimum Edge Reversals So Every Node Is Reachable (LC 2858)](https://leetcode.com/problems/minimum-edge-reversals-so-every-node-is-reachable/) — Rerooting on directed graphs using 0/1 edge weights.
10. [Distance in Tree (Codeforces 161D)](https://codeforces.com/problemset/problem/161/D) — Archetype 4 (Subtree Merging): Counting node pairs at distance K via dp[u][k].
11. [Binary Tree With Factors (LC 823)](https://leetcode.com/problems/binary-tree-with-factors/) — Bottom-up tree construction DP sorted by node values (root = left * right).
12. [Minimum Edge Weight Equilibrium Queries in a Tree (LC 2846)](https://leetcode.com/problems/minimum-edge-weight-equilibrium-queries-in-a-tree/) — Binary Lifting as DP (up[u][j] and prefix frequency counts from root).

---

### 6. Interval, Partition & Insertion DP (6 Problems)

1. [Burst Balloons (LC 312)](https://leetcode.com/problems/burst-balloons/) — Reverse interval thinking: iterate over the last item processed in range [L...R].
2. [Minimum Cost to Cut a Stick (LC 1547)](https://leetcode.com/problems/minimum-cost-to-cut-a-stick/) — Interval boundary sorting + O(M^3) partition DP.
3. [Strange Printer (LC 664)](https://leetcode.com/problems/strange-printer/) — Character-merging interval DP (s[i] == s[k] optimization).
4. [Minimum Cost to Merge Stones (LC 1000)](https://leetcode.com/problems/minimum-cost-to-merge-stones/) — K-way pile merging with prefix sums and modulo length feasibility checks.
5. [Split Array Largest Sum (LC 410)](https://leetcode.com/problems/split-array-largest-sum/) — Compare O(N^2 * K) Partition DP against O(N log(∑)) Binary Search on Answer.
6. [Number of Ways to Rearrange Sticks With K Sticks Visible (LC 1866)](https://leetcode.com/problems/number-of-ways-to-rearrange-sticks-with-k-sticks-visible/) — Insertion DP (placing the shortest stick first into growing permutations).

---

### 7. Bitmask, Submask (3^N) & SOS DP (9 Problems)

1. [Partition to K Equal Sum Subsets (LC 698)](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/) — Bitmask state (mask, current_bucket_sum).
2. [Shortest Path Visiting All Nodes (LC 847)](https://leetcode.com/problems/shortest-path-visiting-all-nodes/) — Multi-source BFS + Bitmask DP (u, mask) for shortest Hamiltonian walk.
3. [Minimum Cost to Connect Two Groups of Points (LC 1595)](https://leetcode.com/problems/minimum-cost-to-connect-two-groups-of-points/) — Bipartite matching with bitmask state.
4. [Number of Ways to Wear Different Hats to Each Other (LC 1434)](https://leetcode.com/problems/number-of-ways-to-wear-different-hats-to-each-other/) — Axis Swapping: Iterate over 40 hats and bitmask the 10 people (2^10 vs. 2^40).
5. [Elevator Rides (CSES)](https://cses.fi/problemset/task/1653) — Storing a tuple (min_rides, min_last_weight) inside dp[mask] to eliminate a state dimension.
6. [Maximum Students Taking Exam (LC 1349)](https://leetcode.com/problems/maximum-students-taking-exam/) — Broken profile / row-by-row bitmask validation ((mask & (mask >> 1)) == 0).
7. [Fair Distribution of Cookies (LC 2305)](https://leetcode.com/problems/fair-distribution-of-cookies/) — O(3^N) Submask Enumeration: `for (int sub = mask; sub > 0; sub = (sub - 1) & mask)`.
8. [Maximum Product of the Length of Two Palindromic Subsequences (LC 2002)](https://leetcode.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences/) — Disjoint bitmask enumeration (mask and ((1 << N) - 1) ^ mask).
9. [Compatible Numbers (Codeforces 165E)](https://codeforces.com/problemset/problem/165/E) — SOS (Sum Over Subsets) DP in O(N · 2^N) when N=20 and O(3^N) TLEs.

---

### 8. Digit DP (3 Problems)

1. [Numbers At Most N Given Digit Set (LC 902)](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/) — Foundation of tight and isLeadingZero flags.
2. [Count Special Integers (LC 2376)](https://leetcode.com/problems/count-special-integers/) — Digit DP + 10-bit used-digit mask.
3. [Number of Beautiful Integers in the Range (LC 2827)](https://leetcode.com/problems/number-of-beautiful-integers-in-the-range/) — Digit DP tracking (even_odd_diff, running_val_mod_K).

---

### 9. Game Theory & Probability DP (5 Problems)

1. [Predict the Winner (LC 486)](https://leetcode.com/problems/predict-the-winner/) — Minimax score difference: dp[L][R] = max(nums[L] - dp[L+1][R], nums[R] - dp[L][R-1]).
2. [Stone Game II (LC 1140)](https://leetcode.com/problems/stone-game-ii/) — Suffix sum minimax with dynamic step limit M.
3. [Can I Win (LC 464)](https://leetcode.com/problems/can-i-win/) — Top-down minimax over a bitmask state space.
4. [Knight Probability in Chessboard (LC 688)](https://leetcode.com/problems/knight-probability-in-chessboard/) — Floating-point Markov chain grid transitions.
5. [New 21 Game (LC 837)](https://leetcode.com/problems/new-21-game/) — Probability DP optimized from O(N · W) to O(N) using a sliding window sum.

---

### 10. DP + Data Structures / Optimization Tricks (8 Problems)

1. [Maximum Profit in Job Scheduling (LC 1235)](https://leetcode.com/problems/maximum-profit-in-job-scheduling/) — DP + Binary Search: Sorting intervals + jumping to the next non-overlapping state.
2. [Maximum Number of Events That Can Be Attended II (LC 1751)](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended-ii/) — 2D DP (dp[i][k]) + Binary Search on sorted intervals.
3. [Constrained Subsequence Sum (LC 1425)](https://leetcode.com/problems/constrained-subsequence-sum/) — DP + Monotonic Deque: Reducing range maximum transition max of j = i - K to i - 1 (dp[j]) to amortized O(1).
4. [K Inverse Pairs Array (LC 629)](https://leetcode.com/problems/k-inverse-pairs-array/) — DP + Prefix Sum: Optimizing range sum transitions dp[i][j] = ∑ (k = j - i + 1 to k = j (dp[i-1][k])) to O(1) per state.
5. [Longest Arithmetic Subsequence of Given Difference (LC 1218)](https://leetcode.com/problems/longest-arithmetic-subsequence-of-given-difference/) — Replacing inner transition search with an O(1) HashMap lookup.
6. [Russian Doll Envelopes (LC 354)](https://leetcode.com/problems/russian-doll-envelopes/) — Custom comparator (width ASC, height DESC) to reduce 2D box nesting to 1D O(N log N) LIS.
7. [Longest Increasing Subsequence II (LC 2407)](https://leetcode.com/problems/longest-increasing-subsequence-ii/) — Value-Indexed DP + Segment Tree: Range max query on [val - K, val - 1] in O(N log(max A)).
8. [Maximum Balanced Subsequence Sum (LC 2926)](https://leetcode.com/problems/maximum-balanced-subsequence-sum/) — Algebraic inequality rewrite (nums[i] - i <= nums[j] - j) + Coordinate Compression + Fenwick/Segment Tree.