# Juspay Questions - 12 July 2025

> 1. Smart City Traffic Toll System  
> 2. Corporate Restructuring ✅  
> 3. Security Key Generation -  
> 4. Vineyard Partition  
> 5. Customer Satisfaction Analysis  
> 6. Internet Service Provider Cable Management  
> 7. Factory Maintainance Scheduling ✅  
> 8. Water Pipeline Cost Management  
> 9. Parcel Delivery Optimisation  
> 10. Data Network Cost Management  
> 11. Network Technician’s Mission  
> 12. Cloud Networking Bandwidth Pricing ✅  
> 13. Encrypted File Name Update -  
> 14. Data Server Storage Consolidation  
> 15. Weather Monitoring System  
> 16. Building Distance Analysis  
> 17. Urban Wi-Fi Coverage Planning  
> 18. Magical Sequence Transformation  
> 19. Library Shelf Organisation  
> 20. Solar Panel Installation on Rooftops  
> 21. Power Grid Management  
> 22. Student Seat Arrangement  
> 23. The Medieval Market Problem  
> 24. Parking Lot Arrangement  


# 12. Cloud Network Bandwidth Pricing

### Background:

In a **cloud network**, an infinite number of *data centers* are set up globally. Each data center is assigned a unique positive integer starting from 1. The cloud network has direct, bidirectional data channels between:

> 1. Data center `i` and data center `2i`.
> 2. Data center `i` and data center `2i+1`.

There is always a unique shortest path between any two data centers. 
Initially, data transfer between any two data centers is free. However, due to network optimization, the **cloud provider** occasionally imposes bandwidth fees on certain data channels. The network provider will make several updates in the fees based on upcoming events.

There are two types of events:

1. **Fee Update**: The cloud provider updates the bandwidth fee for all data channels along the shortest path between data centers `a` and `b` by adding a fee of `x` units.
2. **Data Transfer**: A customer needs to transfer data from one data center `a` to another data center `b`. The transfer always follows the shortest path. You need to calculate the total cost incurred for this data transfer.

---

### Input:

* The first line contains an integer `q (1≤q≤1,000)`, representing the number of events.

* The next `q` lines contain details of the events in chronological order:

> * 1 a b x   → Fee update where all channels on the shortest path from a to b increase their fee by x units.
> * 2 a b 0   → Data transfer request from data center a to data center b.
> <br>They are present in the form of a 2D array of `q` rows and `4` columns.

---

### Output:

* For each **data transfer event** (`2 a b 0`), add to the sum the total cost for the transfer and return the final sum.

---

### Function Description:
> Complete the **solve** function in the editor below. It has the following parameter(s):

| Name | Type | Description |
|------|------|-------------|
| q    | INTEGER | Number of events |
| a    | INTEGER[][] | 2D array containing event details in chronological order |

#### Returns:

* `INTEGER`: The function must return an integer denoting the sum of total cost for each transfer event.

---

### Constraints:

* `1 ≤ q ≤ 10^3`
* `1 ≤ a[i][j] ≤ 10^9`

---

### Input Format (for debugging):

```
The first line contains an integer, q, denoting the number of rows in a.
The next line contains an integer, 4, denoting the number of columns in a.
Each line i of the q subsequent lines (where 0 ≤ i < q) contains 4 space-separated integers each describing the row of a[i].
```

---

### Sample Testcases

