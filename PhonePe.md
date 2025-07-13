# PhonePe - Super Dream Offer Placement Questions


# 🏃‍♂️ Paris Olympics – Hurdle Challenge

## 📝 Problem Statement

The **Paris Olympics** committee has introduced a new game:

* There are `n` **ordered hurdles**.
* Each hurdle has a **non-negative integer score**.
* The athlete is allowed to **keep at most `k` consecutive hurdles**.
* The athlete aims to **remove hurdles** such that no more than `k` consecutive hurdles are left in the final configuration.
* The goal is to **maximize the total score of the remaining (kept) hurdles**.

### ❗ Constraints

* Hurdles are **ordered and cannot be reordered**.
* You can keep **any subset** of hurdles, but **not more than `k` consecutive**.
* You may **skip (remove)** as many hurdles as needed to satisfy the above rule.

---

## 📥 Input Format

```
First line: Two space-separated integers n (number of hurdles) and k (maximum consecutive hurdles allowed)
Next n lines: Each line contains an integer — score of the i-th hurdle
```

### Constraints:

* `1 ≤ n ≤ 10^5`
* `1 ≤ k ≤ n`
* `0 ≤ score[i] ≤ 2 * 10^9`

---

## 📤 Output Format

```
Print a single integer — the maximum score achievable while keeping at most k consecutive hurdles.
```

---

## 📘 Sample Input 0

```
6 2
1
2
3
1
6
10
```

### ✅ Sample Output 0

```
21
```

### 🔍 Explanation

* You can keep \[2, 3], skip 1, then keep \[6, 10] → total = `2 + 3 + 6 + 10 = 21`
* Any attempt to keep 3 or more consecutive hurdles (e.g., \[1,2,3]) is **invalid**.

---

## 📘 Sample Input 1

```
5 4
1
2
3
4
5
```

### ✅ Sample Output 1

```
14
```

### 🔍 Explanation

* Keep \[2, 3, 4, 5] → total = `2 + 3 + 4 + 5 = 14`
* \[1, 2, 3, 4, 5] is invalid (5 consecutive kept > 4)

---

## 🔑 Key Points

* You must **break long stretches** (more than `k` hurdles) by removing hurdles.
* Choose when to **skip** a hurdle wisely to **maximize total score**.
* Use **Dynamic Programming** to track the current best total at each position for different streak lengths.

---

## 🧠 Approach (Dynamic Programming)

### Define State:

* Let `dp[i][j]` be the **maximum score** up to index `i` with **j consecutive hurdles kept**.
* `j` can range from `0` to `k`.

### Transition:

* From `dp[i][j]`:

  * **Skip** current hurdle → move to `dp[i+1][0]`
  * **Keep** current hurdle if `j < k` → move to `dp[i+1][j+1]` and add current score

### Base Case:

* `dp[0][0] = 0` (starting with 0 score and no streak)

### Final Answer:

* The answer is the **maximum** among all `dp[n][j]` for `j = 0 to k`

---

## ✅ Java Code

```java
import java.util.*;

public class HurdleMaxScore {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int k = sc.nextInt();
        int[] hurdles = new int[n];
        for (int i = 0; i < n; i++) {
            hurdles[i] = sc.nextInt();
        }

        long[][] dp = new long[n + 1][k + 1];

        for (int i = 0; i <= n; i++) {
            Arrays.fill(dp[i], Long.MIN_VALUE);
        }

        dp[0][0] = 0; // Starting point

        for (int i = 0; i < n; i++) {
            for (int j = 0; j <= k; j++) {
                // Skip current hurdle — reset streak
                dp[i + 1][0] = Math.max(dp[i + 1][0], dp[i][j]);

                // Keep current hurdle — extend streak if possible
                if (j < k) {
                    dp[i + 1][j + 1] = Math.max(dp[i + 1][j + 1], dp[i][j] + hurdles[i]);
                }
            }
        }

        long maxScore = 0;
        for (int j = 0; j <= k; j++) {
            maxScore = Math.max(maxScore, dp[n][j]);
        }

        System.out.println(maxScore);
    }
}
```
## Woking Code

