---
title: Special Effect
date: 2019-06-17 14:13:06
tags:
- Computer Graphics
category:
- Computer Graphics
---

## Special Effect

* [Shadow](#Shadow)
* [Planar Reflect](#Planar-Reflect)

<!--more-->

---

### Shadow

* [Shadow Polygon](#Shadow-Polygon)
* [Shadow Map](#Shadow-Map)

#### Shadow Polygon

**Introduction**

Shadow Polygon 是使用幾何學的方法將會產生影子的部分計算出來

* shadow caster (使人產生影子的遮擋物)
* shadow receiver (影子所在的被遮擋物)

我們可以透過由光源點對 shadow caster 發出的射線投影到 shadow receiver ，而形成的面積就是 shadow

{% asset_img Shadow00.jpg shaodw %}


<font color='red'>如果我們要知道如何投影，我們需要知道光源, 被投影點, 投影點之間的關係</font>

假設:

射線方程式為 
> r(t) = L + (P - L)t

receiver 平面方程式為 
> <N, Q> + d = 0

則
> <N, L + (P - L)t> + d = 0
> <N, L> + <N, (P - L)t> + d = 0

> <font color='red'>t = -(<N, L> + d) / (<N, (P - L)t>)</font>
> <font color='red'>Q = L - (P - L)(<N, L> + d)/(<N, P - L>)</font>


{% asset_img Shadow01.jpg shadow1 %}

我們知道光源, 被投影點, 投影點之間的關係後就能將被投影部分的 fragment 透過產生出來的投影矩陣投影

{% asset_img Shadow02.jpg ProjectionMatrix %}

<font color='red'>實作時只要將此矩陣投影出來的 polygon 只留 ambient ，將 diffuse, specular 都消除就能有產生 shadow 的效果</font>

**Clipping**

在時做的時候的能會發生 shadow 超出原本的被投影平面的狀況。為了解決這狀況，我們需要使用 clipping

{% asset_img Shadow03.jpg clipping %}

我們可以使用 stencil buffer 去記錄哪些區塊是本來就要被 render 出來的，在計算 shadow 的時候只有本來會被 render 出來的區塊才會有影響

#### Shadow Map

**Introduction**

Shadow Map 是將我們的 eye 先移動到光源處，然後做出 view volume 
透過 view volume 往內看(render)可以發現哪些部分是看得見而哪些是被遮擋的<font color='red'>(將 front face cull 掉留下的就是可能被擋住的)，然後再取出 Depth Buffer 製成 Shadow Map</font>

<font color='red'>Shadow Map = Depth Buffer</font>

最後在 render 的時候，將深度較深的部分做成 texture 直接貼上

{% asset_img Shadow04.jpg ShadowMap %}

---

### Planar Reflect

**Method1**

將眼睛反射至鏡面內的坐標系 -> render 製成 image 1
將眼睛退回原本坐標系 -> render 製成 image 2
blend image1 on image2

* Reflect 3 view parameters
* Light is not reflected
* Blending is required
* Back face culling must be turn off 

**Method2**

將場景與光線反射至鏡面，將反射面以外 clip 掉，再 render 原場景

* Reflect the scene and light
* Light is reflected
* Blending is required 

**<font color='red'>Reflect</font>**

先將欲反射點轉換到反射面上再inverse的做一次一樣的動作可以達到反射點

假設平面為 ax + by + cz + d = 0  // <N, P> + d = 0

Step1

製作 translate matrix T

{% asset_img T.jpg Traslate %}

Step2

製作 rotate matrix R

R = {U, N, V}

<font color='red'>U, V 平行於鏡面 N 垂直於鏡面(法向量)</font>

{% asset_img R.jpg Rotate %}

之後透過 <U, U> = 1, <V, V> = 1, <U, V> = 0, <U, N> = 0, <N, V> = 0 將 U, V 解出

Step3

製作 reflect matrix S

對 Y 軸(法向量 N 軸) 做反射

{% asset_img S.jpg Reflect %}

Step4

完整的運算

> M = T(dN) * R<sup>T</sup> * S(1, -1, 1) * R * T(-dN)

{% asset_img M.jpg Matrix %}

該算的算完就是該 Clipping 的 Clipping 該 Blending 的 Blending ~~~

