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


# 1. Cloud Network Bandwidth Pricing

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
```java
Name         Type          Description
-------------------------------------------------------
q            INTEGER       Number of events
a            INTEGER[][]   2D array containing event details in chronological order
```

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

```Java
//Cloud Network Bandwidth Pricing - Juspay (12 July 2025) Round 1: Coding

public static int solve(int q, List<List<Integer>> a) {
    Map<Long, Long> edgeFees = new HashMap<>();
    long totalCost = 0;

    for (int i = 0; i < q; i++) {
        List<Integer> event = a.get(i);
        int type = event.get(0);
        int u = event.get(1);
        int v = event.get(2);
        int x = event.get(3);

        if (type == 1) {
            // Fee update along path u -> v
            while (u != v) {
                if (u > v) {
                    long key = getKey(u);
                    edgeFees.put(key, edgeFees.getOrDefault(key, 0L) + x);
                    u /= 2;
                } else {
                    long key = getKey(v);
                    edgeFees.put(key, edgeFees.getOrDefault(key, 0L) + x);
                    v /= 2;
                }
            }
        } else if (type == 2) {
            // Data transfer cost along path u -> v
            long cost = 0;
            while (u != v) {
                if (u > v) {
                    long key = getKey(u);
                    cost += edgeFees.getOrDefault(key, 0L);
                    u /= 2;
                } else {
                    long key = getKey(v);
                    cost += edgeFees.getOrDefault(key, 0L);
                    v /= 2;
                }
            }
            totalCost += cost;
        }
    }

    return (int)totalCost;
}

// Helper to uniquely identify an edge between a node and its parent
private static long getKey(int node) {
    int parent = node / 2;
    int min = Math.min(node, parent);
    int max = Math.max(node, parent);
    return (((long)min) << 32) | (long)max;
}
```
