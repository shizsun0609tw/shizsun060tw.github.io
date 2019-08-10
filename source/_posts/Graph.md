---
title: Graph
date: 2019-08-04 10:57:13
tags:
- Algorithm
category:
- Algorithm
---

## Graph

* [BFS](#BFS)
* [DFS](#DFS)
* [Connected Component](#Connected-Component)
* [Topological Sort](#Topological-Sort)
* [Strong Connected Component](#Strong-Connected-Component)

<!--more-->

---

### BFS

Breadth-First-Search

* O(m+n)
* queue
* boolean to record explored/unexplored
* layer to record dist()

#### Pseudocode

```C++
void BFS-LOOP(){
    for i = 1 to n:
        if i unexplored:
            BFS(G, i)
}

void BFS(Graph G, StartVertex S){
    mark S explored
    let Q = queue data structure /* init with S */
    while Q != φ:
        remove the first of Q, call it v
        for each edge(v, w):
            if w unexplored:
                dist(w) = dist(v) + 1
                mark w as explored
                add to Q
}
```

---

### DFS

Depth-First-Search

* O(m+n)
* stack
* boolean to record explored/unexplored
* recursive

#### Pseudocode

```C++
void DFS-LOOP(){
    for i = 1 to n:
        if i unexplored:
            DFS(G, i)
}

void DFS(Graph G, StartVertex S){
    mark S explored
    for each edge(S, v):
        if V unexplored:
            DFS(G, v)
}
```

---

### Connected Component

* You can use BFS-LOOP or DFS-LOOP to traverse the whole graph and you will find connected component at the same time.

---

### Topological Sort

{% asset_img TopologicalSort-1.png %}

如果你想修 3D 遊戲設計，你必須先修過電腦圖學，這種有條件的先後順序性的排成之序列稱為 topological sort
Example: 線性代數 -> 訊號與系統 -> 電腦圖學 -> 3D 遊戲設計

* DAG (directed acyclic graph) # 有向無環圖
* DFS

#### Pesudocode

current_label 用來記錄DFS離開的順序，從最深的標上 n 依序離開時遞減到 1

```C++
void DFS-LOOP(){
    current_label = n
    for i = 1 to n:
        if i unexplored:
            DFS(G, i)
}

void DFS(Graph g, StartVertex S){
    mark S explored
    for each edge(S, v):
        if v unexplored:
            DFS(G, v)
    set f(S) = current_label
    current_label--
}
```

---

### Strong Connected Component

如果有向圖內含有環可以將環當作 strong connected component

#### Kosaraju's Two-Pass Algorithm

* O(m+n)
* DFS * 2

#### Step

* Let G<sup>rev</sup> = G with all arcs reversed
* run DFS-LOOP on G<sup>rev</sup>   # get DFS finish time on G<sup>rev</sup>
* run DFS-LOOP on G                 # use finish time to run DFS-LOOP on G

#### Psudocode

```C++
void DFS-LOOP(){
    global int t = 0            // DFS finish time
    global Node S = NULL        // for leaders in 2nd pass
    Assume nodes labeled 1 to n
    for i = n down to 1:
        if i unexplored:
            s = i
            DFS(G, i)    
}

void DFS(Graph G, node i){
    mark i as explored          // for reset DFS-LOOP
    set leader(i) = node S
    for each arc (i, j) in G:
        if j unexplored:
            DFS(G, j)
    t++
    set f(i) = t                // finish time
}
```

#### How does it work?

reverse 後會與原本找到相同的 strong connected components ，在 reverse graph 上可以倒著將 graph travese 一遍
而記錄回來的倒序可以將原圖依類似拓樸排序法的方式 traverse 使得 strong connected components 可以依序被拿下

---

