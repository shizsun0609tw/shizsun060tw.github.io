---
title: Compiler-CFG
date: 2019-05-30 10:13:28
tags:
- Compiler
- NTOU
category:
- Compiler
---

## CFG(Context Free Grammar)

* [Introduction](#Introduction)
* [Parse Tree](#Parse-Tree)
* [NFA->CFG](#NFA-to-CFG)
* [CFG vs RE](#CFG-vs-RE)
* [Leftmost/Rightmost Derivation](#Leftmost-and-Rightmost-Derivation)
* [Elimination of Left Recursion](#Elimination-of-Left-Recursion)
* [Top-Down Parsing](#Top-Down-Parsing)
* [First and Follow](#First-and-Follow)

<!--more-->

---

### <font color='red'>Introduction</font>

##### Description
CFG(Context Free Grammar)中文翻作<font color='red'>上下文無關文法</font>
CFG 主要由 4 種成員組成 記為 G = (V<sub>N</sub>, V<sub>T</sub>, P, S)
* V<sub>N</sub>
  * nonterminals(非終止符號)
  * 可以換成別的符號的符號
* V<sub>T</sub>
  * terminals(終止符號)
  * 不可換成別的符號的符號
* P
  * production(產生式)
  * 換算式
* S
  * start symbol(開始符號) 
  * 從此符號開始做<font color='red'>(大部分都沒特別講就為第一個產生式的左符號)</font>

##### Example

P0: expression -> expression + term
P1: expression -> expression - term
P2: expression -> term
P3: term -> term \* factor | term / factor | factor
P4: factor -> (expression) | id

V<sub>N</sub> = {expression, term, factor}
V<sub>T</sub> = {+, -, \*, /, (, ), id}
P = {P0, P1, P2, P3, P4}
S = {expression}

---

### Parse Tree

##### Description

直接看實例比較快懂

##### Example

S -> SS+ | SS\* | a
String: aa+a\*

<div style='width:200px;' align='left'>
{% asset_img ParseTree01.jpg %}
</div>

S => S S \* => S S + S \* => a S + S \* => a a + S \* => a a + a \*

---

### NFA to CFG

##### Description

只要將各狀態拆成產生式(條件放前綴)

##### Example

(a|b)*abb
<div style='width:50%;' align='left'>
{% asset_img NFAtoCFG01.jpg %}
</div>
S<sub>0</sub> -> aS<sub>0</sub> | b S<sub>0</sub> | S<sub>1</sub>
S<sub>1</sub> -> bS<sub>2</sub>
S<sub>2</sub> -> bS<sub>3</sub>
S<sub>3</sub> -> ε

---

### CFG vs RE

##### Description

RE包含在CFG內 所以 <font color='red'>RE可以換CFG 可是CFG不一定能換RE</font>

<div style='width:20%;' align='left'>
{% asset_img REvsCFG01.jpg %}
</div>

##### Example

<font color = 'red'>RE to CFG</font>

(a|b)*abb 
=> 
A -> BC
B -> BB | a | b | ε
C -> abb

<font color='red'>CFG to RE</color>

A -> aAb | ε
=>
???


---

### <font color='red'>Leftmost and Rightmost Derivation</font>

##### Description

最左優先推導/最右優先推導，在將開始符號推至目標字串時從最左或最右的 nonterminal 開始推

##### Example

E -> E + E | E \* E | (E) | -E | id
String: id + id \* id

**Leftmost Derivation**

<div style='width:50%;' align='left'>
{% asset_img LeftMost01.jpg %}
</div>

E
=> E + E
=> id + E
=> id + E \* E
=> id + id \* E
=> id + id \* id

**Rightmost Derivation**

<div style='width:50%;' align='left'>
{% asset_img RightMost01.jpg %}
</div>

E
=> E \* E
=> E \* id
=> E + E \* id
=> E + id \* id
=> id + id \* id

**<font color='red'>Ambiguity</font>**

一個合法的語法不能 Ambiguity (模稜兩可)
如果同一個 String 能透過 Grammar 寫出兩種 Parse Tree 則此語法 Ambiguity

<div style='width:50%;' align='left'>
{% asset_img LeftMost01.jpg %}{% asset_img RightMost01.jpg %}
</div>

<font color='red'>我們上面的語法就讓 id + id * id 產生兩種 Parse Tree 因此上面的 Grammar 為 Ambiguity</font>

---

### <font color='red'>Elimination of Left Recursion</font>

##### Description

消除左遞迴，如果有一產生式為 E -> E + id ，則 Parse Tree 可能無限的往左展開，因此我們要消除左遞迴的情況

##### Example

**Ex1**
A -> Aa | b

可以轉成

A -> bA'
A' -> aA' | ε

**Ex2**
E -> E + T | E - T | T

可以轉成

E -> TE'
E' -> +TE' | -TE' | ε

---

### Top-Down Parsing

##### Description

就是用開始符號從上往下展開到目標字串

##### Example

E -> TE'
E' -> +TE' | ε
T -> FT'
T' -> *FT' | ε
F -> (E) | id

String: id + id * id

<div style='width:30%;' align=left>
{% asset_img TopDown01.jpg %}
</div>

---

### <font color='red'>First-and-Follow</font>

##### Description

First 和 Follow 是 function 寫作 First(a) 或 Follow(a)

**First**

* 參數可以是 terminal 或 nonterminal
* 找到接在參數<font color='red'>前面的字首</font>可能是哪些 terminal 的集合

**Follow**

* 參數只能是 terminal
* 找到接在參數<font color='red'>後面一個</font>可能是哪些 terminal 的集合

##### Example

**First**

沒甚麼訣竅 暴力展開往前找就對了 <font color='red'>記得如果看到 ε 要往後看一個</font>
<font color='red'>如果整個字串都可能為 ε 則也要將 ε 加進集合</font>

<font color='red'>ex1</font>

E -> TE'
E' -> +TE' | ε
T -> FT'
T' -> *FT' | ε
F -> (E) | id

First(E) = First(T) = First(F) = {(, id)}
First(E') = {+, ε}
First(T') = {*, ε}

<font color='red'>ex2</font>

S -> ACB | CbB | Ba
A -> da | BC
B -> g | ε
C -> h | ε

First(S) = {d, g, h, ε, b, a}
First(A) = {d, g, h, ε}
First(B) = {g, ε}
First(C) = {h, ε}

**Follow**
 
Follow 比較麻煩一點，有幾條小弟自己整理出來的規則可以參考看看

假設參數為 A

* A 是開始符號，在集合尾巴放進 \$
* 有條產生式為 S -> Ab ， b 為 terminal ，則把 b 加入進集合
* 有條產生式為 S -> AB ， B 為 nonterminal ，則把 <font color='red'>First(B)</font> 加入進集合
* 有條產生式為 S -> ABC， B 為 nonterminal <font color='red'>且 B 可能為 ε ，則把 Follow(B)</font> 加入進集合
* 有條產生式為 S -> AB ， B 為 nonterminal <font color='red'>且 B 可能為 ε ，則把 Follow(S)</font> 加入進集合

在換的過程中小弟是用有點像遞迴的方式去理解，邊用筆記錄換到哪才不會換到亂掉

<font color='red'>ex1</font>

E -> TE'
E' -> +TE' | ε
T -> FT'
T' -> *FT' | ε
F -> (E) | id

Follow(E) = Follow(E') = {), $)}
Follow(T) = Follow(T') = {+, ), $}
Follow(F) = {+, *, ), $}


<font color='red'>ex2</font>

S -> ACB | CbB | Ba
A -> da | BC
B -> g | ε
C -> h | ε

Follow(S) = {$}
Follow(A) = {h, g, $}
Follow(B) = {$, a, h, g}
Follow(C) = {g, $, b, h}