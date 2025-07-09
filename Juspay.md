## ✅ Problem: Data Server Storage Consolidation

### 🔍 Problem Description

A data center has multiple **servers**, each with a certain **storage capacity**. You are also given multiple **applications**, each requiring a certain amount of **storage space** to deploy.

Your task is to deploy all applications across the servers in such a way that:

* Every application is allocated to exactly one server.
* A server can hold multiple applications as long as its total storage isn't exceeded.
* **The number of servers used must be minimized.**

You need to **print the allocation of applications to servers**, and the **minimum number of servers used**.

---

## 📥 Input Format

```
n             // Number of servers
s1 s2 ... sn  // Server capacities

m             // Number of applications
a1 a2 ... am  // Application storage requirements
```

---

## 📤 Output Format

```
Total Servers Used: X
Server i: Applications [app1, app2, ...]
...
```

---

## 📌 Constraints

```
1 <= n <= 1000
1 <= m <= 1000
1 <= si <= 10^9     (server capacity)
1 <= ai <= 10^9     (application size)
Sum of all ai <= Sum of all si
```

---

## 🧪 Sample Test Case 1

### Input

```
3
100 200 150
5
70 80 50 30 60
```

### Output

```
Total Servers Used: 2
Server 2: Applications [80, 60]
Server 3: Applications [70, 50, 30]
```

---

## 🧪 Sample Test Case 2

### Input

```
2
100 100
3
80 90 50
```

### Output

```
Total Servers Used: 2
Server 2: Applications [90]
Server 1: Applications [80, 50]
```

---

## 💡 Approach

This is a **Greedy Strategy + Sorting** problem, inspired by the **First Fit Decreasing (FFD)** technique from Bin Packing.

### Steps:

1. **Sort applications** in descending order (largest first).
2. **Sort servers** by capacity (also descending), and track their remaining capacity and ID.
3. For each application:

   * Try to place it into the **first server** (from used list) that has enough space.
   * If it doesn't fit into any used server, take a new unused server.
4. Track which applications go into which server.
5. Output the server list and number of servers used.
