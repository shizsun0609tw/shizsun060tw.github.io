---
title: Compiler-Parsing
date: 2019-06-01 16:44:08
tags:
- Note
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
* [LR(0)](#LR-0)
* [SLR(1)](#SLR-1)
* [LALR(1)](#LALR-1)

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
* 1 每一個只會往前看1個
* 不能有左遞迴
* 不能有 Ambiguous

**建表方法**

* 先畫出 x 軸為 terminal ， y 軸為 nonterminal 的表格
* 從每一個判斷式依序做
* 將產生式填入 (nonterminal, First(nonterminal)) 的位置
* 如果 First 內含有 ε ，則將產生式填入 (nonterminal, Follow(nonterminal)) 的位置

**Example**

E -> TE'
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

因為是 0 所以他不會偷看 OuO

##### Example

G -> AB
A -> BC
B -> C
C -> a

不管怎麼推都會推回去 a

---

### Bottom-Up Parsing

<font color='red'>由 input 開始推，推回開始符號</font>

---

### LR(0)

##### Description

* L input 從左到右
* L 最右優先推導
* 0 每一個只會往前看0個
* 不能有 Ambiguous
* I = Item
* r = reduse
* s = shift

**建表方法**

先畫出 Automation 再利用畫出來的圖建表

**<font color='red'>Automation</font>**

* 先將所有產生式拆開並增加 S' -> S
* I<sub>0</sub>為 S' -> ●S 開始推
* ● 代表的是目前看到哪個位置
* 如果同一個 I 下，● 後為 nonterminal 則將其產生式加入
* 一個 I 完成後將每一個 ● 後還有東西的經過一個符號並產生新的 I 之後產生到結尾

**Example**

S -> a B
B -> aBAB | ε
A -> + | *
<div style="width:60%;">
{% asset_img LR0_automation.jpg LR0_Automation %}
</div>

**<font color='red'>Parsing Table</font>**

* x軸先放上 terminal 再放上 nonterminal 兩者隔開
* y軸放上 I
* 如果此 I<sub>j</sub> 可以透過某個符號到另一個 I<sub>k</sub>
  *  如果符號為 nonterminal 則在 (I<sub>j</sub>, nontermianl) 填上 S<sub>k</sub>
  * 如果符號為 terminal 則在 (I<sub>j</sub>, terminal) 填上 k
* 如果 I<sub>j</sub>內有產生式 P<sub>i</sub> 的 ● 已經在結尾，則將所有 nonterminal 填上 r<sub>i</sub>

<font color = 'red'>最後一項可能造成 Conflict</font>

**Example**

請看 SLR 的示範

---

### SLR(1)

##### Description

基本上跟 LR(0) 的流程相同，但在建表的時候最後一項

* 如果 I<sub>j</sub>內有產生式 P<sub>i</sub> ● 已經在結尾，則將<font color='red'>所有 nonterminal</font> 填上 r<sub>i</sub>

改為

* 如果 I<sub>j</sub>內有產生式 P<sub>i</sub> ● 已經在結尾，則將 <font color = 'red'>follow(P<sub>開始符號</sub>) </font>填上 r<sub>i</sub>

<font color='red'>解決 LR(0) 的 Conflict</font>

**Example**

S -> a B
B -> aBAB | ε
A -> + | *

<div style="width:40%;">
{% asset_img SLRtable.jpg SLRtable %}
</div>

**Parsing**

透過 table Parse
* 查看下個要進來的 input 需透過現在 stack.top() 的狀態經過何種操作才能得到
* 如果為 S<sub>i</sub> 將下個 input 放進 symbol ， Stack push(i)
* 如果為 r<sub>i</sub> 將 P<sub>i</sub>右式符號數量從 stack pop掉並代換掉 symbol<sub>suffix</sub> 再查看 stack.top() 的 nonterminal<sub>P開始符號</sub> 要 push 哪個 I

<div style="width:50%;">
{% asset_img SLRparsing.jpg SLRparsing %}
</div>

---

### LL(1)

##### Descrition 

* 在建立 Automation 的時候如果在 I 因拓展產生的 P 則偷看 ● 後的 first 並寫在後方(lookahead set)
* 在建表的時候 reduce 透過後方的 lookahead 而不是 SLR 的 follow

<font>lookahead 為 follow 的 subset 能減少 Conflict 的問題</font>

**Example**

S -> A a A b | B b B a
A -> ε
B -> ε

<div style="width:60%;">
{% asset_img LR1_automation.jpg LR1_automation %}
<br/>
</div>
<div style="width:40%;">
{% asset_img LR1_table.jpg LR1_table %}
</div>

---

### LALR(1)

##### Description

* 將 LR(0) 如果產生式皆相同，但 lookahead set不同的部分合併
* S 可以合併 R 不行

<font color='red'>減少表的大小</font>