| Input                                                                                                         | Output | Output Description                                                                                                                                                                                                                                                                                                         |
|--------------------------------------------------------------------------------------------------------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 7  <br> 1 3 4 <br> 20 <br> 1 4 1 2 <br> 1 3 6 8 <br> 2 4 3 0 <br> 1 6 1 <br> 40 <br> 2 3 7 0 <br> 2 2 4 0                  | 86     | 1. 2 4 3 0 : Answer is 64 as path: 4→2→1→3 <br>And, 4→2 has cost 22, 2→1 has 22, 1→3 has 20. So, 22+22+20=64. <br>2. 2 3 7 0 : Answer is 0 as no cost is present. <br>3. 2 2 4 0 : Answer is 22 as path 2→4 has cost 20 from 1st query and 2 from 2nd. So, 22. <br>So, 64+0+22 = **86**.         |
| 7  <br> 1 3 4 <br> 10 <br> 1 4 1 2 <br> 1 3 6 8 <br> 2 4 3 0 <br> 1 6 1 <br> 40 <br> 2 3 7 0 <br> 2 2 4 0                  | 46     | 1. 2 4 3 0 : Answer is 34 as path: 4→2→1→3 <br>And, 4→2 has cost 22, 2→1 has 22, 1→3 has 10. So, 12+12+10=34. <br>2. 2 3 7 0 : Answer is 0 as no cost is present. <br>3. 2 2 4 0 : Answer is 12 as path 2→4 has cost 10 from 1st query and 2 from 2nd. So, 12. <br>So, 34+0+12 = **46**.      |
| 3  <br> 1 1 2 2 <br> 1 1 4 2 <br> 2 1 4 0                                                                     | 6      | 2 1 4 0 : <br>Path from 1 to 4 is: 1→2→4. So, 4+2 = **6**.                                                                                                                                                                                                                                                                  |

---
## Code
```Java
//Cloud Network Bandwidth Pricing - Juspay (12 July 2025) Round 1: Coding
import java.io.*;
import java.util.*;
import java.lang.Math;
import java.util.stream.Collectors.joining;
import java.util.stream.Collectors.toList;

public class Solution {

    public static int solve(int q, List < List < Integer >> a) {
        Map < Long, Long > edgeFees = new HashMap < > ();
        long totalCost = 0;

        for(int i = 0; i < q; i++) {
            List < Integer > event = a.get(i);
            int type = event.get(0);
            int u = event.get(1);
            int v = event.get(2);
            int x = event.get(3);

            if(type == 1) {
                // Fee update along path u -> v
                while(u != v) {
                    if(u > v) {
                        long key = getKey(u);
                        edgeFees.put(key, edgeFees.getOrDefault(key, 0 L) + x);
                        u /= 2;
                    } else {
                        long key = getKey(v);
                        edgeFees.put(key, edgeFees.getOrDefault(key, 0 L) + x);
                        v /= 2;
                    }
                }
            } else if(type == 2) {
                // Data transfer cost along path u -> v
                long cost = 0;
                while(u != v) {
                    if(u > v) {
                        long key = getKey(u);
                        cost += edgeFees.getOrDefault(key, 0 L);
                        u /= 2;
                    } else {
                        long key = getKey(v);
                        cost += edgeFees.getOrDefault(key, 0 L);
                        v /= 2;
                    }
                }
                totalCost += cost;
            }
        }

        return (int) totalCost;
    }

    // Helper to uniquely identify an edge between a node and its parent
    private static long getKey(int node) {
        int parent = node / 2;
        int min = Math.min(node, parent);
        int max = Math.max(node, parent);
        return (((long) min) << 32) | (long) max;
    }


    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        int q = Integer.parseInt(scan.nextLine()
            .trim());

        List < List < Integer >> a = new ArrayList < > ();
        for(int i = 0; i < q; i++) {
            a.add(
                Arrays.asList(scan.nextLine()
                    .trim()
                    .split(" "))
                .stream()
                .map(s - > Integer.parseInt(s))
                .collect(Collectors.toList())
            );
        }

        int result = solve(q, a);
        System.out.println(result);
    }
}
```
---
# 7. Factory Maintenance Scheduling

You are the manager of a large factory with various machines, each of which requires periodic maintenance to keep running efficiently. There is a production target, measured in units of output that you need to achieve, and each machine contributes to the total output.

You have `n` machines in the factory. Each machine can produce `o_i` units of output before it requires maintenance, which takes `c_i` days to complete, meaning the next time you can use this machine is day `x + c_i` if your current day is `x`. During maintenance, the machine cannot contribute to production. On each day, you can use all machines that are not currently under maintenance. If all machines are under maintenance on a given day, no output is produced for that day.

Initially, all machines are available for production. The goal is to determine how many days it will take to meet or exceed the required production target, using all available machines optimally.

---

## Function Description

Complete the `solve` function in the editor below. It has the following parameter(s):

| Name | Type | Description |
|------|------|-------------|
| P    | INTEGER | Total production target |
| n    | INTEGER | Number of machines |
| o    | INTEGER ARRAY | Output units each machine can produce before requiring maintenance |
| c    | INTEGER ARRAY | Number of days each machine takes for maintenance after producing its output |

