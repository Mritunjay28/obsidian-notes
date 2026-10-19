---
share: true
---

### 1. Sliding Window & Two-Pointer Tricks (15 Problems)
Standard sliding window ONLY works when expanding the window (`right++`) monotonically increases a metric and shrinking (`left++`) monotonically decreases it. When a problem asks for exact counts or three pointers, you must use the `atMost(K) - atMost(K - 1)` identity.

#### A. Variable Window & Frequency State
1. [Minimum Window Substring (LC 76)](https://leetcode.com/problems/minimum-window-substring/) — The canonical `have == need` frequency map shrinking template.
2. [Longest Repeating Character Replacement (LC 424)](https://leetcode.com/problems/longest-repeating-character-replacement/) — Maintaining `maxFreq` inside the window without scanning the 26-char array on shrink.
3. [Permutation in String (LC 567)](https://leetcode.com/problems/permutation-in-string/) / [Find All Anagrams in a String (LC 438)](https://leetcode.com/problems/find-all-anagrams-in-a-string/) — Fixed-size window maintaining a single `matches == 26` counter for $O(1)$ transitions.
4. [Substring with Concatenation of All Words (LC 30)](https://leetcode.com/problems/substring-with-concatenation-of-all-words/) — Running $M$ independent sliding windows (where $M = \text{wordLen}$) at offsets $0, 1, \dots, M-1$.
5. [Longest Substring with At Least K Repeating Characters (LC 395)](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/) — Crucial Trick: You cannot slide a single window directly; iterate over $\text{targetUnique} = 1 \text{ to } 26$ and run a sliding window for each.
6. [Frequency of the Most Frequent Element (LC 1838)](https://leetcode.com/problems/frequency-of-the-most-frequent-element/) — Sorting + Sliding Window: Sort the array first, then maintain the window cost to raise all elements in $[L \dots R]$ to $\text{nums}[R]$ via $(R - L + 1) \times \text{nums}[R] - \text{windowSum} \le K$.
7. [Minimum Operations to Reduce X to Zero (LC 1658)](https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/) — Reverse Thinking: Minimizing prefix + suffix elements summing to $X$ is identical to maximizing the middle subarray summing to $\text{TotalSum} - X$.

#### B. The AtMost(K) - AtMost(K - 1) & Multi-Pointer Pattern (OA Favorites)
1. [Subarrays with K Different Integers (LC 992)](https://leetcode.com/problems/subarrays-with-k-different-integers/) — Blueprint for $\text{exactly}(K) = \text{atMost}(K) - \text{atMost}(K - 1)$.
2. [Count Number of Nice Subarrays (LC 1248)](https://leetcode.com/problems/count-number-of-nice-subarrays/) / [Binary Subarrays With Sum (LC 930)](https://leetcode.com/problems/binary-subarrays-with-sum/) — Counting exact conditions using either $\text{atMost}(K) - \text{atMost}(K-1)$ or a 3-pointer window (`left_min`, `left_max`, `right`).
3. [Count Subarrays With Fixed Bounds (LC 2444)](https://leetcode.com/problems/count-subarrays-with-fixed-bounds/) — OA Staple: Tracking `lastMinK`, `lastMaxK`, and `lastInvalidIdx` in a single pass: `ans += max(0, min(lastMinK, lastMaxK) - lastInvalidIdx)`.
4. [Count Complete Subarrays in an Array (LC 2799)](https://leetcode.com/problems/count-complete-subarrays-in-an-array/) / [Number of Substrings Containing All Three Characters (LC 1358)](https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/) — Counting via $N - \text{right}$ whenever the window becomes valid.

#### C. Dual-Heap & TreeSet Sliding Windows
1. [Sliding Window Median (LC 480)](https://leetcode.com/problems/sliding-window-median/) — Maintaining a lazy-deletion `maxHeap` (left half) and `minHeap` (right half) (or two `TreeSet<Integer>` instances comparing indices) as elements enter and leave.
2. [Divide an Array Into Subarrays With Minimum Cost II (LC 3013)](https://leetcode.com/problems/divide-an-array-into-subarrays-with-minimum-cost-ii/) — OA Boss: Maintaining the sum of the smallest $K-1$ elements inside a sliding window of size $D$ using two TreeSets (`kSmallest` and `remaining`).
3. [Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit (LC 1438)](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/) — Sliding window where validity depends on $\max - \min \le \text{limit}$ (bridges into Two Monotonic Deques).
4. [Maximize the Confusion of an Exam (LC 2024)](https://leetcode.com/problems/maximize-the-confusion-of-an-exam/) / [Max Consecutive Ones III (LC 1004)](https://leetcode.com/problems/max-consecutive-ones-iii/) — Binary flips inside a window.

---

### 2. Prefix Sums, Difference Arrays & Prefix Bitmasks (16 Problems)
Never recompute range sums, range XORs, or range increments. In OAs, Prefix Sum is combined with Modulo Arithmetic, Bitmask Parity, or Coordinate-Shifted HashMaps.

#### A. Prefix Sum + HashMap & Modulo Identities
1. [Subarray Sum Equals K (LC 560)](https://leetcode.com/problems/subarray-sum-equals-k/) — Base `map.getOrDefault(currSum - K, 0)` template (works with negative numbers where sliding window fails!).
2. [Subarray Sums Divisible by K (LC 974)](https://leetcode.com/problems/subarray-sums-divisible-by-k/) — Handling negative modulo in Java: `int rem = ((currSum % K) + K) % K`.
3. [Continuous Subarray Sum (LC 523)](https://leetcode.com/problems/continuous-subarray-sum/) — Storing the first seen index of `currSum % K` in a HashMap to enforce subarray length $\ge 2$.
4. [Make Sum Divisible by P (LC 1590)](https://leetcode.com/problems/make-sum-divisible-by-p/) — Finding the shortest subarray whose removal makes the remaining sum divisible by $P$: target remainder `need = (currRem - totalRem + P) % P`.
5. [Contiguous Array (LC 525)](https://leetcode.com/problems/contiguous-array/) — Replacing 0 with -1 to turn equal 0/1 counts into "longest subarray with sum 0".
6. [Number of Submatrices That Sum to Target (LC 1074)](https://leetcode.com/problems/number-of-submatrices-that-sum-to-target/) — 2D-to-1D Compression: Fix `topRow` and `bottomRow` in $O(R^2)$, compress columns into a 1D array, and run Subarray Sum Equals K in $O(C)$ for $O(R^2 \cdot C)$ total time.

#### B. Prefix XOR & Bitmask Parity (Huge OA Filter)
1. [Count Triplets That Can Form Two Arrays of Equal XOR (LC 1442)](https://leetcode.com/problems/count-triplets-that-can-form-two-arrays-of-equal-xor/) — Observing that $a = b \iff a \oplus b = 0$, reducing triplet counting to $O(N)$ prefix XOR index math.
2. [Find the Longest Substring Containing Vowels in Even Counts (LC 1371)](https://leetcode.com/problems/find-the-longest-substring-containing-vowels-in-even-counts/) — 5-Bit Prefix Mask: Compress the even/odd parity of 'a','e','i','o','u' into a 5-bit mask (0 to 31) and find the first occurrence of the same mask.
3. [Find Longest Awesome Substring (LC 1542)](https://leetcode.com/problems/find-longest-awesome-substring/) / [Number of Wonderful Substrings (LC 1915)](https://leetcode.com/problems/number-of-wonderful-substrings/) — 10-Bit Palindrome Prefix Mask: A substring can form a palindrome iff at most 1 character has an odd count. Check `seen[mask]` (0 odd counts) and `seen[mask ^ (1 << bit)]` for all 10 bits (1 odd count).

#### C. 1D & 2D Difference Arrays (Sweep Line) & Prefix Math
1. [Corporate Flight Bookings (LC 1109)](https://leetcode.com/problems/corporate-flight-bookings/) / [Car Pooling (LC 1094)](https://leetcode.com/problems/car-pooling/) — 1D Difference Array (`diff[L] += val; diff[R + 1] -= val;`).
2. [Shifting Letters II (LC 2381)](https://leetcode.com/problems/shifting-letters-ii/) — Modulo-26 range shifts via difference arrays.
3. [Increment Submatrices by One (LC 2536)](https://leetcode.com/problems/increment-submatrices-by-one/) — 2D Difference Array: `diff[r1][c1]++`, `diff[r1][c2+1]--`, `diff[r2+1][c1]--`, `diff[r2+1][c2+1]++`, followed by 2D prefix sum reconstruction.
4. [Stamping the Grid (LC 2132)](https://leetcode.com/problems/stamping-the-grid/) — Ultimate 2D Prefix Sum + 2D Difference Array Combo: Use 2D Prefix Sum to check in $O(1)$ if an $H \times W$ stamp fits without hitting an 'X', then mark coverage using a 2D Difference Array.
5. [Product of Array Except Self (LC 238)](https://leetcode.com/problems/product-of-array-except-self/) — Prefix and Suffix running accumulations without division.
6. [Sum of Absolute Differences in a Sorted Array (LC 1685)](https://leetcode.com/problems/sum-of-absolute-differences-in-a-sorted-array/) / [Minimum Cost to Make Array Equal (LC 2448)](https://leetcode.com/problems/minimum-cost-to-make-array-equal/) — Calculating $\sum \vert{}A[i] - A[j]\vert{}$ in $O(1)$ per index by splitting into left prefix sum and right suffix sum: `(i * A[i] - prefix[i-1]) + (suffix[i+1] - (N - 1 - i) * A[i])`.
7. [Count of Range Sum (LC 327)](https://leetcode.com/problems/count-of-range-sum/) — Synced with DP list: When you need to count how many previous prefix sums fall in $[\text{currSum} - \text{upper}, \text{currSum} - \text{lower}]$ with negative numbers, HashMap isn't enough—use Merge Sort (Divide & Conquer) or Fenwick Tree on Prefix Sums.

---

### 3. Monotonic Stack, Contribution Technique & Parsing (17 Problems)
A Monotonic Stack finds the Nearest Smaller/Greater Element to the Left and Right of every index $i$ in strictly $O(N)$ time. In OAs, this is the secret weapon for the Contribution Technique: instead of iterating over all $O(N^2)$ subarrays and finding their min/max, ask: *"In how many subarrays is $A[i]$ the minimum?"*

#### A. Next Greater / Smaller & Histogram Foundations
1. [Daily Temperatures (LC 739)](https://leetcode.com/problems/daily-temperatures/) & [Next Greater Element II (LC 503)](https://leetcode.com/problems/next-greater-element-ii/) — Basic monotonic decreasing stack + $2 \times N$ modulo loop for circular arrays.
2. [Online Stock Span (LC 901)](https://leetcode.com/problems/online-stock-span/) — Compressing `(price, count)` pairs inside the stack.
3. [Trapping Rain Water (LC 42)](https://leetcode.com/problems/trapping-rain-water/) — Know both the Two-Pointer $O(1)$ space approach (vertical water bars) AND the Monotonic Stack approach (horizontal water layers bounded by left/right walls).
4. [Largest Rectangle in Histogram (LC 84)](https://leetcode.com/problems/largest-rectangle-in-histogram/) & [Maximal Rectangle (LC 85)](https://leetcode.com/problems/maximal-rectangle/) — Finding `prevSmaller[i]` and `nextSmaller[i]` to bound rectangle widths: `(R - L - 1) * H[i]`.
5. [Maximum Score of a Good Subarray (LC 1793)](https://leetcode.com/problems/maximum-score-of-a-good-subarray/) — Histogram rectangle constrained to include index $K$ (solve via Monotonic Stack or greedy two-pointer expansion from $K$).
6. [Maximum Subarray Min-Product (LC 1856)](https://leetcode.com/problems/maximum-subarray-min-product/) — Monotonic stack to find `[prevSmaller + 1, nextSmaller - 1]` + Prefix Sum to maximize $A[i] \times \text{rangeSum}$.

#### B. The Contribution Technique (Absolute #1 OA Stack Pattern)
1. [Sum of Subarray Minimums (LC 907)](https://leetcode.com/problems/sum-of-subarray-minimums/) — Counting subarrays where $A[i]$ is minimum via `(i - prevSmaller[i]) * (nextSmaller[i] - i)`. Critical Duplicate Rule: Use strict `<` on one side and non-strict $\le$ on the other so duplicate minimums aren't double-counted!
2. [Sum of Subarray Ranges (LC 2104)](https://leetcode.com/problems/sum-of-subarray-ranges/) — Computing $\sum (\text{Max} - \text{Min}) = \sum \text{SubarrayMax} - \sum \text{SubarrayMin}$ in $O(N)$ using two monotonic stacks.
3. [Sum of Total Strength of Wizards (LC 2281)](https://leetcode.com/problems/sum-of-total-strength-of-wizards/) — Amazon/Uber OA Boss: Combines Monotonic Stack bounds $(L, R)$ with Prefix Sum of Prefix Sums to sum all subarray sums containing index $i$ in $O(1)$.

#### C. Lexicographical Greedy & Non-Adjacent Stack Jumps
1. [Remove K Digits (LC 402)](https://leetcode.com/problems/remove-k-digits/) — Monotonic increasing stack to form the smallest possible number.
2. [Remove Duplicate Letters (LC 316)](https://leetcode.com/problems/remove-duplicate-letters/) — Monotonic stack + `lastIndex[char]` + `inStack[char]` boolean array to build the lexicographically smallest unique subsequence.
3. [Maximum Width Ramp (LC 962)](https://leetcode.com/problems/maximum-width-ramp/) — Two-Phase Stack Trick: When looking for the farthest greater element $j > i$ (not the nearest), push a strictly decreasing prefix onto the stack in pass 1, then sweep $j$ backward from $N-1 \to 0$ popping valid starts in pass 2.
4. [132 Pattern (LC 456)](https://leetcode.com/problems/132-pattern/) — Sweeping right-to-left while maintaining a monotonic stack for the '3' candidate and a tracked maximum for the '2' (third) candidate.
5. [Car Fleet (LC 853)](https://leetcode.com/problems/car-fleet/) & [Car Fleet II (LC 1776)](https://leetcode.com/problems/car-fleet-ii/) — Sorting by position + monotonic stack tracking collision times in floating point.

#### D. Expression Evaluation & Nested State Stacks
1. [Basic Calculator (LC 224)](https://leetcode.com/problems/basic-calculator/) & [Basic Calculator II (LC 227)](https://leetcode.com/problems/basic-calculator-ii/) — Handling `( )` sign reversals and `*` / `/` operator precedence with a single stack.
2. [Decode String (LC 394)](https://leetcode.com/problems/decode-string/) / [Number of Atoms (LC 726)](https://leetcode.com/problems/number-of-atoms/) — Pushing `(prevString, repeatCount)` or `Map<String, Integer>` frames onto a stack when encountering `[` or `(`.
3. [Longest Valid Parentheses (LC 32)](https://leetcode.com/problems/longest-valid-parentheses/) — Pushing index -1 as a base sentinel on the stack (or 2-pass left/right counter scan in $O(1)$ space).

---

### 4. Queues & Monotonic Deque (12 Problems)
In Java, always use `ArrayDeque<Integer>` (storing indices, never raw values!) for both stacks and queues. The Monotonic Deque is the single most important queue structure for OAs—it solves sliding window min/max AND fixes sliding windows broken by negative numbers.

#### A. Monotonic Deque (Sliding Window Min/Max & Prefix Sums with Negatives)
1. [Sliding Window Maximum (LC 239)](https://leetcode.com/problems/sliding-window-maximum/) — Base template: `pollFirst()` if `dq.peekFirst() <= i - K` (out of window), `pollLast()` while `nums[dq.peekLast()] <= nums[i]` (maintain decreasing order).
2. [Shortest Subarray with Sum at Least K (LC 862)](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/) — THE Most Important OA Deque Problem: Standard sliding window fails because of negative numbers. Compute Prefix Sums $P[]$, then use a Monotonic Increasing Deque on $P[]$ to find the largest $j < i$ where $P[i] - P[j] \ge K$ in $O(N)$.
3. [Max Value of Equation (LC 1499)](https://leetcode.com/problems/max-value-of-equation/) — Algebraic Separation + Monotonic Deque: Rewrite $y_i + y_j + \vert{}x_i - x_j\vert{}$ (for $x_j < x_i$) as $(y_i + x_i) + (y_j - x_j)$. Maintain the max of $(y_j - x_j)$ in a Monotonic Deque where $x_i - x_j \le K$.
4. [Maximum Sum Circular Subarray (LC 918)](https://leetcode.com/problems/maximum-sum-circular-subarray/) — Know both Kadane's (`max(maxSubarray, totalSum - minSubarray)`) and the $2N$ Prefix Sum + Monotonic Deque approach (which generalizes to bounded length $\le K$).
5. [Jump Game VI (LC 1696)](https://leetcode.com/problems/jump-game-vi/) & [Constrained Subsequence Sum (LC 1425)](https://leetcode.com/problems/constrained-subsequence-sum/) — Synced with DP Sheet: Monotonic Deque optimizing $dp[i] = A[i] + \max_{j=i-K}^{i-1} dp[j]$.
6. [Maximum Number of Robots Within Budget (LC 2398)](https://leetcode.com/problems/maximum-number-of-robots-within-budget/) — Combining Variable Sliding Window (`left++` when over budget) with a Monotonic Decreasing Deque tracking $\max(\text{chargeTimes})$ inside the current window.
7. [Continuous Subarrays (LC 2762)](https://leetcode.com/problems/continuous-subarrays/) / [Delivering Boxes from Storage to Ports (LC 1687)](https://leetcode.com/problems/delivering-boxes-from-storage-to-ports/) — Advanced Monotonic Deque window bounding.

#### B. Simulation, Priority Scheduling & Circular Queue Invariants
1. [Task Scheduler (LC 621)](https://leetcode.com/problems/task-scheduler/) — Pairing a PriorityQueue (most frequent available tasks) with a cooldown `Queue<int[]>` storing `[remainingCount, availableAtTime]` (plus the $O(N)$ math formula `(maxFreq - 1) * (n + 1) + countOfMax`).
2. [Design Circular Queue (LC 622)](https://leetcode.com/problems/design-circular-queue/) & [LRU Cache (LC 146)](https://leetcode.com/problems/lru-cache/) — Live interview system-design staples (`head`, `tail`, `size` modulo arithmetic, and Doubly Linked List + HashMap).
3. [Reveal Cards In Increasing Order (LC 950)](https://leetcode.com/problems/reveal-cards-in-increasing-order/) — Simulating index positions using a Queue to reverse-engineer the initial deck order.
4. [Dota2 Senate (LC 649)](https://leetcode.com/problems/dota2-senate/) — Two-Queue index simulation (`radiant.offer(r_idx + N)`) to model greedy circular ban rounds.
5. [Find the Winner of the Circular Game (LC 1823)](https://leetcode.com/problems/find-the-winner-of-the-circular-game/) — Queue simulation vs. the $O(N)$ iterative Josephus recurrence `ans = (ans + K) % i`.

---

### The "Subarray Decision Tree" (Instant OA Pattern Matcher)
Whenever an OA asks you to find the shortest, longest, or number of subarrays satisfying a sum/XOR/min/max condition, run through this exact 5-second checklist:

1. **Are all numbers non-negative ($\ge 0$) and condition is $\le K$ or $\ge K$?**  
   $\rightarrow$ Standard Two-Pointer Sliding Window (#1–7) in $O(N)$ time, $O(1)$ space.
2. **Are all numbers non-negative, but condition asks for EXACTLY $K$?**  
   $\rightarrow$ Rewrite as $\text{atMost}(K) - \text{atMost}(K - 1)$ (#8–9) using two sliding windows.
3. **Do numbers include NEGATIVES and it asks for $\text{sum} == K$ or $\text{sum} \pmod K == 0$?**  
   $\rightarrow$ Sliding window is dead. Use Prefix Sum + HashMap (#16–20).
4. **Do numbers include NEGATIVES and it asks for $\text{sum} \ge K$ (Shortest) or $\text{length} \le W$ (Max Sum)?**  
   $\rightarrow$ Prefix Sum + Monotonic Deque (#50, #52).
5. **Does the formula involve $\min(\text{subarray})$ or $\max(\text{subarray})$ across ALL subarrays?**  
   $\rightarrow$ Monotonic Stack Contribution Technique: `(i - prev) * (next - i)` (#37–40).

---

### 5. Binary Search on Answer & Rotated Arrays (8 Problems)
OAs love asking "Minimize the maximum $X$" or "Find the $K$-th smallest $Y$". If you try to write DP for these, you get TLE; they require Binary Search on the answer space.

1. [Search in Rotated Sorted Array (LC 33)](https://leetcode.com/problems/search-in-rotated-sorted-array/) & [Find Minimum in Rotated Sorted Array (LC 153)](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) — Live interview staples.
2. [Median of Two Sorted Arrays (LC 4)](https://leetcode.com/problems/median-of-two-sorted-arrays/) — Partitioning two arrays in $O(\log(\min(N, M)))$.
3. [Koko Eating Bananas (LC 875)](https://leetcode.com/problems/koko-eating-bananas/) / [Capacity To Ship Packages Within D Days (LC 1011)](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) — Base `check(mid)` monotonic predicate template.
4. [Minimize Max Distance to Gas Station (LC 774)](https://leetcode.com/problems/minimize-max-distance-to-gas-station/) / [Aggressive Cows (SPOJ - AGGRCOW)](https://www.spoj.com/problems/AGGRCOW/) — Binary search on min/max distance.
5. [Find K-th Smallest Pair Distance (LC 719)](https://leetcode.com/problems/find-k-th-smallest-pair-distance/) — Binary Search on Answer + Two-Pointer counting inside `check(mid)`.
6. [Kth Smallest Number in Multiplication Table (LC 668)](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/) — Binary search on value space.
7. [Maximum Running Time of N Computers (LC 2141)](https://leetcode.com/problems/maximum-running-time-of-n-computers/) — Greedy + Binary search on answer.
8. [Find Peak Element (LC 162)](https://leetcode.com/problems/find-peak-element/) — Binary search on non-sorted local slopes.

---

### 6. Heaps, Greedy Intervals & Sweep Line (8 Problems)
In OAs, whenever intervals overlap or resources must be scheduled dynamically without DP, it's either a Sweep Line or a Two-Heap problem.

1. [Merge Intervals (LC 56)](https://leetcode.com/problems/merge-intervals/) & [Non-overlapping Intervals (LC 435)](https://leetcode.com/problems/non-overlapping-intervals/) — Sorting by end time vs. start time.
2. [Meeting Rooms II (LC 253)](https://leetcode.com/problems/meeting-rooms-ii/) — Min-Heap / Sweep line (+1 at start, -1 at end).
3. [Find Median from Data Stream (LC 295)](https://leetcode.com/problems/find-median-from-data-stream/) — Two-heap balancing (`maxHeap` for left half, `minHeap` for right half).
4. [IPO (LC 502)](https://leetcode.com/problems/ipo/) — Two-Heap / Sort + PQ Greedy: Sort projects by capital needed, push affordable ones into a max-profit PriorityQueue. Very common in OAs.
5. [Maximum Performance of a Team (LC 1383)](https://leetcode.com/problems/maximum-performance-of-a-team/) — Fix Minimum + Min-Heap: Sort by efficiency descending so current element is always the bottleneck, and maintain top $K-1$ speeds in a Min-Heap.
6. [Minimum Interval to Include Each Query (LC 1851)](https://leetcode.com/problems/minimum-interval-to-include-each-query/) — Offline sorting of queries + Min-Heap lazy cleanup.
7. [Gas Station (LC 134)](https://leetcode.com/problems/gas-station/) & [Candy (LC 135)](https://leetcode.com/problems/candy/) — Classic 1D greedy invariants.
8. [The Skyline Problem (LC 218)](https://leetcode.com/problems/the-skyline-problem/) — Coordinate sweep line with a TreeMap / Max-Heap.

---

### 7. Tries, Bitwise Trie & String Hashing (7 Problems)
You already have KMP + DP and Trie + DP in your DP sheet. You just need pure Bitwise Trie (for Maximum XOR queries in OAs) and Rolling Hash.

1. [Implement Trie (Prefix Tree) (LC 208)](https://leetcode.com/problems/implement-trie-prefix-tree/) & [Word Search II (LC 212)](https://leetcode.com/problems/word-search-ii/) — Pruning Backtracking on a board using a Trie.
2. [Maximum XOR of Two Numbers in an Array (LC 421)](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/) — Bitwise Trie (0/1 Trie): Inserting 31-bit integers and walking the opposite bit greedily.
3. [Maximum XOR With an Element From Array (LC 1707)](https://leetcode.com/problems/maximum-xor-with-an-element-from-array/) — Offline Query Sorting + Bitwise Trie.
4. [Count Prefix and Suffix Pairs II (LC 3045)](https://leetcode.com/problems/count-prefix-and-suffix-pairs-ii/) — Trie where each node transitions on a pair `(charFromFront, charFromBack)` (or Z-algorithm).
5. [Longest Duplicate Substring (LC 1044)](https://leetcode.com/problems/longest-duplicate-substring/) — Binary Search on Length + Rabin-Karp Rolling Hash (the standard OA alternative to Suffix Arrays).
6. [Shortest Palindrome (LC 214)](https://leetcode.com/problems/shortest-palindrome/) — Using KMP's `lps` array on `s + "#" + reverse(s)` to find the longest palindromic prefix in $O(N)$.
7. [Single Number II (LC 137)](https://leetcode.com/problems/single-number-ii/) & [Single Number III (LC 260)](https://leetcode.com/problems/single-number-iii/) — Bitwise ones/twos state machine and lowest set bit `xor & -xor` partitioning.

---

### 8. Linked Lists & Backtracking (7 Problems — Strictly for Live Interviews)
OAs never ask Linked Lists, and rarely ask pure Backtracking (since constraints are small and turn into Bitmask DP). But Live Interviewers LOVE Linked Lists and Backtracking because they test pointer bugs and recursion trees on a whiteboard.

1. [Reverse Nodes in k-Group (LC 25)](https://leetcode.com/problems/reverse-nodes-in-k-group/) — The ultimate pointer manipulation test. If you can code this bug-free, you know Linked Lists.
2. [Copy List with Random Pointer (LC 138)](https://leetcode.com/problems/copy-list-with-random-pointer/) — Interleaving nodes in $O(1)$ space.
3. [Merge k Sorted Lists (LC 23)](https://leetcode.com/problems/merge-k-sorted-lists/) & [Linked List Cycle II (LC 142)](https://leetcode.com/problems/linked-list-cycle-ii/) — Divide & Conquer merge and Floyd's Tortoise & Hare math.
4. [Subsets II (LC 90)](https://leetcode.com/problems/subsets-ii/) & [Permutations II (LC 47)](https://leetcode.com/problems/permutations-ii/) — Handling duplicates in recursion (`if (i > start && nums[i] == nums[i-1]) continue`).
5. [N-Queens (LC 51)](https://leetcode.com/problems/n-queens/) & [Sudoku Solver (LC 37)](https://leetcode.com/problems/sudoku-solver/) — Bitmask row/col/diagonal state pruning in backtracking.
6. [Palindrome Partitioning (LC 131)](https://leetcode.com/problems/palindrome-partitioning/) — Backtracking + string slicing.
7. [LFU Cache (LC 460)](https://leetcode.com/problems/lfu-cache/) — Combined with LRU Cache (from your Queue sheet), covers 99% of in-memory data-structure design rounds.
