# 🧩 Problem: 1. Maximum Weight Node in a Maze

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
# 🧩 Problem: 2. Nearest Meeting Cell

## 🧩 **Problem Description**

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