---

## Constraints

- 1 ≤ P ≤ 2×10⁵  
- 1 ≤ n ≤ 2×10⁵  
- 1 ≤ o[i] ≤ 2×10⁵  
- 1 ≤ c[i] ≤ 2×10⁵  

---

## Input Format for Debugging

- The first line contains an integer, `P`, denoting the total production target.
- The next line contains an integer, `n`, denoting the number of machines.
- Each of the next `n` lines contains an integer describing `o[i]`.
- Each of the next `n` lines contains an integer describing `c[i]`.

---

## Sample Testcases

| Input                                                                                                                                                     | Output | Output Description                                                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 24 <br> 6 <br> 3 <br> 3 <br> 2 <br> 1 <br> 2 <br> 3 <br> 9 <br> 10 <br> 2 <br> 8 <br> 6 <br> 9                                                                | 9      | Use all machines on day 1. Total target = 14. <br>For 3rd, 5th, 7th and 9th day, use machine 3. So, 14+8=22. <br>On day 7, machine 5 can be used. It gives 22+2=24. <br>There are many other combinations possible, but the minimum days needed are **9**.                                               |
| 3 <br> 1 <br> 1 <br> 1                                                                                                                                    | 3      | We can use the machine on each day. So total = **3** days.                                                                                                                                                                                                                                               |
| 90000 <br> 2 <br> 200000 <br> 200000 <br> 1 <br> 1                                                                                                         | 1      | On **day 1** itself, the target gets exceeded.                                                                                                                                                                                                                                                           |

---

## Code

```Java

// Factory Maintenance Scheduling - Juspay (12 July 2025) Round 1: Coding

import java.util.*;

public class Solution {
    public static int solve(int p, int n, List<Integer> o, List<Integer> c) {
        int days = 0;
        int total_output = 0;

        // Stores next available day for each machine
        int[] available = new int[n];
        Arrays.fill(available, 1); // machines are available on day 1

        while (total_output < p) {
            days++;

            // Find the best machine available today
            int bestMachine = -1;
            int maxOutput = -1;

            for (int i = 0; i < n; i++) {
                if (available[i] <= days && o.get(i) > maxOutput) {
                    maxOutput = o.get(i);
                    bestMachine = i;
                }
            }

            // If no machine is available, skip to next day
            if (bestMachine == -1) continue;

            // Use the best machine
            total_output += o.get(bestMachine);
            available[bestMachine] = days + c.get(bestMachine); // update its next availability
        }

        return days;
    }

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        int p = Integer.parseInt(scan.nextLine().trim());
        int n = Integer.parseInt(scan.nextLine().trim());

        List<Integer> o = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            o.add(Integer.parseInt(scan.nextLine().trim()));
        }

        List<Integer> c = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            c.add(Integer.parseInt(scan.nextLine().trim()));
        }

        System.out.println(solve(p, n, o, c));
    }
}

```
---
Certainly! Below is the fully formatted version in **Markdown**, preserving the original **table format for sample testcases and outputs**, and keeping everything clean and structured:

---

# 1. Smart City Traffic Toll System

### Background:

In a **smart city**, there are an infinite number of **traffic control hubs** numbered with positive integers starting from 1. Each hub is connected by smart roads in a unique tree-like structure:

> 1. There is a direct, bidirectional road between hub i and 2i.
> 2. Another direct road exists between hub `i` and `2*i + 1`.

Given this structure, there is always a unique shortest path between any two traffic control hubs.

Initially, passing through any road is toll-free. However, to optimize traffic flow and manage congestion, the city traffic authority occasionally imposes toll fees on certain roads along specific paths.

The traffic authority will introduce a series of changes:

---
**1. Toll Fee Update:** An update described by integers `x`, `y`, and `t` imposes a toll of `t` units on all roads along the shortest path from hub `y` to hub `x`.

**2. Travel Cost Calculation:** A commuter travels from hub `x` to hub `y` using the shortest path, and you need to calculate the total toll fees they incur.

---

### **Input:**

