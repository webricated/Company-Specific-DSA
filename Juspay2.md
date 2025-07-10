# 🧩 Problem Description: 1. Maximum Weight Node in a Maze

You are given a **maze with N cells**.
Each cell may have **multiple entry points**, but **at most one exit** — that is, entry/exit points are **unidirectional** like valves.

Each cell is uniquely named using an integer from `0` to `N-1`.

---

## 🎯 Objective

Find the node (cell) that has the **maximum weight**.

> ⚖️ **Weight of a node** = Sum of all node numbers (indices) that **point to it**.

---

## 🧾 Input Format

* The first line contains a single integer `N` — the number of cells.
* The second line contains `N` space-separated integers in the array `edge[]`, where:

  * `edge[i]` is the destination cell number that cell `i` points to.
  * `edge[i] = -1` means cell `i` does **not** have any outgoing edge.

---

## 📤 Output Format

* Output a single integer: the **node number** with the **maximum weight**.
* If multiple nodes have the same maximum weight, print the one with the **greatest index**.

---

## 🧪 Sample Input

```
23
4 4 1 4 13 8 8 8 0 8 14 9 15 11 -1 10 15 22 22 22 22 22 21
```

---

## ✅ Sample Output

```
22
```

---

## 🔍 Explanation

We build the weight of each node by summing the indices that point to it.

Example:

* Node 22 is pointed to by nodes: 17, 18, 19, 20, 21
* Weight of node 22 = 17 + 18 + 19 + 20 + 21 = **95**
* It has the **highest weight**, so output is `22`.

---

![009571ee-adcc-4fe8-ba5d-9fda2c6b9553_1652382460 7890408](https://github.com/user-attachments/assets/b6967ab4-135a-4837-838c-633a9d965f7f)

---
## ⭐ Code

```Java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // Input number of cells
        int N = sc.nextInt();

        // Input edges
        int[] edge = new int[N];
        for (int i = 0; i < N; i++) {
            edge[i] = sc.nextInt();
        }

        int result = findMaxWeightNode(edge);
        System.out.println(result);
    }

    public static int findMaxWeightNode(int[] edge) {
        int n = edge.length;
        int[] weight = new int[n];
        int ans = Integer.MIN_VALUE;
        int result = -1;

        for (int i = 0; i < n; i++) {
            int dest = edge[i];
            if (dest != -1) {
                weight[dest] += i;
                if (weight[dest] > ans || (weight[dest] == ans && dest > result)) {
                    ans = weight[dest];
                    result = dest;
                }
            }
        }

        return result;
    }
}

```
---
# 🧩 Problem Description: 2. Nearest Meeting Cell

You are given a **maze with N cells**.
Each cell has **at most one exit** but may have **multiple entry points** (like one-way valves).
The maze is represented using an array `edge[]`, where:

* `edge[i] = x` means there is a **direct path** from cell `i` to cell `x`.
* `edge[i] = -1` means cell `i` has **no exit**.

---

### 🎯 **Your Task**

Given two starting cells `C1` and `C2`, find the **Nearest Meeting Cell (NMC)**:

* A cell `Cm` that can be reached **from both C1 and C2**.
* Out of all such common reachable cells, choose the one for which **the maximum distance** from either C1 or C2 is **minimized**.
* If there are multiple such cells, return the one with the **smallest index**.
* If **no common reachable cell** exists, return `-1`.

---

## 🧾 Input Format

1. First line: Integer `N` — number of cells.
2. Second line: `N` integers representing `edge[]`.
3. Third line: Two integers `C1` and `C2`.

---

## 📤 Output Format

Print the index of the **Nearest Meeting Cell**. Print `-1` if no such cell exists.

---

## 🧪 Sample Input

```
23
4 4 1 4 13 8 8 8 0 8 14 9 15 11 -1 10 15 22 22 22 22 22 21
9 2
```

---

## ✅ Sample Output

```
4
```
![009571ee-adcc-4fe8-ba5d-9fda2c6b9553_1652382460 7890408](https://github.com/user-attachments/assets/b6967ab4-135a-4837-838c-633a9d965f7f)

---
## ⭐ Code

```Java
import java.util.*;

public class Main {

    public static int[] getDistances(int start, int[] edge) {
        int n = edge.length;
        int[] dist = new int[n];
        Arrays.fill(dist, -1);
        Set<Integer> visited = new HashSet<>();

        int curr = start, d = 0;
        while (curr != -1 && !visited.contains(curr)) {
            dist[curr] = d;
            visited.add(curr);
            curr = edge[curr];
            d++;
        }

        return dist;
    }

    public static int nearestMeetingCell(int[] edge, int c1, int c2) {
        int[] dist1 = getDistances(c1, edge);
        int[] dist2 = getDistances(c2, edge);

        int minDist = Integer.MAX_VALUE;
        int result = -1;

        for (int i = 0; i < edge.length; i++) {
            if (dist1[i] != -1 && dist2[i] != -1) {
                int maxD = Math.max(dist1[i], dist2[i]);
                if (maxD < minDist || (maxD == minDist && i < result)) {
                    minDist = maxD;
                    result = i;
                }
            }
        }

        return result;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int[] edge = new int[n];
        for (int i = 0; i < n; i++) edge[i] = sc.nextInt();

        int c1 = sc.nextInt();
        int c2 = sc.nextInt();

        int res = nearestMeetingCell(edge, c1, c2);
        System.out.println(res);
    }
}


```

---

# 🧩 **Problem Description: 3. Maximum Entry Points in Maze**

You are given a **maze with N cells**, represented by a one-way system of doors (unidirectional exits only).
Each cell can have:

* **At most one outgoing edge** (exit),
* But **multiple incoming edges** (entry points).

The maze is represented using an array `edge[]`, where:

* `edge[i] = x` means there's a **direct path from cell i → x**.
* `edge[i] = -1` means cell `i` has **no exit**.

---

### 🎯 **Objective**

You are required to find the **maximum number of entry points** (incoming edges) for **any single cell** in the maze.

---

## 🧾 **Input Format**

* First line: Integer `N` — total number of cells.
* Second line: `N` integers representing `edge[]` array.

---

## 📤 **Output Format**

* A single integer: the **maximum entry points** (incoming edges) among all cells.

---

## 🧪 **Sample Input**

```
23
4 4 1 4 13 8 8 8 0 8 14 9 5 11 -1 10 15 22 22 22 22 22 21
```

---

## ✅ **Sample Output**

```
5
```

---

## 🧠 **Explanation of Sample**

Let’s count how many times each cell appears in the `edge[]` array:

* For example:

  * Cell `4` is targeted by edge\[0], edge\[1], and edge\[3] → 3 entries
  * Cell `22` is targeted by edge\[17], edge\[18], edge\[19], edge\[20], edge\[21] → 5 entries

The cell with the **most incoming edges** is **cell 22**, with **5 entries**.

✅ So, the answer is `5`.

---
![009571ee-adcc-4fe8-ba5d-9fda2c6b9553_1652382460 7890408](https://github.com/user-attachments/assets/b6967ab4-135a-4837-838c-633a9d965f7f)

---
## ⭐ Code
```Java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int[] edge = new int[N];
        for (int i = 0; i < N; i++) {
            edge[i] = sc.nextInt();
        }

        int[] count = new int[N];  // count of incoming edges
        for (int i = 0; i < N; i++) {
            if (edge[i] != -1) {
                count[edge[i]]++;
            }
        }

        int maxEntry = 0;
        for (int c : count) {
            if (c > maxEntry) {
                maxEntry = c;
            }
        }

        System.out.println(maxEntry);
    }
}
```
---
