---
share: true
---

### 1. DFS Bottom-Up Invariants & Pointer Manipulation (8 Problems)
Skip basic Inorder/Preorder. Focus on problems where a node must pass a constraint down while aggregating a structural change or tuple back up.

1. [Diameter of Binary Tree (LC 543)](https://leetcode.com/problems/diameter-of-binary-tree/) — Base template: returning height to parent while updating global max path at LCA.
2. [Lowest Common Ancestor of a Binary Tree (LC 236)](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) — The core post-order bubbling pattern (and know how to adapt it if nodes might not exist in the tree: [Lowest Common Ancestor of a Binary Tree II (LC 1644)](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-ii/)).
3. [Flatten Binary Tree to Linked List (LC 114)](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/) — Reverse post-order (right $\rightarrow$ left $\rightarrow$ root) pointer rewiring in $O(1)$ auxiliary space.
4. [Delete Nodes And Return Forest (LC 1110)](https://leetcode.com/problems/delete-nodes-and-return-forest/) — Passing parent deletion state down while severing child pointers bottom-up.
5. [Distribute Coins in Binary Tree (LC 979)](https://leetcode.com/problems/distribute-coins-in-binary-tree/) — Classic OA/Interview: Counting edge flow contributions (`abs(left_excess) + abs(right_excess)`) instead of simulating coin movement.
6. [Step-By-Step Directions From a Binary Tree Node to Another (LC 2096)](https://leetcode.com/problems/step-by-step-directions-from-a-binary-tree-node-to-another/) — Finding LCA + building path strings ('U', 'L', 'R') without storing full node arrays.
7. [Binary Tree Maximum Path Sum (LC 124)](https://leetcode.com/problems/binary-tree-maximum-path-sum/) — Bridges with your DP sheet: clamping negative branch sums to 0.
8. [Recover Binary Search Tree (LC 99)](https://leetcode.com/problems/recover-binary-search-tree/) — Identifying two swapped nodes in an inorder sequence + Morris Traversal for $O(1)$ space.

---

### 2. BFS, Multi-Source & Coordinate Views (6 Problems)
Anytime a tree problem asks for "minimum time to spread/burn" or "nodes at distance K," immediately convert the tree into an undirected graph (via parent pointers) and run BFS.

1. [All Nodes Distance K in Binary Tree (LC 863)](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/) — Converting a TreeNode into an undirected graph via a parent HashMap + BFS.
2. [Amount of Time for Binary Tree to Be Infected (LC 2385)](https://leetcode.com/problems/amount-of-time-for-binary-tree-to-be-infected/) — Single-pass DFS way to compute max distance from an arbitrary target node (faster than 2-pass BFS).
3. [Maximum Width of Binary Tree (LC 662)](https://leetcode.com/problems/maximum-width-of-binary-tree/) — Indexing binary tree nodes as a heap ($2 \times \text{idx}, 2 \times \text{idx} + 1$) with offset normalization to prevent overflow.
4. [Vertical Order Traversal of a Binary Tree (LC 987)](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/) — 2D coordinate tracking (row, col) with custom sorting rules.
5. [Populating Next Right Pointers in Each Node II (LC 117)](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/) — $O(1)$ space level-order traversal using already-established next pointers as a dummy linked list.
6. [Boundary of Binary Tree (LC 545 / LintCode 878)](https://leetcode.com/problems/boundary-of-binary-tree/) — Cleanly separating left boundary, leaf traversal, and post-order right boundary without duplicating corners.

---

### 3. Tree Construction, Serialization & Monotonic Stack (6 Problems)
Every live interview loop has at least one question on reconstructing a tree from traversals or encoding it into a string.

1. [Construct Binary Tree from Preorder and Inorder Traversal (LC 105)](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) — Subarray boundary math + HashMap index lookup.
2. [Construct Binary Tree from Preorder and Postorder Traversal (LC 889)](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/) — Understanding why multiple valid trees exist and using subtree size offsets.
3. [Construct Binary Search Tree from Preorder Traversal (LC 1008)](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/) — Crucial: Do NOT sort to get inorder ($O(N \log N)$); use a single global index with (`min_bound`, `max_bound`) in strictly $O(N)$.
4. [Serialize and Deserialize Binary Tree (LC 297)](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) — Preorder string encoding with null markers.
5. [Find Duplicate Subtrees (LC 652)](https://leetcode.com/problems/find-duplicate-subtrees/) — Subtree Hashing / Structural Serialization: Assigning integer IDs to `(left_id, val, right_id)` triplets to reduce $O(N^2)$ string hashing to $O(N)$.
6. [Recover a Tree From Preorder Traversal (LC 1028)](https://leetcode.com/problems/recover-a-tree-from-preorder-traversal/) — Iterative tree reconstruction using an explicit stack tracking node depths.

---

### 4. Binary Search Tree (BST) Augmentation & Merging (6 Problems)
Exploiting the $L < \text{Root} < R$ invariant to prune search spaces or validate subtrees bottom-up.

1. [Validate Binary Search Tree (LC 98)](https://leetcode.com/problems/validate-binary-search-tree/) — Top-down range propagation (`long min`, `long max`).
2. [Maximum Sum BST in Binary Tree (LC 1373)](https://leetcode.com/problems/maximum-sum-bst-in-binary-tree/) — Post-order Tuple Aggregation: Returning `{isBST, minVal, maxVal, sum}` from every subtree.
3. [Kth Smallest Element in a BST (LC 230)](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) — Know the follow-up: augmenting each TreeNode with `leftSubtreeSize` for $O(\text{height})$ queries and updates.
4. [Binary Search Tree Iterator (LC 173)](https://leetcode.com/problems/binary-search-tree-iterator/) — Controlled iterative inorder stack ($O(H)$ memory) for lazy stream merging.
5. [Delete Node in a BST (LC 450)](https://leetcode.com/problems/delete-node-in-a-bst/) — Recursive pointer replacement using the inorder successor.
6. [Number of Ways to Reorder Array to Get Same BST (LC 1569)](https://leetcode.com/problems/number-of-ways-to-reorder-array-to-get-same-bst/) — Combinatorics on trees: $\binom{L+R}{L} \times \text{ways}(L) \times \text{ways}(R)$ using Pascal's triangle.

---

### 5. Tree DP & Rerooting (10 Problems — Synced with DP Sheet)
Included here so your Tree tracker is 100% self-contained. Covers both TreeNode and N-ary adjacency lists.

1. [House Robber III (LC 337)](https://leetcode.com/problems/house-robber-iii/) — Pattern 1: Subtree Include / Exclude (`[not_robbed, robbed]`).
2. [Binary Tree Cameras (LC 968)](https://leetcode.com/problems/binary-tree-cameras/) — 3-state greedy/DP (`UNCOVERED`, `HAS_CAMERA`, `COVERED_NO_CAM`).
3. [Longest Path With Different Adjacent Characters (LC 2246)](https://leetcode.com/problems/longest-path-with-different-adjacent-characters/) — N-ary Tree Diameter maintaining the top-2 valid child branches.
4. [Maximum Score After Applying Operations on a Tree (LC 2925)](https://leetcode.com/problems/maximum-score-after-applying-operations-on-a-tree/) — Subtree sum vs. min bottleneck kept to keep root-to-leaf paths valid.
5. [Find the Maximum Sum of Node Values (LC 3068)](https://leetcode.com/problems/find-the-maximum-sum-of-node-values/) — Tree XOR parity DP (realizing you can flip any pair of nodes via path XOR).
6. [Sum of Distances in Tree (LC 834 / CSES Tree Distances II)](https://leetcode.com/problems/sum-of-distances-in-tree/) — The Rerooting bible: $\text{ans}[v] = \text{ans}[u] - \text{sz}[v] + (N - \text{sz}[v])$.
7. [Tree Distances I (CSES 1132)](https://cses.fi/problemset/task/1132) — Rerooting maximum distance by tracking the top-2 longest downward paths at each node.
8. [Minimum Edge Reversals So Every Node Is Reachable (LC 2858)](https://leetcode.com/problems/minimum-edge-reversals-so-every-node-is-reachable/) — Rerooting on directed edges (0/1 weights).
9. [Time Taken to Mark All Nodes (LC 3241)](https://leetcode.com/problems/time-taken-to-mark-all-nodes/) — Modern OA Rerooting with odd/even node weight transitions.
10. [Distance in Tree (Codeforces 161D)](https://codeforces.com/problemset/problem/161/D) — Subtree depth merging (`dp[u][k]`) for counting pairs at distance $K$.

---

### 6. Binary Lifting, LCA & Path Queries (7 Problems — Huge OA Filter)
Whenever an OA gives you $Q \le 10^5$ queries asking for the distance, heaviest edge, or $K$-th node on the path between $u$ and $v$ in a static tree, you MUST use Binary Lifting (`up[u][20]`).

1. [Kth Ancestor of a Tree Node (LC 1483)](https://leetcode.com/problems/kth-ancestor-of-a-tree-node/) — Base template for building the `up[N][LOG]` sparse table.
2. [Company Queries II (CSES 1688)](https://cses.fi/problemset/task/1688) & [Distance Queries (CSES 1135)](https://cses.fi/problemset/task/1135) — Computing $\text{LCA}(u, v)$ in $O(\log N)$ and path distance via $\text{depth}[u] + \text{depth}[v] - 2 \times \text{depth}[\text{lca}]$.
3. [Minimum Edge Weight Equilibrium Queries in a Tree (LC 2846)](https://leetcode.com/problems/minimum-edge-weight-equilibrium-queries-in-a-tree/) — OA Favorite: Storing prefix frequency arrays `freq[u][26]` from the root + LCA to query edge weights on any path $(u, v)$ in $O(26 + \log N)$.
4. [Counting Paths (CSES 1136)](https://cses.fi/problemset/task/1136) — Difference Array on Trees: Adding +1 to all nodes on path $(u, v)$ offline using `diff[u]++`, `diff[v]++`, `diff[lca]--`, `diff[parent(lca)]--` followed by one bottom-up DFS sum.
5. [Cycle Length Queries in a Tree (LC 2509)](https://leetcode.com/problems/cycle-length-queries-in-a-tree/) — Implicit Binary Lifting on a complete binary tree up to $2^{30}$ nodes.
6. [Checking Existence of Edge Length Limited Paths (LC 1697)](https://leetcode.com/problems/checking-existence-of-edge-length-limited-paths/) / Kruskal Reconstruction Tree — Offline MST + Binary Lifting for bottleneck edge queries.
7. [Maximize Value of Function in a Ball Passing Game (LC 2836)](https://leetcode.com/problems/maximize-value-of-function-in-a-ball-passing-game/) — Binary Lifting on Functional Graphs (directed trees + 1 cycle per component, maintaining `up[u][j]` and `sum[u][j]`).

---

### 7. Euler Tour (Tree Flattening) + Fenwick / Segment Tree (6 Problems)
How do you handle dynamic updates on a tree ("Update node $u$'s value, then query the sum/XOR of $u$'s entire subtree")? You flatten the tree into a 1D array using DFS entry/exit timestamps $[\text{tin}[u], \text{tout}[u]]$. Every subtree becomes a contiguous subarray!

1. [Subtree Queries (CSES 1137)](https://cses.fi/problemset/task/1137) — The foundational template: Euler Tour $[\text{tin}[u], \text{tout}[u]]$ + Point-Update Range-Sum Fenwick Tree (BIT).
2. [Path Queries (CSES 1138)](https://cses.fi/problemset/task/1138) — Updating node values and querying root-to-$u$ path sums using $+\text{val}$ at $\text{tin}[u]$ and $-\text{val}$ at $\text{tout}[u] + 1$ in a Fenwick Tree.
3. [Minimum Score After Removals on a Tree (LC 2322)](https://leetcode.com/problems/minimum-score-after-removals-on-a-tree/) — Using $\text{tin}[u] \le \text{tin}[v] \land \text{tout}[v] \le \text{tout}[u]$ to check if $v$ is in $u$'s subtree in $O(1)$ time while cutting two edges.
4. [Height of Binary Tree After Subtree Removal Queries (LC 2458)](https://leetcode.com/problems/height-of-binary-tree-after-subtree-removal-queries/) — Flattening via Euler Tour (or prefix/suffix max heights per depth) to answer "what is the max tree height if subtree $u$ is deleted?" in $O(1)$.
5. [Number of Nodes in the Sub-Tree With the Same Label (LC 1519)](https://leetcode.com/problems/number-of-nodes-in-the-sub-tree-with-the-same-label/) — Frequency array aggregation across DFS entry/exit boundaries.
6. [Tree and Queries (Codeforces 375D)](https://codeforces.com/problemset/problem/375/D) — Flattening a tree with Euler Tour to run offline range queries on subtrees.

---

### 8. Tree Center, Topological Pruning & Small-to-Large Merging (6 Problems)
These cover the remaining structural graph-tree patterns that appear as Q3/Q4 in Rubrik, Uber, and Google OAs.

1. [Minimum Height Trees (LC 310)](https://leetcode.com/problems/minimum-height-trees/) — Finding the 1 or 2 centroids/centers of an unrooted tree via Onion-Peeling (Leaf BFS Topological Sort).
2. [Collect Coins in a Tree (LC 2603)](https://leetcode.com/problems/collect-coins-in-a-tree/) — Multi-stage leaf pruning: trim leaves with no coins, then trim two more layers of leaves to account for distance-2 collection.
3. [Frog Position After T Seconds (LC 1377)](https://leetcode.com/problems/frog-position-after-t-seconds/) — Probability propagation on an undirected tree (handling leaf absorption vs. passing through).
4. [Number of Good Paths (LC 2421)](https://leetcode.com/problems/number-of-good-paths/) — Reverse Tree Thinking (DSU on Trees): Instead of DFS, sort nodes by value and build the tree bottom-up using Disjoint Set Union (DSU) to count valid paths in $O(N \log N)$.
5. [Smallest Missing Genetic Value in Each Subtree (LC 2003)](https://leetcode.com/problems/smallest-missing-genetic-value-in-each-subtree/) — Walking strictly up the ancestor chain from the node with value 1 ($O(N)$ total work) instead of merging sets at every node.
6. [Distinct Colors (CSES 1139)](https://cses.fi/problemset/task/1139) — Small-to-Large Merging (DSU on Tree / Sack): Merging child HashSets into the parent by always swapping the larger Set to the parent reference (`if (childSet.size() > parentSet.size()) swap`). Turns $O(N^2)$ set merging into strictly $O(N \log^2 N)$.

---

### The "OA Tree Arsenal" Summary
When you sit for an OA and see $N$ nodes and $N-1$ edges, you only ever need to ask yourself which of the 5 OA Hammers applies:

1. **Need best answer for every root?** $\rightarrow$ Rerooting DP (#32–35)
2. **Static tree + $Q$ path queries between $(u, v)$?** $\rightarrow$ Binary Lifting LCA + Prefix Counts from Root (#37–39)
3. **Offline path increments ($+X$ to all nodes on path $u \leftrightarrow v$)?** $\rightarrow$ Tree Difference Array `diff[u]++`, `diff[v]++`, `diff[lca]--` (#40)
4. **Dynamic node updates + Subtree queries?** $\rightarrow$ Euler Tour $[\text{tin}, \text{tout}]$ + Fenwick Tree (#44–46)
5. **Paths constrained by "Maximum Node Value on Path"?** $\rightarrow$ Sort edges/nodes by value + DSU `union` (#53) or Small-to-Large Set Merging (#55).
