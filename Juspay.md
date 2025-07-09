## ✅ Problem: 4. Data Server Storage Consolidation

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

---
---
---

## ✅ Problem: 3. Road Network Breakdown

### 🔍 **Problem Description**

The city’s road network is represented as an undirected graph with `n` intersections (nodes) and `m` roads (edges). Certain roads are **critical** — if any one of them is removed, the city’s network will become disconnected (i.e., at least one pair of intersections will no longer be reachable from one another).

Your task is to identify and list all such **critical roads (bridges)** in the network.

---

### 📥 Input Format

```
n m
u1 v1
u2 v2
...
um vm
```

* First line: two integers `n` (number of nodes), `m` (number of edges).
* Next `m` lines: each contains two integers `ui` and `vi` representing a bidirectional road between intersection `ui` and `vi`.

> **Note:** Nodes are numbered from 0 to n-1.

---

### 📤 Output Format

Print all critical roads, one per line, in the form:

```
u v
```

Where `(u, v)` is a bridge. The order of edges or nodes in each pair does not matter. If there are no bridges, print nothing.

---

### 📌 Constraints

* `1 ≤ n ≤ 10^5`
* `0 ≤ m ≤ 10^5`
* `0 ≤ ui, vi < n`
* There are no parallel edges or self-loops.

---

### 🧪 Sample Test Case 1

**Input**

```
5 5
0 1
1 2
2 0
1 3
3 4
```

**Output**

```
3 4
1 3
```

> Removing either (3-4) or (1-3) disconnects the network.

---

### 🧪 Sample Test Case 2

**Input**

```
4 3
0 1
1 2
2 3
```

**Output**

```
2 3
1 2
0 1
```

> This is a linear chain; every edge is a bridge.

---

## 🧠 Approach

We use **Tarjan’s Algorithm** to find **bridges in a graph** using DFS. The core idea is to track:

* `disc[u]`: Discovery time of a node `u` in DFS
* `low[u]`: The lowest discovery time reachable from the subtree rooted at `u`

For each node `u`, if you explore a child `v` such that `low[v] > disc[u]`, then the edge `(u, v)` is a **bridge**.

---
---
---



## 🚦 **Problem Title: 2. Smart City Traffic Toll System**

---

## 📘 **Problem Description:**

In a smart city, roads between locations are equipped with electronic tolls and traffic sensors. Each road segment between two locations has a **toll cost** and a **travel time**. A user can choose whether they want to minimize the total **toll cost** or the total **travel time** between two points.

Given the road network as a graph, and a user’s preference (`"TOLL"` or `"TIME"`), determine the minimum cost (either toll or time) required to travel from a **source** location to a **destination** location.

---

## 🔡 **Input Format:**

```
n m
u1 v1 toll1 time1
u2 v2 toll2 time2
...
um vm tollm timem
preference
source destination
```

* `n` = number of locations (nodes)
* `m` = number of roads (edges)
* Each of the next `m` lines contains:

  * `ui`, `vi`: two endpoints of the road
  * `tolli`: toll cost of the road
  * `timei`: travel time of the road
* `preference`: either `"TOLL"` or `"TIME"`
* `source`: starting location
* `destination`: target location

---

## 🖨️ **Output Format:**

```
Minimum <preference>: <value>
```

Example:

```
Minimum toll: 30
```

or

```
Minimum time: 18
```

---

## 📋 **Constraints:**

```
1 <= n <= 10^4
1 <= m <= 5*10^4
0 <= tolli, timei <= 10^4
0 <= ui, vi < n
preference ∈ {"TOLL", "TIME"}
```

---

## 🧪 **Sample Test Case 1:**

### Input:

```
5 5
0 1 10 5
1 2 5 7
0 2 15 20
2 3 10 4
3 4 5 2
TOLL
0 4
```

### Output:

```
Minimum toll: 30
```

---

## 🧪 **Sample Test Case 2:**

### Input:

```
5 5
0 1 10 5
1 2 5 7
0 2 15 20
2 3 10 4
3 4 5 2
TIME
0 4
```

### Output:

```
Minimum time: 18
```

---

## 💡 **Approach:**

* Model the city as a **graph** with nodes as locations and edges with dual weights (toll, time).
* Use **Dijkstra's Algorithm** to find the shortest path.
* Use the **user’s preference** to choose the weight for comparison:

  * `"TOLL"` → minimize toll cost
  * `"TIME"` → minimize time
* Track distances and use a **priority queue** to always expand the node with the lowest cumulative cost.

---
---
---




## 📝 **Problem: 1. Corporate Reconstruction**

### 💼 **Problem Description**

A company has multiple subsidiaries organized in a hierarchical structure. Over time, subsidiaries merge with each other, forming new entities. Each merger results in two subsidiaries becoming part of the same parent group.

You're given a list of such mergers as pairs `(A, B)`, which means **subsidiary B is merged into A**. After all the mergers, your task is to determine the **top-level subsidiaries**, i.e., subsidiaries that do not report to any other and act as the root of their merged group.

---

### 🔢 **Input Format**

* The first line contains an integer `n`, the number of merger operations.
* The next `n` lines each contain two strings `A` and `B`, denoting that subsidiary `B` is merged into subsidiary `A`.