```Java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        // Read n and the list of numbers
        int n = Integer.parseInt(br.readLine().trim());
        StringTokenizer st = new StringTokenizer(br.readLine());
        List<Long> initial = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            initial.add(Long.parseLong(st.nextToken()));
        }

        // Sort ascending so that each time Harsh can split off the smallest element
        Collections.sort(initial);

        // We'll use a queue to simulate the rounds:
        // each element is a subsequence that Pranay will sum
        Queue<List<Long>> q = new ArrayDeque<>();
        q.add(initial);

        long totalPoints = 0;

        while (!q.isEmpty()) {
            List<Long> curr = q.poll();

            // Pranay sums up the current list
            long sum = 0;
            for (long x : curr) sum += x;
            totalPoints += sum;

            // Harsh’s turn: if more than one element, split it
            int size = curr.size();
            if (size > 1) {
                if (size == 2) {
                    // Only one way to split two elements
                    q.add(Collections.singletonList(curr.get(0))); // subsequence [first]
                    q.add(Collections.singletonList(curr.get(1))); // subsequence [second]
                } else {
                    // Split off the smallest element to maximize future sums
                    List<Long> splitOff = Collections.singletonList(curr.get(0));
                    List<Long> rest = curr.subList(1, size); // the other n-1 elements
                    // Harsh gives 'rest' first, then 'splitOff'
                    q.add(new ArrayList<>(rest));
                    q.add(new ArrayList<>(splitOff));
                }
            }
            // If size == 1, Harsh throws it out (we simply do nothing further)
        }

        // Print the total points Pranay accumulated
        System.out.println(totalPoints);
    }
}
```
---

## 🧮 Dry Run: Sample Input 0

```
6 2
1 2 3 1 6 10
```

* Keep: \[2, 3], skip 1, keep: \[6, 10] → Total = 21 ✅

---

## ⏱ Time and Space Complexity

* **Time Complexity**: `O(n * k)`
* **Space Complexity**: `O(n * k)` (can be optimized to `O(k)`)

---
Let's **explain the line-by-line working of the DP solution** for the **Paris Olympics Hurdle Challenge** with:

* **Visual diagrams**
* **Line-by-line walkthrough**
* **Sample Input:**

```
n = 6, k = 2
Hurdles = [1, 2, 3, 1, 6, 10]
```

---

## 🧠 Problem Recap

* You can **keep at most `k` consecutive hurdles**.
* You want to **maximize** the **total score** from selected hurdles.
* You can **skip** hurdles to **reset the streak**.

---

## ✅ Line-by-Line Code Explanation

```java
Scanner sc = new Scanner(System.in);
int n = sc.nextInt();         // Number of hurdles
int k = sc.nextInt();         // Max allowed consecutive hurdles
int[] hurdles = new int[n];   // Array to store hurdle scores
```

* `n = 6`
* `k = 2`
* Input: 1 2 3 1 6 10

```java
for (int i = 0; i < n; i++) {
    hurdles[i] = sc.nextInt();  // Read each hurdle score
}
```

Now `hurdles = [1, 2, 3, 1, 6, 10]`

---

```java
long[][] dp = new long[n + 1][k + 1];
```

* `dp[i][j]` represents the **maximum score possible after processing the first `i` hurdles**, where the last `j` hurdles are selected consecutively.

* Dimensions:

  * `dp[0...n][0...k]`
  * i.e., `dp[0][0], dp[0][1], ..., dp[n][k]`

---

```java
for (int i = 0; i <= n; i++) {
    Arrays.fill(dp[i], Long.MIN_VALUE);
}
```

* Initially, all states are invalid (worst case), except the base case.

```java
dp[0][0] = 0;
```

### 🟢 Base Case:

* At the **start**, you’ve processed **0 hurdles**, and the streak is **0**.
* So the best score = **0**.

```
dp[0][0] = 0
```

---

### 💡 DP Transition

```java
for (int i = 0; i < n; i++) {          // i = current hurdle index
    for (int j = 0; j <= k; j++) {     // j = current streak length
```

We loop through each position and possible streak length.

---

#### 📘 Decision 1: Skip the hurdle

```java
dp[i + 1][0] = Math.max(dp[i + 1][0], dp[i][j]);
```

* If you **skip** hurdle `i`, streak resets to 0.
* The max score is updated by carrying forward the previous score.

#### 📘 Decision 2: Keep the hurdle (if streak < k)

```java
if (j < k) {
    dp[i + 1][j + 1] = Math.max(dp[i + 1][j + 1], dp[i][j] + hurdles[i]);
}
```

