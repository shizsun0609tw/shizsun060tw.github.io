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
* 1 每一格只能有一種選擇(查表的時候只有一種可能)
* 不能有左遞迴
* 不能有 Ambiguous

**建表方法**

* 先畫出 x 軸為 terminal ， y 軸為 nonterminal 的表格
* 從每一個判斷式依序做
* 將產生式填入 (nonterminal, First(nonterminal)) 的位置
* 如果 First 內含有 ε ，則將產生式填入 (nonterminal, Follow(nonterminal)) 的位置

**Example**

*E -> TE'
E' -> +TE' | ε
T -> FT'
T' -> *FT' | ε
F -> (E) | id

First(E) = First(T) = First(F) = {(, id)}
First(E') = {+, ε}
First(T') = {*, ε}

Follow(E) = Follow(E') = {), $)}
Follow(T) = Follow(T') = {+, ), $}
Follow(F) = {+, *, ), $}

<div style="width:60%;">
{% asset_img LLtable01.jpg %}
</div>


**Parse**

* 根據 input 將開始符號放入 stack 中 向左展開至目標 nonterminal 後消除
* 查表讓 nonterminal 往 input 的目標 terminal 轉換

String: id + id * id

<div style="width:60%;">
{% asset_img LLtable02.jpg %}
</div>

**<font color='red'>How it works?</font>**

<font color='red'>
如果我們需要將開始符號往目標字串轉換<br/>
由於此方法屬於最左優先推導，因此將 first 放入表中後，透過查表就能知道此 nonterminal 要如何往左換出目標 terminal<br/>
如果遇到 ε 的情況，由於 nonterminal 被換成 ε 所以他的 follow 也有可能讓他展開到目標 terminal
</font>

---

### LL(0)

語法不存在著選擇性用來單一的判斷

##### Example

G -> AB
A -> BC
B -> C
C -> a

不管怎麼推都會推回去 a

---

### Bottom-Up Parsing

<font color='red'>由 input 開始推，推回開始符號</font>
基本上由 <font color='red'>shift(移入) reduce(化簡)</font> 組成

---

### LR(0)

##### Description

* L input 從左到右
* R 最右優先推倒
* 0 每次只會向前看零個
* 不能有 Ambiguous

**建表方法(Automation)**

* 先寫一條新的產生式，產生開始符號
* 將第一條產生式放入I<sub>0</sub>，並在右邊最前面放一 ●
* 將 ● 後面的符號遞迴展開，加在 I<sub>0</sub> 下方
* I<sub>0</sub>的每一成員將 ● 向後移一個，透過此符號產生新的 I<sub>k</sub>
* 產生出來 I<sub>k</sub> 的再繼續遞迴上面步驟

**Example**

S -> a B
B -> a B A B | ε
A -> + | *

=> S' -> S (先產生此產生式)

**建表方法(Parsing Table)**

* 

---