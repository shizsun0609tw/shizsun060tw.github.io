---
title: Compiler-Parsing
date: 2019-06-01 16:44:08
tags:
- note
- Compiler
category:
- Compiler
---

## Parsing

* [Introduction](#Introduction)

**Top-Down Parsing**

* [Top-Down Parsing](#Top-Down-Parsing)
* [LL(1)](#LL-1)
* [LL(0)](#LL-0)

**Bottom-Up Parsing**

* [Bottom-Up Parsing](#Bottom-Up-Parsing)

<!--more-->

---

### Introduction

Parsing 就是要將輸入字串對於我們定義的規則查詢、分析，確認 input 是否為我們定義出的合法規則 

我們要有系統、有規則的做 Parsing 而且還要有辦法寫成 Code 因此我們需要一些不同的 Parsing 方法，讓一切都是流程化的進行

簡單來說我們會透過 First 與 Follow 建查詢的 table ，讓我們在新輸入一個 input 時，能夠查表進行 Parse 並轉出成目標的語法構造

---

### Top-Down Parsing

<font color='red'>由開始符號開始推，推出目標的 input 語法</font>

---

### LL(1)

###### Description

* L input 從左到右
* L 最左優先推導
* 1 每一格只能有一種選擇 
* 不能有左遞迴
* 不能有 Ambiguous

**建表方法**

* 先畫出 x 軸為 terminal ， y 軸為 nonterminal 的表格
* 從每一個判斷式依序做
* 將產生式填入 (nonterminal, First(nonterminal)) 的位置
* 如果 First 內含有 ε ，則將產生式填入 (nonterminal, Follow(nonterminal)) 的位置

**Example**

**Parse**



---

### LL(0)

---

### Bottom-Up Parsing

<font color='red'>由 input 開始推，推回開始符號</font>

---