* The first line contains an integer `q (1 ≤ q ≤ 1,000)`, representing the number of events.
* The next `q` lines contain details of the events:

  * `1 x y t`: describes a toll update where all roads on the shortest path between hubs x and y increase their toll by t units.
  * `2 x y 0`: represents a travel event where a commuter moves from hub x to hub y.
    They are given in the form of a 2D array of `q` rows and `4` columns.

---

### **Output:**

For each travel event `(2 x y 0)`, add to a sum the total cost for the trip and return the sum.

---

### **Function Description:**

Complete the `solve` function in the editor below. It has the following parameter(s):

| Name | Type             | Description           |
| ---- | ---------------- | --------------------- |
| q    | INTEGER          | Number of events      |
| a    | INTEGER 2D ARRAY | Details of the events |

---

### **Constraints:**

* $1 \leq q \leq 10^5$
* $1 \leq a[i][j] \leq 10^5$

---

### **Input format for debugging:**

* The first line contains an integer `q`, denoting the number of rows in `a`.
* The next line contains an integer `4`, denoting the number of columns in `a`.
* Each line `i` of the `q` subsequent lines (where `0 ≤ i < q`) contains 4 space-separated integers each describing the row `a[i]`.

---

### **Sample Testcases:**

| Input                                                                                | Output | Output Description                                                                                                                                                                                                        |
| ------------------------------------------------------------------------------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `3`<br>`1 1 2 2`<br>`1 1 4 2`<br>`2 1 4 0`                                           | `6`    | Query: 2 1 4 0<br>Path from 1 to 4 is: 1 → 2 → 4<br>So, fees is 4 + 2 = 6                                                                                                                                                 |
| `5`<br>`1 1 4 2`<br>`1 2 7 2`<br>`2 4 7 0`<br>`2 5 7 0`<br>`2 6 7 0`                 | `10`   | 1. 2 4 7 0:<br>Path: 4 → 2 → 1 → 3 → 7<br>Cost: 4→2 = 2, 2→1 = 4, 1→3 = 2, 3→7 = 2<br>Total = 2 + 4 + 2 + 2 = 10                                                                                                          |
| `7`<br>`1 3 4 20`<br>`1 4 1 22`<br>`1 3 6 20`<br>`2 4 3 0`<br>`2 3 7 0`<br>`2 2 4 0` | `86`   | 1. 2 4 3 0:<br>Path: 4→2→1→3<br>Cost: 4→2 = 22, 2→1 = 22, 1→3 = 20 → Total = 64<br>2. 2 3 7 0: Path has no cost → 0<br>3. 2 2 4 0: Cost: 2→4 = 20 (from 1st), +2 (from 2nd) → Total = 22<br>Final Total: 64 + 0 + 22 = 86 |

---

```
import java.util.*;

public class SmartCityTrafficToll {
    static Map<String, Long> tollMap = new HashMap<>();

    // Helper to generate a consistent key for edge (parent-child)
    static String edgeKey(int u, int v) {
        int min = Math.min(u, v);
        int max = Math.max(u, v);
        return min + "-" + max;
    }

    // Toll fee update: Add t to every edge in the path from x to y
    static void updateToll(int x, int y, int t) {
        while (x != y) {
            if (x > y) {
                String key = edgeKey(x, x / 2);
                tollMap.put(key, tollMap.getOrDefault(key, 0L) + t);
                x /= 2;
            } else {
                String key = edgeKey(y, y / 2);
                tollMap.put(key, tollMap.getOrDefault(key, 0L) + t);
                y /= 2;
            }
        }
    }

    // Travel cost calculation: Sum tolls in the path from x to y
    static long queryCost(int x, int y) {
        long cost = 0;
        while (x != y) {
            if (x > y) {
                String key = edgeKey(x, x / 2);
                cost += tollMap.getOrDefault(key, 0L);
                x /= 2;
            } else {
                String key = edgeKey(y, y / 2);
                cost += tollMap.getOrDefault(key, 0L);
                y /= 2;
            }
        }
        return cost;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int q = sc.nextInt(); // number of events

        for (int i = 0; i < q; i++) {
            int type = sc.nextInt();
            int x = sc.nextInt();
            int y = sc.nextInt();

            if (type == 1) {
                int t = sc.nextInt();
                updateToll(x, y, t);
            } else if (type == 2) {
                System.out.println(queryCost(x, y));
            }
        }
        sc.close();
    }
}
```