* If current streak `j < k`, you can keep the hurdle.
* New score = previous score + current hurdle value.
* Update next state `dp[i+1][j+1]`.

---

## 🧮 Let's Visualize With Sample Input

```
Input: 6 2
Hurdles: [1, 2, 3, 1, 6, 10]
```

We process hurdle-by-hurdle. We'll track only the best values for `j = 0, 1, 2` at each step.

---

### 🧾 Step-by-Step Table (Partial):

| Hurdle Index `i` | Current Hurdle | Streak j | dp\[i]\[j] | Action               | dp\[i+1]\[j']  |
| ---------------- | -------------- | -------- | ---------- | -------------------- | -------------- |
| 0                | 1              | 0        | 0          | Skip                 | dp\[1]\[0] = 0 |
|                  |                |          |            | Take                 | dp\[1]\[1] = 1 |
| 1                | 2              | 0        | 0          | Skip                 | dp\[2]\[0] = 0 |
|                  |                |          |            | Take                 | dp\[2]\[1] = 2 |
| 1                | 2              | 1        | 1          | Skip                 | dp\[2]\[0] = 1 |
|                  |                |          |            | Take                 | dp\[2]\[2] = 3 |
| 2                | 3              | 0        | 1          | Skip                 | dp\[3]\[0] = 1 |
|                  |                |          |            | Take                 | dp\[3]\[1] = 4 |
| 2                | 3              | 1        | 2          | Skip                 | dp\[3]\[0] = 2 |
|                  |                |          |            | Take                 | dp\[3]\[2] = 5 |
| 2                | 3              | 2        | 3          | Skip                 | dp\[3]\[0] = 3 |
|                  |                |          |            | ✖ Too Long → No Take |                |

You continue until `i = 5`.

---

## 🏁 Final Step

```java
long maxScore = 0;
for (int j = 0; j <= k; j++) {
    maxScore = Math.max(maxScore, dp[n][j]);
}
System.out.println(maxScore);
```

* Scan through all final states `dp[n][j]` where `j ∈ [0, k]`
* Return the **maximum score achieved**

---

## ✅ Final Output:

For this input:

```
6 2
1
2
3
1
6
10
```

* Optimal picks: \[2, 3], skip 1, \[6, 10] → 2 + 3 + 6 + 10 = **21**

```
Output: 21
```

---

## 📊 Summary Diagram

```plaintext
[1]   [2]   [3]   [1]   [6]   [10]
 ↑     ↑     ↑    ↑     ↑     ↑
Skip  Keep  Keep Skip  Keep  Keep
      (Streak=2)      (Streak=2)
```

* You break before streak exceeds `k=2`
* Maximize score with valid streaks

---
---

# 2. Help Mike Get Rich

## Problem Statement

Mike Ross is a brilliant lawyer who wins all his cases. However, he is not a morning person. His law firm has introduced a new rule: to be eligible for a bonus, every lawyer must review **at least one pending case document every day**. If a lawyer misses even a single day, their bonus for that year is forfeited.

To maintain fairness in case distribution, the following rules are set:  

1. Whenever a lawyer arrives at the office, they join a queue waiting for the next case distribution event.  
2. Each case distribution event on a day distributes a fixed number of cases.  
3. At distribution time, lawyers who arrived earliest get assigned cases first.  
4. If cases remain undistributed and no lawyers are waiting in queue, those cases go to senior partners and are considered lost opportunities.  
5. Case distribution times and the number of cases distributed per event can **change daily** (different test cases).  

**Important note:**  
- No two lawyers arrive at exactly the same time.

---

## Mike's Dilemma

Mike has a vision: an unordered list of timestamps representing the next day's **case distribution events** and the arrival times of his competing lawyers. Mike wants to figure out the **latest arrival time** at the office such that he still gets assigned a case on that day (thus preserving his bonus streak).

---

## Input Format

- **Line 1:** An integer `n` — number of case distribution events in the day.
- **Line 2:** `n` space-separated integers — timestamps of the distribution events (in arbitrary order).
- **Line 3:** An integer `m` — number of competing lawyers (excluding Mike).
- **Line 4:** `m` space-separated integers — arrival timestamps of competing lawyers (in arbitrary order).
- **Line 5:** An integer `c` — number of cases distributed per event.

---

## Constraints

- `1 <= n <= 10^5` (number of distribution events)
- `1 <= m <= 10^5` (number of competing lawyers)
- `1 <= c <= 10^4` (cases per distribution event)
- All timestamps are integers starting from 0 (day starts at time 0).
- No two lawyers share the same arrival timestamp.
- Distribution event times and arrival times may not be sorted.

---

## Output Format

- Print a single integer — the **latest timestamp** Mike can arrive at the office and still get assigned a case on the given day.

---

## Sample Input 1

```
3
20 30 10
7
19 13 26 4 25 11 21
2
```

## Sample Output 1

```
20
```

### Explanation

- Distribution events: at times 10, 20, and 30.
- Cases per event: 2
- Lawyers arrive at timestamps: 4, 11, 13, 19, 21, 25, 26.

**Event at 10:**  
- Cases assigned to lawyer who arrived at 4.  
- One case remains and is lost since no other lawyers have arrived yet.

**Event at 20:**  
- Available lawyers by 20 are those who arrived before or at 20: 11, 13, 19, and Mike (arriving at 20, latest possible).  
- The first two cases go to the two earliest arrivals: 11 and 13.  
- Mike also arrived exactly at 20, so he will be next in queue.

**Event at 30:**  
- Remaining lawyers including those arriving before 30: 19, 20 (Mike), 21, 25, 26.  
- Cases are assigned in order of arrival time.

If Mike arrived any later than 20, a lawyer arriving at 21 would receive the case first, breaking Mike’s streak.

---

## Sample Input 2

```
2
10 20
4
2 17 18 19
2
```

## Sample Output 2

```
16
```

### Explanation

- Distribution events at 10 and 20, 2 cases each.
- Lawyers arrive at: 2, 17, 18, 19.

**Event at 10:**  
- Lawyer at 2 gets a case.  
- One case left unused (no other lawyers have arrived yet).

**Event at 20:**  
- Lawyers arrived before 20: 17, 18, 19 and possibly Mike who wants to arrive latest.  
- If Mike arrives at 16, he will also be in time to receive a case.  
- If Mike arrives after 16, lawyers 17, 18, 19 go first and Mike misses out.

---

## Clarifications

- Lawyers can only get a case if they have arrived **at or before** the distribution timestamp.
- Mike must **arrive before or exactly at** some timestamp where he can be assigned a case, considering the queue order and batches.
- If Mike arrives at a timestamp where all cases are given out to lawyers who arrived earlier, Mike misses the streak.

---

## Goal

Find the maximum possible arrival timestamp for Mike to ensure he receives at least one case that day.

---

## Additional Notes

- Consider sorting the distribution timestamps and lawyer arrivals to simulate the process in chronological order.
- Efficient algorithms and data structures should be used given the input constraints.
- Pay special attention to how Mike’s arrival impacts the queue order and case allocation.

---

*End of problem statement.*

---

## Problem Recap

Mike Ross wants to maintain a daily streak of receiving at least one case to be eligible for a bonus. Cases are distributed in events throughout the day with fixed batch sizes, and lawyers are assigned cases in order of arrival time (earliest first). Mike wants to find the **latest timestamp he can arrive** at the office and still get a case assigned on that day.

---

## Key Observations

- Cases are assigned **FCFS (First Come, First Serve)** based on arrival times.
- Mike must arrive **at or before** some distribution event to be considered.
- At each distribution, up to `c` cases are assigned to waiting lawyers who have not yet received cases.
- We must find the latest arrival time where inserting Mike still results in Mike getting a case.

---

## Approach

1. **Sort** both distribution event timestamps and lawyer arrival timestamps.
2. Use **binary search** on possible arrival times for Mike, checking at each step if Mike can secure a case.
3. To check feasibility, **simulate distribution** with Mike inserted at the candidate arrival timestamp.
4. Return the maximum arrival time for which Mike still gets a case.

---

## Java Code

```java
import java.util.*;

public class Main {

    static int n, m, c;
    static int[] distTimes, lawyerArrivals;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        n = sc.nextInt();
        distTimes = new int[n];
        for (int i = 0; i < n; i++) distTimes[i] = sc.nextInt();

        m = sc.nextInt();
        lawyerArrivals = new int[m];
        for (int i = 0; i < m; i++) lawyerArrivals[i] = sc.nextInt();

        c = sc.nextInt();

        Arrays.sort(distTimes);
        Arrays.sort(lawyerArrivals);

        int lo = 0, hi = (int)1e9;
        int ans = -1;

        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (canMikeGetCase(mid)) {
                ans = mid;  // try later arrival
                lo = mid + 1;
            } else {
                hi = mid - 1;
            }
        }

        System.out.println(ans);
    }

    static boolean canMikeGetCase(int mikeArrival) {
        // Merge Mike’s arrival with existing lawyer arrivals
        List<Integer> allArrivals = new ArrayList<>();
        for (int a : lawyerArrivals) allArrivals.add(a);
        allArrivals.add(mikeArrival);
        Collections.sort(allArrivals);

        int totalLawyers = allArrivals.size();
        boolean[] hasCase = new boolean[totalLawyers];

        int ptr = 0; // advances through arrivals as we simulate
        for (int time : distTimes) {
            int start = ptr;
            while (ptr < totalLawyers && allArrivals.get(ptr) <= time) ptr++;
            int end = ptr;

            int assigned = 0;
            for (int i = start; i < end && assigned < c; i++) {
                if (!hasCase[i]) {
                    hasCase[i] = true;
                    assigned++;
                }
            }
        }

        for (int i = 0; i < totalLawyers; i++) {
            if (allArrivals.get(i) == mikeArrival) {
                return hasCase[i];
            }
        }
        return false;
    }
}
```

---

## Example

Input:
```
3
20 30 10
7
19 13 26 4 25 11 21
2
```

Output:
```
20
```

---

## Complexity

- Sorting: `O(n log n + m log m)`
- Each binary search iteration: `O(n + m)`
- Binary search with ~30 iterations → efficient for input size up to `10^5`.

---

## Summary

By simulating the daily distribution process and using binary search on Mike's arrival time, we can efficiently determine the **latest timestamp** Mike can arrive without losing his daily case streak.


---
---

# 3. Dora The Explorer

## Problem Statement

Dora is vacationing on an archipelago of **N** islands. She starts her vacation on island **S** and ends it on island **E**. Dora travels between islands using **two modes of transportation**: **Boat** and **Sea Plane**.  

Dora wants to:  
- Use **exactly one mode exclusively** for some part of her journey,  
- Then **switch modes exactly once**,  
- And complete her trip from **S** to **E**.

For example, Dora can travel from **S** to some island **I** using **only Boat** (possibly visiting intermediate islands), then from **I** to **E** using **only Plane** (also possibly through other islands). Or, she may travel using **Plane first** then switch to **Boat** at some island.

You are given two cost matrices:
- `B[i][j]`: cost of traveling from island `i` to `j` by Boat,
- `P[i][j]`: cost of traveling from island `i` to `j` by Plane.

If `B[i][j]` or `P[i][j]` is `-1`, it means travel directly between `i` and `j` by that mode is not possible.

Your task is to find the minimum possible cost of the entire trip. If no valid path exists, return `-1`.

---

## Input Format

- First line: Integer `N` — number of islands.
- Next `N` lines: Each contains `N` integers denoting Boat cost matrix `B`.
- Next `N` lines: Each contains `N` integers denoting Plane cost matrix `P`.
- Last line: Two integers `S` and `E` denoting start and end islands.

---

## Constraints

- `3 <= N <= 1250`
- `1 <= S, E <= N` and `S != E`
- `-1` or `1 <= B[i][j], P[i][j] <= 100`

---

## Output Format

- A single integer — minimum total cost of Dora's vacation; `-1` if not possible.

---

## Sample Input 1

```
3
-1 1 2
3 -1 4
5 6 -1
-1 6 5
1 -1 4
3 2 -1
1 2
```

## Sample Output 1

```
4
```

### Explanation

- Dora travels from Island 1 → Island 3 by **Boat** at cost 2.
- Then from Island 3 → Island 2 by **Plane** at cost 2.
- Total cost = 2 + 2 = 4.

---

## Sample Input 2

```
5
-1 3 38 96 1
40 -1 16 24 73
5 52 -1 93 34
78 10 73 -1 84
50 15 51 53 -1
-1 67 30 50 45
96 -1 5 2 12
60 19 -1 51 46
-1 56 79 -1 2
83 2 30 5 -1
1 4
```

## Sample Output 2

```
5
```

### Explanation

- Dora routes: Island 1 → Island 5 by Boat (cost 1)
- Island 5 → Island 2 → Island 4 by Plane (cost 2 + 2)
- Total cost = 1 + 2 + 2 = 5.

---

# Approach: Shortest Path with Mandatory Mode Switch

This problem is a variant of the shortest path problem with a **mandatory single mode switch**. We use **Dijkstra’s algorithm** on both transportation modes independently to compute shortest paths and combine results.

---

## Solution Outline

1. Compute shortest costs for:
   - Boat travel **from S** to every island → `boatFromS[]`
   - Plane travel **from S** to every island → `planeFromS[]`
   - Boat travel **to E** from every island → `boatToE[]`
   - Plane travel **to E** from every island → `planeToE[]`

2. For every island `i` (except `S` and `E`), consider switching at `i`:
   - Case 1: Boat from `S` to `i` + Plane from `i` to `E`
   - Case 2: Plane from `S` to `i` + Boat from `i` to `E`

3. Find the minimum total cost over all `i`.

---

## Java Code Implementation

```java
import java.util.*;

public class Main {

    static final int INF = Integer.MAX_VALUE;
    static int N;
    static int[][] B, P;

    // Dijkstra to find shortest paths from src on given graph
    static int[] dijkstra(int[][] graph, int src) {
        int[] dist = new int[N];
        Arrays.fill(dist, INF);
        dist[src] = 0;

        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));
        pq.offer(new int[]{src, 0});

        while (!pq.isEmpty()) {
            int[] cur = pq.poll();
            int u = cur[0], d = cur[1];
            if (d > dist[u]) continue;

            for (int v = 0; v < N; v++) {
                if (graph[u][v] == -1) continue;
                int nd = d + graph[u][v];
                if (nd < dist[v]) {
                    dist[v] = nd;
                    pq.offer(new int[]{v, nd});
                }
            }
        }
        return dist;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        N = sc.nextInt();
        B = new int[N][N];
        P = new int[N][N];

        for (int i = 0; i < N; i++)
            for (int j = 0; j < N; j++)
                B[i][j] = sc.nextInt();

        for (int i = 0; i < N; i++)
            for (int j = 0; j < N; j++)
                P[i][j] = sc.nextInt();

        int S = sc.nextInt() - 1;
        int E = sc.nextInt() - 1;

        int[] boatFromS = dijkstra(B, S);
        int[] planeFromS = dijkstra(P, S);
        int[] boatToE = dijkstra(B, E);
        int[] planeToE = dijkstra(P, E);

        int minCost = INF;
        for (int i = 0; i < N; i++) {
            if (i == S || i == E) continue;

            // Switch: Boat -> Plane
            if (boatFromS[i] != INF && planeToE[i] != INF)
                minCost = Math.min(minCost, boatFromS[i] + planeToE[i]);

            // Switch: Plane -> Boat
            if (planeFromS[i] != INF && boatToE[i] != INF)
                minCost = Math.min(minCost, planeFromS[i] + boatToE[i]);
        }

        System.out.println(minCost == INF ? -1 : minCost);
    }
}
```

---

## Step-by-Step Example (Sample Input 1)

- Islands = 3, S=1, E=2 (0-based: S=0, E=1)  
- After running Dijkstra:

| Island `i` | boatFromS | planeFromS | boatToE | planeToE |
|------------|------------|------------|---------|----------|
| 0          | 0          | 0          | 3       | 1        |
| 1          | 1          | 6          | 0       | 0        |
| 2          | 2          | 5          | 4       | 2        |

- Check switching at island 2:  
  - Boat S->2 + Plane 2->E = 2 + 2 = 4  
  - Plane S->2 + Boat 2->E = 5 + 4 = 9  
- Minimum = 4

---

## Execution Time Limit

- 10 seconds (efficient Dijkstra implementation needed).

---
---

# 4. Fun Friday

## Problem Statement

As a fun Friday activity, Harsh and Pranay decided to play a new game. The game begins with Harsh giving a list of *n* numbers to Pranay.

In each iteration:  
- Pranay adds up all the numbers in the list and adds that sum to his points. Then, he returns the list of numbers to Harsh.

In each iteration, Harsh performs the following:  
1. If Harsh gets a list with only one number, he throws it out of the game.  
2. If Harsh gets a list consisting of more than one number, he divides it into two non-empty subsequences and gives both subsequences to Pranay one by one.

After all iterations, Pranay's total points are the sum of all added sums throughout the game. Your task is to determine the **maximum possible points** Pranay can get.

---

## Input Format

- The first line contains a single integer, *n*, the number of elements in the list.  
- The second line contains *n* integers, `a1, a2, ..., an` where `1 <= ai <= 10^6`.

---

## Constraints

- `1 <= n <= 3 * 10^5`  
- `1 <= ai <= 10^6`

---

## Output Format

- Print a single integer representing the maximum possible points Pranay can achieve.

---

## Sample Input 0

```
3
4 2 6
```

## Sample Output 0

```
34
```

## Explanation

**Initial list:** [4, 2, 6]  
- Pranay adds sum 4 + 2 + 6 = 12.  

**Harsh's turn:**  
- Splits into (2) and (4, 6), gives these lists to Pranay one by one.

**Pranay's subsequent turns:**  
- Adds sum(2) = 2 points, total = 14  
- Adds sum(4, 6) = 10 points, total = 24  

**Harsh's turn again for (4, 6):**  
- Splits into (4) and (6)

**Pranay adds:**  
- 4 + 6 = 10 points, total = 34

---

# Approaches to Solve "Fun Friday"

Let's explore **four approaches** to find the maximum points Pranay can get, each with detailed explanations and Java implementations.

---

## Problem Recap

Given an array, Pranay scores points by:  

1. Adding the sum of the current list.  
2. Harsh splits the current list into two non-empty sublists recursively.  
3. Pranay repeats the scoring on each sublist.  
4. Single-element lists are discarded.

Goal: **Maximize Pranay's total score.**

---

## Example

**Input:**

```
3
4 2 6
```

**Output:**  
```
34
```

**Breakdown:**

- [4,2,6] → 12 points
- Split into (2) and (4,6) → points + 2 + 10 = 12
- Split (4,6) into (4) and (6) → points + 4 + 6 = 10  
Total = 12 + 2 + 10 + 4 + 6 = 34

---

## Approach 1: Brute Force Recursion

### Logic

- Try all possible split points in the current segment.  
- Recursively compute the points for left and right subsequences.  
- Sum the current segment plus the maximum points from the splits.

### Time Complexity

- Exponential: `O(2^n)`, not feasible for large n (only for conceptual understanding).

### Code

```java
public class Main {
    static long solve(int[] arr, int l, int r) {
        if (l >= r) return 0;

        long sum = 0;
        for (int i = l; i <= r; i++) sum += arr[i];

        long max = 0;
        for (int i = l; i < r; i++) {
            long left = solve(arr, l, i);
            long right = solve(arr, i + 1, r);
            max = Math.max(max, left + right);
        }

        return sum + max;
    }

    public static void main(String[] args) {
        int[] arr = {4, 2, 6};
        System.out.println(solve(arr, 0, arr.length - 1));
    }
}
```

### Explanation

- Checks all splits recursively to find the one giving maximum score.
- Returns 0 when only one element remains (discarded).

---

## Approach 2: Top-Down DP (Memoization)

### Logic

- Use a 2D DP array to store results of subproblems `(l, r)` and avoid recomputation.  
- Same recursive logic as brute force, but with memoization.

### Time and Space Complexity

- Time: `O(n^2)`  
- Space: `O(n^2)`

### Code

```java
import java.util.*;

public class Main {
    static long[][] dp;
    static long[] prefix;

    static long sum(int l, int r) {
        return prefix[r + 1] - prefix[l];
    }

    static long dfs(int l, int r) {
        if (l >= r) return 0;
        if (dp[l][r] != -1) return dp[l][r];

        long max = 0;
        for (int i = l; i < r; i++) {
            long left = dfs(l, i);
            long right = dfs(i + 1, r);
            max = Math.max(max, left + right);
        }

        return dp[l][r] = sum(l, r) + max;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        prefix = new long[n + 1];
        dp = new long[n][n];

        for (int i = 0; i < n; i++) {
            arr[i] = sc.nextInt();
            prefix[i + 1] = prefix[i] + arr[i];
        }

        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);
        System.out.println(dfs(0, n - 1));
    }
}
```

### Explanation

- Precompute prefix sums for O(1) range sum queries.  
- Memoize results in `dp` to avoid exponential calls.

---

## Approach 3: Greedy Divide & Conquer (Balanced Splits)

### Logic

- Harsh splits the list into two subsequences with sums as balanced as possible.  
- Recursively apply to both sublists.

### Time Complexity

- Average: `O(n log n)`

### Code

```java
import java.util.*;

public class Main {
    static long totalPoints = 0;

    static void divide(int[] arr, long[] prefix, int l, int r) {
        if (l >= r) return;

        long sum = prefix[r + 1] - prefix[l];
        totalPoints += sum;

        int bestSplit = l;
        long minDiff = Long.MAX_VALUE;

        for (int i = l; i < r; i++) {
            long leftSum = prefix[i + 1] - prefix[l];
            long rightSum = prefix[r + 1] - prefix[i + 1];
            long diff = Math.abs(leftSum - rightSum);

            if (diff < minDiff) {
                minDiff = diff;
                bestSplit = i;
            }
        }

        divide(arr, prefix, l, bestSplit);
        divide(arr, prefix, bestSplit + 1, r);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        long[] prefix = new long[n + 1];

        for (int i = 0; i < n; i++) {
            arr[i] = sc.nextInt();
            prefix[i + 1] = prefix[i] + arr[i];
        }

        divide(arr, prefix, 0, n - 1);
        System.out.println(totalPoints);
    }
}
```

### Explanation

- Find the split index that minimizes the absolute difference of left and right sums for best balance.
- Balance encourages larger sums to be added earlier, yielding higher points.

---

## Approach 4: Priority Queue (Greedy Simulation by Max Segments)

### Logic

- Use a max-heap to always process the segment with the largest sum first.  
- Split into balanced parts and add them back into the heap, simulating the game optimally.

### Time Complexity

- About `O(n log n)` due to heap operations.

### Code

```java
import java.util.*;

public class Main {
    static class Segment {
        int l, r;
        long sum;

        Segment(int l, int r, long sum) {
            this.l = l;
            this.r = r;
            this.sum = sum;
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        long[] prefix = new long[n + 1];

        for (int i = 0; i < n; i++) {
            arr[i] = sc.nextInt();
            prefix[i + 1] = prefix[i] + arr[i];
        }

        PriorityQueue<Segment> pq = new PriorityQueue<>((a, b) -> Long.compare(b.sum, a.sum));
        pq.add(new Segment(0, n - 1, prefix[n]));
        long total = 0;

        while (!pq.isEmpty()) {
            Segment seg = pq.poll();
            if (seg.l == seg.r) continue;

            total += seg.sum;

            int bestSplit = seg.l;
            long minDiff = Long.MAX_VALUE;

            for (int i = seg.l; i < seg.r; i++) {
                long left = prefix[i + 1] - prefix[seg.l];
                long right = prefix[seg.r + 1] - prefix[i + 1];
                long diff = Math.abs(left - right);
                if (diff < minDiff) {
                    minDiff = diff;
                    bestSplit = i;
                }
            }

            long leftSum = prefix[bestSplit + 1] - prefix[seg.l];
            long rightSum = prefix[seg.r + 1] - prefix[bestSplit + 1];

            pq.add(new Segment(seg.l, bestSplit, leftSum));
            pq.add(new Segment(bestSplit + 1, seg.r, rightSum));
        }

        System.out.println(total);
    }
}
```

### Explanation

- Always pick and split the segment that contributes most to maximize overall score.  
- This mimics the optimal gameplay decision dynamically.

---

## Summary

| Approach           | Logic              | Time Complexity | Suitable For `n`       | Notes                        |
| ------------------ | ------------------ | --------------- | --------------------- | ----------------------------|
| 1. Brute Force     | Exhaustive splits  | `O(2^n)`        | Very small (≤20)      | Conceptual demonstration     |
| 2. DP + Memoization| Cache subproblems  | `O(n^2)`        | Small to medium (≤5000)| Exact, but heavy on memory   |
| 3. Greedy D&C      | Balanced splits    | `O(n log n)`    | Large (up to 3e5)     | Fast and practical           |
| 4. Priority Queue  | Max segment first  | `O(n log n)`    | Large (up to 3e5)     | Realistic simulation          |

---
