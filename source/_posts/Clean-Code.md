---
title: Clean Code
date: 2019-07-17 10:02:59
tags:
- Note
- Software Engineering
category:
- Software Engineering
---

## Clean Code

* [Introduction](#Introduction)
* [Name](#Name)
* [Function](#Function)
* [Comment](#Comment)
* [Format](#Format)
* [Object And Data Structure](#Object-And-Data-Structure)
* [Error Handling](#Error-Handling)
* [Boundary](#Boundary)
* [Unit Test](#Unit-Test)
* [Class](#Class)

<!--more-->

---

### Introduction

本篇文章是閱讀 [Clean Code](https://www.google.com/search?q=Clean+code) 這本書寫下來的筆記
雖然 Clean Code 在剛開始撰寫上會比較花時間去思考，但我想這必須是多練習後融在你的血液裡的
雖然開發速度可能較慢，但在尋找 bug 上絕對比骯髒的 code 來的迅速許多，實際開發下來的時間並不會差太多的!
而且如果你能將這些技巧觀念融在血液裡，我想開發效率也不會低到哪裡去

以下使用作者代替寫 code 的人，讀者代替讀 code 的人
以下使用 X 代表避免使用這樣方式撰寫， 使用 O 代表應修正為此種寫法較為恰當

---

### Name

<font color='red'>**一眼看出其名稱代表的意義**</font>

* [有意義的區別](#有意義的區別)
* [能夠唸出來的名稱](#能夠唸出來的名稱)
* [可被搜尋的名字](#可被搜尋的名字)
* [類別的命名](#類別的命名)
* [方法的命名](#方法的命名)
* [每個概念用一種字詞](#每個概念用一種字詞)
* [別說雙關語](#別說雙關語)
* [使用方案領域的命名](#使用方案領域的命名)
* [使用問題領域的命名](#使用問題領域的命名)
* [避免使用過長的複合名稱](#避免使用過長的複合名稱)

---

#### 有意義的區別

##### X

如果今天我們有一個 Product 類別，如果又有 ProductInfo, ProductData 等類別(變數亦同)
那並沒有辦法在第一眼就分辨出這些名稱的關係，無法直接區分他們的意義

也不要使用 NameString, CustomerObject 等把資料型態放在變數上的命名

##### O

不使用相近意思的變數命名，讓每個變數都能正確一眼看出其內容

---

#### 能夠唸出來的名稱

##### X

我們常常因為懶惰所以將變數名稱簡寫，簡寫到不能一眼直觀的看出意思或正確的唸出
畢竟我們在讀 code 的時候其實也會依賴在腦中不自覺中念出的意思，取個沒有辦法很直接唸出的簡寫是會影響閱讀的

```C++
class DtaRcrd{}
```

##### O

正確的寫出完整可唸出的名稱

---

#### 可被搜尋的名字

##### X

不使用數字藏在程式碼內
不便修改，不便搜尋，也無法看出其意

```C++
for(int i = 0; i < 30; ++i){
    sum += 2000;
}
```

也不要用 i, v, e 等單字母作為變數名稱 (迴圈迭代為慣用的例外)
用這些單字就會讓 Crtl + F 或是讓 IDE 跳出補齊字時跑出一大堆的結果

```C++
vector v;
float e;
```

##### O

使用正確的變數名字取代數字

```C++
const int DAYS_PER_MONTH = 30;
const int SALARY_PER_DAY = 2000;
int salary = 0;
for(int i = 0; i < DAYS_PER_MONTH; ++i)}{
    salary += SALARY_PER_DAY;
}
```

避免簡寫以及簡短變數

```C++
vector customer;
const float ERROR;
```

---

#### 類別的命名

##### O

class 和 object 應該使用名次或名詞片語，且避免 Manager, Processor, Data, Info 等需揣測的字詞

---

#### 方法的命名

##### O

function 應該使用動詞或動詞片語(accsssors 用 get 當字首, mutators 用 set 當字首, predicates 用 is 當字首)

---

#### 每個概念用一種字詞

##### X

避免同時使用 manager, controller 會讓人混淆類似的字義，請統一使用一種

---

#### 別說雙關語

##### X

避免同一個字詞底下卻可能做不同的事情，例如: add 有些地方代表相加，有些地方代表新增

---

#### 使用方案領域的命名

##### O

可以盡量使用 CS 領域的術語，因為通常使用這些術語讀者就能一眼看出其意，例如: 演算法名字

---

#### 使用問題領域的命名

##### O

如果沒有比較好的 CS 領域術語，就使用問題領域的術語吧! 至少讀者可以透過詢問相關領域人員其意思

---

#### 避免使用過長的複合名稱

##### X

有時候會因為詞意的關係寫出非常冗長的複合變數名稱，不僅造成閱讀上的困難，還會造成多個變數名稱無法用眼睛一眼清楚的分辨

##### O

如果變數名稱相當長(含有多個名詞)，通常代表使用了些無意義的字詞或是重複的字詞來自於另一個抽象的概念，把他們包成 class 吧!

---

### Function

<font color='red'>**簡短，一次只做一件事**</font>

* [簡短](#簡短)
* [只做一件事情](#只做一件事情)
* [降層準則](#降層準則)
* [Switch](#Switch)
* [使用具描述能力的名稱](#使用具描述能力的名稱)
* [函式的參數](#函式的參數)
* [要無副作用](#要無副作用)
* [輸出型的參數](#輸出型的參數)
* [指令和查詢的分離](#指令和查詢的分離)
* [使用例外處理](#使用例外處理)
* [結構化程式設計](#結構化程式設計)

---

#### 簡短

##### O

每個 function 都應該盡量的簡短，最多不應該大於 20 行 應維持在 10 行以下
每個 function 不應該大到包含巢狀結構，縮排不應該大過一或兩層
簡短的內容配上良好的命名能使讀者第一時間就直接看出做了甚麼事且 bug 也將很容易顯露出來

---

#### 只做一件事情

##### X

在簡短的前提下， function 的大小也不能大到有分段，這代表你的 function 一次做了不只一件事

##### O

每個 function 應該做出符合命名的事情，這讓讀者可以簡單明瞭內容，將內容切小也可以 bug 更難出現
每個 function 只有一層抽象概念

---

#### 降層準則

##### O

我們希望程式的閱讀像是由上而下的故事，而每個函式後面都緊接著下一層次的抽象概念
將每一層的層次分割出來

---

#### Switch

##### X

Switch 通常很難簡短，而且只要新增一種類別可能就要進來修改，或是造成多個地方都要一直重複使用 switch 去做撰寫

##### O

我們可以盡量使其埋藏在底部的多型生成，而之後用 override 就不需要多處都需要判斷為哪種衍生類別

---

#### 使用具描述能力的名稱

##### O

每個 fumction 應該有個一目了然的命名
一個較長但具描述性質的名稱比一個較短且難以理解的名稱來的好

```C++
includeSetupAndTeardownPages();
```

---

#### 函式的參數

##### X

多個參數容易造成參數傳錯，無法控管好各個參數，無法一目了然的看出參數的作用

##### O

盡量越少越好，最好為 0 個，最多不超過 3 個，如果性質類似的就再抽成新的 class 吧!
(有時候我們希望傳遞不同數量的參數，如果不同數量的參數本質為同一種則可以視為一個參數)

```java
public String format(String format, Object... args)
```

---

#### 要無副作用

##### X

在 function 裡不可做出與其名稱一眼無法看出的不符內容，這將會使 function 做出無法預期的結果造成時空的混亂!!
例如: 在沒有預期會修改變數的 function 或是不對的時間點 initial 或是 set 到不應該修改的值
寫出這樣的 function 也代表著你違反了"一次只做一件事"的準則
(這種 bug 真的很難找，我自身之前也常遇過這樣子的 bug)

---

#### 輸出型的參數

##### X

有些人會使用參考或是指標的方式傳進 function 改值或是以回傳值再進行修改，會依語言與習慣的不同有不一樣的狀況
但這會造成使用上的困擾，需要擔心傳進的值是否會被修改到，或是我是否需要接收回傳值再進行修改

##### O

請統一寫出一樣風格的格式
更好的方法應該使用物件導向的方式，讓他自己修改自己!

---

#### 指令和查詢的分離

##### X

我們偶而會寫出
```java
public boolean set(String attribute, String value);
```
而使用起來像是
```java
if(set("username", "John")){}
```
我們知道你想表示的是此 function 會進行 set ， set 成功會回傳 true 失敗則為 false

但這是需要幾秒鐘的思考，且你必須去 set 的 function 確認是否與你預期想的一樣，也會造成思考的中斷
這也使這個 function 在語意上一次做了兩件事(set, is)

##### O

我們應該分割開來
```java
if(attributeExists("username")){
    setAttribute("username", "John");
}
```
這樣也能保持一次只做一件事的原則

---

#### 使用例外處理

##### X

有時候我們會用 if else 或是 enum 做出 ERROR 的處理，但這並不是個清楚的表示方法

##### O

我們應該使用 try/catch 將錯誤訊息提取出來，且不要寫出巢狀的 try catch 那會造成 code 的混亂
try/catch 也算一件事，因此一個 function 盡量保持一個 try/catch 就好
而且也可以將 try/catch 的內容再另外寫出一個 function 使其邏輯敘述較為完整好閱讀

```java
public void delete(Page page){
    try{
        deletePageAndAllReferences(page);
    }
    catch (Exception e){
        logError(e);
    }
}

private void deletePageAndAllReferences(Page page) throws Exception{
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey());
}

private void logError(Exception e){
    logger.log(e.getMessage());
}
```

---

#### 結構化程式設計

##### O

盡量讓每個 function 只有一個離開點(return)，這可以讓每個 function 的結果一定
(當然還是要依實際狀況彈性調整，但離開點一定是越少越好)

---

### Comment

<font color='red'>**不要替糟糕的程式寫註解，請重寫他，且如果沒有註解更好**</font>

* [有益的註解](#有益的註解)

---

#### 用程式碼表達你的本意

##### X

註解沒辦法彌補糟糕的 code ，而且有可能還會把模組用的一團亂

##### O

最好的方法就是讓程式碼一眼看去就能明瞭意思，不需要註解的輔助
反正你看完註解還是得看一次程式碼，有時候註解的時間都比看程式碼理解花的時間來的久了

---

#### 有益的註解

##### X

不寫出比觀看程式碼還難理解的註解
不寫出不精確影響思考的註解
不寫多餘的註解
不寫日誌型註解(交給版控吧)
不寫排版或標誌型註解
```C++
// Actions //////////////////////////////
```

##### O

有些公司可能會要求寫上作者或是法律相關註解，最好放在程式碼之外，以不影響程式碼閱讀為原則
有時候你並不是使用一般的演算法，而是你自己想的處理方式，而此光以程式碼無法容易的看出背後的意思，這時候需要註解
對於執行後果的注意，例如: 此測試案例會花費大量時間
TODO，待辦事項可以留下 TODO 但是請記得隔一些時間就要定期清理
註解數量應該要少，能不用就不用(用你的程式碼表達)，這樣留下來的任何註解都會被當作重點而被注意
註解應該要精準解釋，不廢話，且不讓人感到疑惑

---

### Format

<font color='red'>**增加可讀性，並使人感受到你的專業性**</font>

* [垂直的編排](#垂直的編排)
* [垂直距離](#垂直距離)
* [水平的編排](#水平的編排)
* [縮排](#縮排)
* [團隊的共同準則](#團隊的共同準則)

---

#### 垂直的編排

##### O

多數檔案應該都少於 200 行，較大的檔案也不應該超過 500 行
將不同思緒的區段以空白區分，思緒相近的緊密相連

---

#### 垂直距離

##### O

function 呼叫時盡量使其一個接一個，垂直距離盡可能縮短，要讓 function 呼叫呈現向下的相依性
盡可能讓操作的變數宣告在鄰近位置(由於 function 應該盡可能的短，因此以 function 最上方為原則)

---

#### 水平的編排

##### X

不需要做水平的對齊，那樣並不能較清楚的理解類別與變數的關係

##### O

寬度應在 100 ~ 120 字元以下，以能一眼看清楚整體面貌為原則
不同元素間可適當的空白區隔，相近的運算元素可以緊密相連

---

#### 縮排

##### X

避免程式碼塌陷成一行，這樣並不會比較好閱讀
```java
public CommentWidget(ParentWidget parent, String text){super(parent, text);}
public String render() throws Exception {return "";}
```

##### O

盡量維持 scope 的完整性，以及統一性
```java
public CommentWidget(ParentWidget parent, String text) {
    super(parent, text);
}

public String render() throws Exception {
    return "";
}
```

---

#### 團隊的共同準則

##### X

不要參入個人風格的程式碼編排

##### O

應該在開發之前統一定好簡單的編排原則，讓成員在互相閱讀 code 時能輕鬆的閱讀
而外人在閱讀時也能感受到團隊的專業性，且閱讀輕鬆順暢 

---

### Object And Data Structure

<font color='red'>**資料/物件的反對稱性**</font>

* [資料結構與物件](#資料結構與物件)
* [結構化與物件導向](#結構化與物件導向)
* [混和體](#混和體)
* [火車事故](#火車事故)
* [隱藏結構](#隱藏結構)

---

#### 資料結構與物件

我們在實作上經常遇到一個情況，這個類別的成員該如何操控或取得
我們應該把類別分為兩種，資料結構與物件
* <font color='red'>資料結構: 成員可以直接存取(public)，基本上沒有 member function ，結構簡單、無須介面而將成員直接暴露</font>
```java
public class Point {
    public double X;
    public double Y;
}
```
* 我們會清楚的知道 Point 是含有 X, Y 的直角座標系

* <font color='red'>物件: 成員需透過介面存取(private)，會有許多的 member function ，結構隱藏、透過抽象化界面對於成員操作</font>
```java
public interface Point {
    double getX();
    double getY();
    void setCartesian(double x, double y);
    double getR();
    double getTheta();
    void setPolar(double r, double theta);
}
```
* 我們會知道可以透過一些虛擬的介面去獲得相關資料，而並不需要知道底下是由直角座標或是極座標實作

---

#### 結構化與物件導向

* <font color='red'>結構化(使用資料結構的程式碼)</font>

* 容易添加新的函式，而不需要變動已有的資料結構

* 難以添加新的資料結構，因為必須改變所有的函式

```java
public class Square {
    public Point topLeft;
    public double side;
}

public class Rectangle {
    public Point topLeft;
    public double height;
    public double width;
}

public class Circle {
    public Point center;
    public double radius;
}

public class Geometry {
    public final double PI = 3.1415926;

    public double area(Object shape) throws NoSuchShapeException {
        if (shape instanceof Square) {
            Square s = (Square)shape;
            return s.side * s.side;
        }
        else if (shape instanceof Rectangle) {
            Square r = (Rectangle)shape;
            return r.height * r.width;
        }
        else if (shape instanceof Circle) {
            Circle c = (Circle)shape;
            return PI * c.radius * c.radius;
        }
        throw new NoSuchShapeException();
    }
}

```
* <font color='red'>物件導向</font>

* 容易添加新的類別，而不需要變動已有的函式

* 難以添加新的函式，因為必須改變所有的類別

```java
public class Square implements Shape {
    private Point topLeft;
    private double side;

    public double area() {
        return side * side;
    }
}

public class Rectangle implements Shape {
    private Point topLeft;
    private double height;
    private double width;

    public double area() {
        return height * width;
    }
}

public class Circle implements Shape {
    private Point center;
    private double radius;

    public final double PI = 3.1415926;
    public double area() {
        return PI * radius * radius;
    }
}
```
---

### 混和體

##### X

程式設計師應該要能判斷哪些類別使用物件導向哪些類別使用結構化來撰寫，沒有一定的偏向哪種
但混和體(半物件半資料結構)容易造成更加混亂，因此應該盡量避免

---

#### 德摩特爾法則

##### X

class 內不該有回傳底下物件的 function

---

#### 火車事故

##### X

有時候我們會寫出

```java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```
##### O

應該寫成
```java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String ouputDir = scratchDir.getAbsolutePath();
```

這樣還是違反了德摩特爾法則，因為他拿出了物件再對他進行操作不如思考是否為"資料結構"或是用"隱藏結構"的方法獲取

---

#### 隱藏結構

##### X

我們常常會寫出火車事故的 code
是因為要拿它去做別的事情的操作但那樣也違反了摩德特爾法則
```java
String outFile = outputDir + "/" + className.replace('.', '/') + ".class";
FileOutputStream fout = new FileOutputStream(outFile);
BufferedOutputStream bos = new BufferedOutputStream(fout);
```

##### O

我們可以用此方法代替，包在物件本身底下!
```java
BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
```

---

### Error Handling

<font color='red'>**錯誤處理很重要，但請不要影響原本整潔的程式**</font>

* [使用例外事件而非回傳錯誤碼](#使用例外事件而非回傳錯誤碼)
* [在開頭寫下你的 Try-Catch-Finally 敘述](#在開頭寫下你的-Try-Catch-Finally-敘述)
* [使用不檢查型例外](#使用不檢查型例外)
* [提供發生例外的相關資訊](#提供發生例外的相關資訊)
* [不要傳遞 null](#不要傳遞-null)

---

#### 使用例外事件而非回傳錯誤碼

##### X

盡量不要使用錯誤旗幟(flag)，或是回傳錯誤碼，這樣的程式並不整潔直觀

##### O

使用 Try-Catch-Finally 將錯誤捕捉

---

#### 在開頭寫下你的 Try-Catch-Finally 敘述

##### O

在開頭就先將 Try-Catch-Finally 的 scope 定出來可以幫助錯誤處理的思緒釐清，並讓程式碼一定會依照你所想的進行(就算有錯誤)

---

#### 使用不檢查型例外

檢查型例外(checked exceptions)，指的是是否需要在"所有"地方都加上例外處理 ?
在眾多討論的結果，我們認為在必須的地方加入就好，因為錯誤的丟出容易破壞"開放閉合準則"

---

#### 提供發生例外的相關資訊

##### O

拋出的例外應該要提供足夠的內文資訊，足以判斷錯誤的原因與位置，這個拋出的錯誤才是有益的

---

#### 不要傳遞 null

##### X

使用 null 去傳遞或回傳都是把問題丟給呼叫者的行為，使用上檢查起來不便，又會造成呼叫者的負擔

---

### Boundary

<font color='red'>**我們應該將各個套件、API、子系統切開，保持邊界的整潔**</font>

* [封裝第三方程式碼](#封裝第三方程式碼)
* [探索及學習邊界](#探索及學習邊界)
* [使用尚未存在的程式](#使用尚未存在的程式)

---

#### 封裝第三方程式碼

我們可以將第三方程式碼封裝起來再投過介面去操作，較能維持住我們程式的乾淨(不一定在所有情況下適用)

---

#### 探索及學習邊界

我們可以以學習式測試來練習使用我們不熟悉的 API
使用小程式片段邊學習 API 的使用方式，以及尋找 API 可能與你程式間造成的錯誤，或與你想像不同的執行結果
重點: 我們要用此 API 達到甚麼功能，功能執行結果是否與預期的相同

---

#### 使用尚未存在的程式

我們常常會遇到某部分的程式與你負責部分有相關，但負責人尚未將 Prototype 寫出來，使你不知道該如何繼續撰寫有關的部分
我們可以先預想出這些程式應該包含哪些"介面"是供你使用的，透過先將這些介面撰寫出來，再包裝再對方的程式上去做存取
這時候我們只需要透過簡單小部分的修改介面部分的程式就能很好的契合，也能維持"簡潔的邊界的整潔"!

---

### Unit Test

<font color='red'>**單元程式與主程式一樣重要**</font>

* [TDD](#TDD)
* [整潔的測試](#整潔的測試)
* [雙重標準](#雙重標準)
* [一個測試一個概念](#一個測試一個概念)
* [FIRST](#FIRST)

---

#### TDD

Test Driven Development

* (一) 在撰寫一個單元測試(測試失敗的單元測試)前，不可撰寫任何產品程式
* (二) 只撰寫剛好無法通過的單元測試
* (三) 只撰寫剛好能通過當前測試失敗的產品程式

---

#### 整潔的測試

##### X

測試程式不應該為了迅速省時就沒有依照系統程式的準則去撰寫(如:命名，簡短，說明性)

##### O

測試程式與系統程式一樣重要，當測試程式越來越龐大也會讓你無法去掌握整個測試程式，最後造成面臨被丟掉的命運
因此測試程式應該也用系統程式的準則來撰寫，要正確地命名，簡短，整潔...

---

#### 雙重標準

測試程式雖然要與系統程式有相同的軟體標準，但在某些部分可以有雙重標準
像是測試程式不一定要像系統程式一樣有效率，由於測試程式可能是在測試環境執行或是壓力測試等等

---

#### 一個測試一個概念

##### O

每個測試要符合"單元"測試的標準，不要有過多的事物操作或是複數個測試，這樣會使查錯便困難、相依性變高

---

#### FIRST

* Fast(快速) 
* Independent(獨立)
* Repeatable(可重複性)
* Self-Validating(自我驗證)
* Timely(及時)

##### Fast

單元測試要盡量夠快，否則你不會想執行

##### Independent

單元測試要獨立，不然相依會造成連續的失敗

##### Repeatable

單元測試要能重複的在不同環境下測試

##### Self-Validating

單元測試只能回傳 True/False 不要回傳過程訊息

##### Timely

單元測試最好在撰寫主程式前撰寫，並撰寫完後馬上撰寫主程式

---

### Class

<font color='red'>**保持簡短、抽象化**</font>

* [類別要夠簡短](#類別要夠簡短)
* [單一職責原則](#單一職責原則)

---

#### 類別要夠簡短

類別要有個足以描述職責的命名，如果使用了 Processor, Manager, Super 通常代表擁有太多的職責了
要能夠在不使用 if, and, or, but 等字眼下，能在 25 個字詞內簡短的秒數類別

---

#### 單一職責原則

單一職責原則 SRP(Single Responsibility Principle)

類別應該只有一個職責，唯一的一個修改的理由
<font color='red'>多使用抽象化將抽象方法分離出來</font>
系統是由許多小型類別所組成，而不是由少數幾個大型類別所組成
每個小類別"封裝單一的職責"、"只有一個修改的理由"、"與其他少數幾個類別合作來完成系統要求的行為'

---

