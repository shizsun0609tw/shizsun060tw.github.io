---
title: AI-Engine
date: 2019-06-15 17:43:20
tags:
- Computer Graphic
category:
- Computer Graphic
---

## AI Engine

* [Introduction](#Introduction)
* [Intelligent Agent](#Intelligent-Agent)
* [AI Model](#AI-Model)
* [Hierachical Behavior Tree](#Hierachical-Behavior-Tree)
* [Finite State Machine](#Finite-State-Machine)
* [Fuzzy Logic](#Fuzzy-Logic)


<!--more-->

---

### Introduction

在 3D 遊戲設計所定義的 AI 引擎通常是指

* 能接受資訊
* 能有計畫性的計畫性的判斷
* 能有所回應

---

### Intelligent Agent

Intelligent Agent 中文稱作<font color='red'>智慧代理人</font>
用於管理底下所有物件要做出何種反應的管理處(virtual 的存在)
簡單來說就是工頭 XD

這個工頭要負責所有外在進來的資訊(input variable)
可能是天氣太熱或是下大雨，或是內部的溝通資訊(interaction)，可能是員工吵架或是員工中暑

蒐集完所有資訊之後這個工頭要判斷接下來這些員工要有甚麼樣的動作(action)與反應(responce)
例如員工中暑可以指派讓員工休息或是下大雨員工們先暫時躲雨等等

某些時候可能也要跟其他的工地的工頭溝通協調各工地的進度狀況(interact with other IAs)

比喻到這邊 <font color='red'>為甚麼會需要 IA 去控制我們的物件，不讓他們各自控制呢?</font>

由於在實作 AI Engine 部分的程式，如果將各腳色的 AI 分開容易造成同群體間不好溝通， input variable 十分混亂，不同種類的 AI 也難以溝通，為了 implement 的方便，因此才使用此方法來實作

<font color='red'>不過要小心一個 IA 下互相溝通時先後順序的 bug</font>

---

### AI Model

通常在撰寫 AI 判斷部分會用兩種類的 model

* Deterministic
  * Finite state machine
  * Hierachical finite state machine
  * Push down automata
* Non-deterministic
  * Fuzzy logic
  * Neural network
  * Genetic algorithm
  * Non-deterministic automata

Deterministic 比較直觀簡單，收到甚麼樣的 input 就做出對應的反應，<font color='red'>雖然程式好寫，但彈性跟有趣度就較低，也沒有不同程度上的差別</font>

Non-deterministic 較為複雜，可能透過不同種複雜的方式才得出最後的反應，<font color='red'>程式難寫，但彈性跟反應也較多種類的變化</font>

---

### Hierachical Behavior Tree

我們在讓腳色有反應動作時， IA 可能只有下達攻擊敵人、躲避或是逃跑等較不明確的至另，因此要透過 Hierachical Behavior Tree 去控制腳色的行為，例如: 

* Run from enemy = turn + run
* turn = rotate + change gesture
* run = swing arms + motion of feets + translate + change gesture

將他們分寫成較細且清楚的指令，最後只剩下旋轉及移動時再來 update

---

### Finite State Machine

**Finite State Machine**

FSM 這東西如果你再資訊領域學了一陣子應該不陌生
就是最直接的得到甚麼 input 就透過 transition 到不同的 state
<font color='red'>簡單 好寫，但無記憶性 也較無聊</font>
通常可以用兩種方法 implement

* Event-driven 就是最直接的 if else 直接去跑 不多說 
* Table-driven 可以使用一個 table 把狀態關係定義好，在比較多狀態的時候 implement 才不會一團亂

**Extends**

雖然 FSM 這東西很簡單，但也可以用一些其他的拓展方法讓他有更多複雜的反應，例如

* 使用多個 FSM 建出 Hierachical FSM 
* 將 FSM 的 state 放進 stack 裡就讓他有了記憶功能
* 用 DFA 取代 NFA 可以更有隨機性
* 用 Push down Automata 做出更複雜的 script

---

### Fuzzy Logic

由於 FSM 基本上就是得到甚麼 input 就做出甚麼 responce 所以沒辦法有程度上的區別，例如 101m 跟 999m 可能都代表遠，而且也只能對一個 input 做反應，這時候就要出動 fuzzy logic 。

Fuzzy logic 就是在<font color='red'>判斷時以百分比做為判斷條件，再加上 fuzzy operator(and, or, not)，把多種條件複合起來再做反應</font>，例如:

speed 50 m/s => 50% * 100 m/s => 50%(normal speed)
distanc 100 m => 10% * 1000m => 10%(far)

50% and 10% => 10% => run normal
