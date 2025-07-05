# Fun Friday

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
