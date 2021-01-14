---
title: Animation
date: 2019-06-16 16:06:03
tags:
- Computer Graphics
- NTOU
category:
- Computer Graphics
---

## Animation

* [Introduction](#Introduction)
* [Rigid Body](#Rigid-Body)
* [Articulated Body](#Articulated-Body)
* [Deformable Body](#Deformable-Body)

<!--more-->

---

### Introduction

不管在電腦圖學還是遊戲設計，動畫都是很重要的一塊，有了動畫才能讓腳色或物件有較多的變化，而實作動畫的方法也有很多種，主要分成三個種類

* Rigid Body
* Articulated Body
* Deformable Body

---

### Rigid Body

剛體運動，定義上為<font color='red'>不會產生形變的物體</font>，基本上是整體的一起運動

* 變化較少
* 程式較好寫

**Impletation**

可以先將軌跡運算出來，在計算每個時間點(frame)應該是動畫的哪一個畫面

**Cubic Spline**

Cubic 運用三次式做内插，逼近預期軌跡
Spline 簡單來說為多段分開內插的函數，能比一般的內插函數更近似複雜的圖形


**<font color='red'>Bezier Curve</font>**

<font color ='red'>是一種參數化的曲線可以使用 r(t) 求出當前的位置與方向</font>

{% asset_img BezierCurve01.jpg BezierCurve %}

<font color='red'>圖上的位置代表物體的位置，而切線方向則代表物體的面向</font>

**Cubic Bezier Curve**

{% asset_img BezierCurve02.jpg BezierCurve %}

<br/>

{% asset_img BezierCurve03.jpg BezierCurve %}

而且他擁有三個性質

* r(0) = P<sub>0</sub>, r(1) = P<sub>3</sub>
* 節點將會形成將函數包起來的凸包
* P<sub>0</sub>P<sub>1</sub> 平行 P<sub>2</sub>P<sub>3</sub>

但 Bezier Curve 有個問題在於 <font color='red'>r(t) 的單位變化並不是等距的</font>，這會造成我們動畫在配合 frame 的時候有速度不一致的問題，因此我們需要用 Arc Length 來解決

**Arc Length**

Arc Length 就是將曲線的<font color='red'>距離</font>重新定義計算出來，讓每段的距離是相同的

{% asset_img ArcLength01.jpg ArcLength1 %}
{% asset_img ArcLength02.jpg ArcLength2 %}

有了 Arc Length 之後要做甚麼呢?

<font color='red'>我們可以製作出 t 與 s 的對應表</font>

這樣我們就可以運用 t 丟進去 r(t) 轉成 r(s) 這樣我們的動畫的表現就會等距了!

{% asset_img BezierCurve04.jpg LookUp-Table %}

---

### Articulated Body

有很多部件分開來的物體，基本上由 joint 把各個 rigid body 相連起來

* 變化很多
* 程式難度中等

**Impletation**

* Control Structure

基本上就是用 LCS (Local Coordinate System) 將每個 joint 建一個坐標系來操控

* Parameter Vector

因為每個 joint 都會需要 theta 的旋轉角度資訊，我們在實作的時候可以把所有部位包成一個 vector 傳進去，然後讓各個 joint 去存取需要的資訊

* Forward Kinematics 

將每個 joint 的角度傳進各自的坐標系在換算成目標的姿態

* Inverse Kinematics

先知道目標姿態再將中間每個時間點(frame)的角度變化換算出來

* <font color='red'>Skinning Method</font>

在 joint 外面再包上一層 skin 的 mesh 而 LCS 的原點一樣定在 joint 上，在控制 joint 的時候可以讓外層的 mesh 同時改變，讓觀察的效果更好

但在旋轉角度較大的關節可能會出現一些 skin 重疊或是斷裂的問題，這時可能會在關節部分在多加一層的 skin 讓彎曲時不會有斷裂的現象

在現在的實作上其實會做兩三層以上的 skin 讓操縱起來更加的順暢，例如:

* Layer 0: skeleton
* Layer 1: muscle
* Layer 2: surface

---

### Deformable Body

<font color='red'>受外力會產生形變的物體</font>，實作方式可以使用 joint 建在物體外，讓這些 joint 取改變物體的形狀

* 變化最多
* 我還不知道怎麼寫QQ