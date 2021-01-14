---
title: Quaternion
date: 2019-06-15 21:14:18
tags:
- Computer Graphics
- NTOU
category:
- Computer Graphics
---

## Quaternion

* [Introduction](#Introduction)
* [Description](#Description)
* [Operation](#Operation)
* [Vector vs Quaternion](#Vector-vs-Quaternion)
* [Reference](#Reference)
* [Extra Problem](#Extra-Problem)


<!--more-->

---

### Introduction

Quaternion 中文稱作四元數，是一種用來表示角度與用來旋轉的代數系統

我們一般認識的座標系統為 Euler 角，假設在三維空間下就是 (x, y, z) 表示對三個座標軸旋轉多少角度

<font color='red'>這個表示法不好嗎?</font>

由於我們的旋轉是<font color='red'>動態</font>的，因此在某些情況下會造成 [Gimbal lock](https://zh.wikipedia.org/wiki/%E7%92%B0%E6%9E%B6%E9%8E%96%E5%AE%9A)，簡單來說 Gimbal lock 就是歐拉角在旋轉到某些位置時<font color='red'>會造成某一軸的遺失，無法再進行那軸旋轉</font>的問題。而此問題是<font color='red'>無法解決的</font>，因此才會設計出 Quaternion 的代數系統。

---

### Description

Quaternion 是由 a + bi + cj + dk 所組成，其中 a, b, c, d 為實數 i, j, k 為虛數且

* i<sup>2</sup> = j<sup>2</sup> = k<sup>2</sup> = ijk = -1

很神奇吧 我也不知道為什麼呢 不過他就是這樣 所以接受吧 XD
另外還有

* ij = k, jk = i, ki = j
* ji = -k, kj= -i, ik = -j
* ii = jj = kk = -1

---

### Operation

> Q<sub>0</sub> = w<sub>0</sub> + ix<sub>0</sub> + jy<sub>0</sub> + kz<sub>0</sub>
> Q<sub>1</sub> = w<sub>1</sub> + ix<sub>1</sub> + jy<sub>1</sub> + kz<sub>1</sub>

**Addition**

> Q<sub>0</sub> + Q<sub>1</sub> = (w<sub>0</sub> + w<sub>1</sub>) + i(x<sub>0</sub> + x<sub>1</sub>) + j(y<sub>0</sub> + y<sub>1</sub>) + k(z<sub>0</sub> + z<sub>1</sub>) 

> Q<sub>0</sub> + Q<sub>1</sub> = Q<sub>1</sub> + Q<sub>0</sub>

**<font color='red'>Multiplication</font>**

> Q<sub>0</sub> * Q<sub>1</sub> = (w<sub>0</sub> + ix<sub>0</sub> + jy<sub>0</sub> + kz<sub>0</sub>)(w<sub>1</sub> + ix<sub>1</sub> + jy<sub>1</sub> + kz<sub>1</sub>)

> = (w<sub>0</sub>w<sub>1</sub> - x<sub>0</sub>x<sub>1</sub> - y<sub>0</sub>y<sub>1</sub> - z<sub>0</sub>z<sub>1</sub>)

> \+ i(w<sub>0</sub>x<sub>1</sub> + x<sub>0</sub>w<sub>1</sub> + y<sub>0</sub>z<sub>1</sub> - z<sub>0</sub>y<sub>1</sub>)

> \+ j(w<sub>0</sub>y<sub>1</sub> + y<sub>0</sub>w<sub>1</sub> - x<sub>0</sub>z<sub>1</sub> + z<sub>0</sub>x<sub>1</sub>)

> \+ k(w<sub>0</sub>z<sub>1</sub> + z<sub>0</sub>w<sub>1</sub> - y<sub>0</sub>x<sub>1</sub> + x<sub>0</sub>y<sub>1</sub>)

> <font color='red'>Q<sub>0</sub> * Q<sub>1</sub> != Q<sub>1</sub> * Q<sub>0</sub></font>

**Conjugate**

> Q<sub>0</sub><sup>*</sup> = (w<sub>0</sub> - ix<sub>0</sub> - jy<sub>0</sub> - kz<sub>0</sub>)

**<font color='red'>Norm</font>**

> ||Q|| = N(Q) = sqrt(w<sup>2</sup> + x<sup>2</sup> + y<sup>2</sup> + z<sup>2</sup>)

**<font color='red'>Inner Product</font>**

> \<q<sub>0</sub>, q<sub>1</sub>> = w<sub>0</sub>w<sub>1</sub> + x<sub>0</sub>x<sub>1</sub> + y<sub>0</sub>y<sub>1</sub> + z<sub>0</sub>z<sub>1</sub>

---

### Vector vs Quaternion

接下來就是要將我們平常用的三維 vector 轉成 quaternion
而他們的關係為 vector 屬於 quaternion 的 subspace
如果我們有一個向量 v 要轉成 quaternion 要記成 v = (0, v) ，其中 0 為實部
然後再運用 quaternion 的運算

如果要旋轉的話則使用

> q = (cos(&Theta;/2), sin(&Theta;/2)u)
> v' = qvq<sup>*</sup>

---

### Reference

* [了解四元數](https://www.zhihu.com/question/47736315)
* [教學影片1](https://www.youtube.com/watch?v=3BR8tK-LuB0)
* [教學影片2](https://www.youtube.com/watch?v=zjMuIxRvygQ)

---

### Extra Problem

如果你看完 Quaternion 還是用歐拉角寫 code 再往下看 XD

<br/>
<br/>
這個額外的問題是關於歐拉角旋轉的時候由於軸是<font color='red'>有順序性的</font>，所以會被<font color='red'>旋轉的順序互相影響</font>，因此在 coding 時會因為<font color='red'>旋轉軸順序不同造成不當的亂轉</font>(如果你實作的方法是只記錄旋轉角度然後在每一個 frame 都重新由原位開始轉的話)

歐拉角是有順序性的，想想如果我們<font color='red'>只記錄角度且在每一個 frame 重新旋轉</font>會有甚麼問題?

一般來說，我們每次做旋轉的時候應該是以<font color='red'>當前姿態去做旋轉</font>但如果只記錄角度而每一個 frame 重新轉會變成<font color='red'>初始的姿態做旋轉</font>

比方說我們有架飛機，我們請他 x 軸旋轉 30&deg; y軸 旋轉 30&deg; x軸再旋轉 30&deg; 
我們累積下來的角度為 x = 60&deg; y = 30&deg; 
這時如果我們直接用這角度做出旋轉矩陣去旋轉<font color='red'>是不會跟我們原本所期望的姿態 x 30&deg;, y 30&deg;, x 30&deg;不一樣的!!</font>

<font color='red'>這就是因為歐拉角的順序性</font>


**Solution**

這方法算是碰巧在亂試之下試對的，當時也不知道為什麼，現在才仔細地想清楚原因 XD

* <font color='red'>每一個 frame 在旋轉的時候都將旋轉的角度累積到一個旋轉矩陣上</font>，最後 model matrix 再乘以這個旋轉矩陣，取代每一個 frame 重新計算的旋轉矩陣

<font color='red'>How it works?</font>

由於我們是在每一個 frame 上累積旋轉的角度在我們的旋轉矩陣上，因次我們每次旋轉的角度都是代表著<font color='red'>當前那個 frame 的姿態</font>，因次我們是<font color='red'>維持我們旋轉的順序將矩陣累積起來</font>，最後再將 model matrix 乘上他，他就是依照我們的所旋轉的順序做旋轉了!!