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

<!--more-->

---

### BFS

Breadth-First-Search

* O(m+n)
* queue
* boolean to record explored/unexplored
* layer to record dist()

#### Pseudocode

```
while Q != φ:
    remove the first of Q, call it v
    for each edge(v, w):
        if w unexplored:
            dist(w) = dist(v) + 1
            mark w as explored
            add to Q
```

---

