---
title: Optimization Algorithm
date: 2019-10-27 13:50:34
tags:
- Algorithm
- Math
---

## Optimization Algorithm

* [Introduction](#Introduction)
* [Mathematical Review](#Mathematical-Review)
* [Unconstrained Optimization](#Unconstrained-Optimization)
* [Linear Programming](#Linear-Programming)
* [Nonlinear Constrained Optimization](#Nonlinear-Constrained-Optimization)

<!--more-->

---

### Introduction

Optimization Alogrithm(最佳化演算法)簡單來說就是學習各種不同的演算法去找出各個問題解
解的方法可能很多種，要盡量找出最快或最準等等之類的演算法(會因為題目的不同因而使用不同演算法)

---

### Mathematical Review

#### Linear Dependent / Linear Indepent

如果有一向量滿足 α<sub>1</sub>a<sub>1</sub> + α<sub>2</sub>a<sub>2</sub> + ... + α<sub>k</sub>a<sub>k</sub> = 0
的解只有 α<sub>i</sub> = 0
代表此向量的為 Linear Dependent 線性獨立(任何一個向量都不能以其他向量組出)
反之則會出現 Linear Independent 線性相依(某些向量可透過其他向量組成)

#### Linear Combination

如果有一向量 α 與一些向量 a<sub>1</sub>, ..., a<sub>k</sub>
滿足 α = α<sub>1</sub>a<sub>1</sub> + ... + α<sub>k</sub>a<sub>k</sub>
則 a<sub>1</sub>, ..., a<sub>k</sub> 為 α 的 Linear Combination (線性組合)

#### Vector Space

Vector Space (向量空間)  會有一堆向量的集合(set)  (沒有限制)

如果 Vector Space 多上了範圍(field)所組成且滿足以下性質，則稱為此向量空間的 Subspace(子空間)
例如:
我們現在有一個向量空間(集合內有一些向量 v1, v2, ..., vk)
此時如果子空間的 field 為 R<sup>3</sup> ，則:

* 向量(x, y, z)的加法會落在 subspace 內
* 向量(x, y, z)的純量乘法會落在 subspace 內
* 0 向量會位於 subspace 內

#### Span

一些向量的所有線性組合稱為 span[a1, ..., ak] 且就為 subspace

#### Basis

將此空間下的向量轉換為都只剩下一個分量有值(所以會各向量會互相線性獨立)
則這些向量為此空間的 Basis (基底)

#### Rank

此矩陣內的向量互相線性獨立的有幾個這些則為此空間的基底也能透過這些基底組出空間(span)

#### Invertible Matrix

如果一矩陣 A 的行列式值不為 0 則為 Nonsingular Matrix (Ax = 0 只有 0 為解)
則此 A 矩陣為 Invertible Matrix (可逆矩陣)
會存在一反矩陣 AB = I 記為 B = A<sup>-1</sup> 

#### Determininant

Determininant (行列式) 指的是廣義上的面積(體積)

#### Eigenvalue and Eigenvector

Eigenvalue (特徵值) 和 Eigenvector (特徵向量) 會滿足以下性質

代數:

Av = λv
A 為已知矩陣  v 為 Eigenvector λ 為 Eigenvalue
det(A - λI) = 0

幾何:

Eigenvector 在經過 A 的 Transformation(映射)後 會與原本的向量成 λ 倍
因此 Eigenvector 不唯一(set) 互相成一個倍數 如在二維座標上就會成一條直線

---

### Unconstrained Optimization

---

### Linear Programming

---

### Nonlinear Constrained Optimization

---