---

### 📤 **Output Format**

* Print all **top-level subsidiaries** (representatives of each group) in **lexicographical order**, each separated by space.

---

### 📌 **Constraints**

* 1 ≤ n ≤ 10⁴
* All subsidiary names are strings of uppercase letters (1 to 10 characters)
* Subsidiary names are case-sensitive and unique

---

### 🧪 **Sample Input 1**

```
5
A B
B C
D E
F G
E F
```

### ✅ **Sample Output 1**

```
A D
```

---

### 🧪 **Sample Input 2**

```
3
X Y
A B
Y Z
```

### ✅ **Sample Output 2**

```
A X
```

---

## 🧠 **Approach**

We will use **Disjoint Set Union (Union-Find)** data structure to keep track of mergers:

1. **Each subsidiary starts as its own parent.**
2. **For every merge (A, B)**:

   * Find parent of A → `parentA`
   * Find parent of B → `parentB`
   * Set `parent[parentB] = parentA` to merge B into A’s group
3. **Finally**, find the **unique root parents** for all subsidiaries.

We'll use **path compression** in `find()` for optimization.

---
---
---

## ✅ **Problem: 6. Network Technician Mission**

### 🔷 **Problem Description:**

A network technician needs to visit multiple malfunctioning routers in a network. The routers are connected with network cables having certain weights (representing time delay or cost of travel).

The technician starts at a central router (Router 0), visits all other routers exactly once, and returns back to the starting point. Your task is to design a path that takes **minimum total time**, and print that minimum time.
Due to time constraints, the exact solution isn't feasible for large graphs, so you may use a **Travelling Salesman Problem (TSP) approximation**.

---

### 🔷 **Input Format:**

* First line: Two integers `n` and `e` — number of routers and number of connections.
* Next `e` lines: Each line contains three integers `u`, `v`, and `w` — meaning router `u` is connected to router `v` with a weighted edge of cost `w`.

---

### 🔷 **Output Format:**

* Print one integer — the **minimum approximate traversal time** required to visit all routers and return to the start.

---

### 🔷 **Constraints:**

* `2 ≤ n ≤ 1000` (routers)
* `1 ≤ e ≤ n*(n-1)/2`
* `0 ≤ u, v < n`
* `1 ≤ w ≤ 10^4`
* Graph is **connected and undirected**

---

### 🔷 **Sample Input 1:**

```
4 6
0 1 10
0 2 15
0 3 20
1 2 35
1 3 25
2 3 30
```

### 🔷 **Sample Output 1:**

```
80
```

---

## ✅ **Approach**

We use a **2-approximation of the Travelling Salesman Problem**:

1. **Build a Minimum Spanning Tree (MST)** using Prim's algorithm.
2. **Perform DFS traversal** of the MST starting from router 0.
3. Add the cost of returning to the starting router to complete the tour.
4. The total traversal cost gives a good approximation.

---
---
---


## ✅ Problem: **5. Power Grid Management**

### 🔹 Problem Description

The government is planning to connect **all cities with a power grid** using cables. Each connection between two cities has a **certain cost**. Your task is to **connect all cities** such that the **total cost is minimized**.

You are given:

* Number of cities `n` (labelled from `0` to `n-1`)
* A list of possible connections in the form of weighted edges (u, v, cost)

You must determine the **minimum total cost** required to connect all cities such that power can flow between any two cities, **directly or indirectly**.

If it is **not possible** to connect all cities, return `-1`.

---

### 🔹 Input Format

```
n m
u1 v1 cost1
u2 v2 cost2
...
um vm costm
```

* `n`: Number of cities
* `m`: Number of available connections (edges)
* Each of the next `m` lines contains three integers `u`, `v`, and `cost` — representing a connection between city `u` and city `v` with given cost.

---

### 🔹 Output Format

```
Minimum Cost to Power All Cities: <cost>
```

or

```
-1
```

---

### 🔹 Constraints

* 1 ≤ n ≤ 10⁴
* 0 ≤ m ≤ 10⁵
* 0 ≤ u, v < n
* 1 ≤ cost ≤ 10⁶

---

### 🔹 Sample Input 1

```
4 5
0 1 1
0 2 4
1 2 2
1 3 3
2 3 5
```

### 🔹 Sample Output 1

```
Minimum Cost to Power All Cities: 6
```

### 🔹 Sample Input 2

```
4 2
0 1 3
2 3 4
```

### 🔹 Sample Output 2

```
-1
```

---

## ✅ Approach

To minimize cost and still connect all cities, we need to form a **Minimum Spanning Tree (MST)**:

* An MST connects all nodes with **minimum possible total edge weight** and **no cycles**.

### 🔸 Algorithm Used: **Kruskal’s Algorithm**

* Build an **edge list**.
* **Sort** edges by cost.
* Use a **Disjoint Set Union (DSU)** to track connected components.
* For each edge:

  * If the cities are **not already connected**, include the edge in MST.
  * Continue until **n - 1 edges** are included.
* If less than `n - 1` edges are used → Graph is **disconnected** → Return `-1`.

---
---
---
