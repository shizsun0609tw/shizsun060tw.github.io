---
title: QT-打包-Visual-Studio-專案步驟
date: 2019-09-16 15:11:59
tags: 
- Tool
category:
- Tool
---

### Step1

先在 Visual Studio 切成 Release 模式 編譯並執行確認程式無任何問題

{% asset_img Step1.png Step1 %}

<!--more-->

### Step2

從專案的資料夾下找到產生出來的 release 執行檔並將其複製到另一個準備打包的資料夾

{% asset_img Step2.png Step2 %}

### Step3

在你的 Qt 資料夾下找到 windeployqt.exe 並打開在此目錄下打開 cmd

{% asset_img Step3.png Step3 %}

### Step4

在 cmd 下輸入指令

```bash
windeployqt 執行檔路徑
```
<br/>
{% asset_img Step4.png Step4 %}

並確認打包的訊息是否有任何錯誤

(如果有用 qml 請用下面的指令)

```bash
windeployqt 執行檔路徑 --qmldir qml路徑
```

(qml路徑在前面 bin 的上一層能找到)

{% asset_img Step4-2.png Step4 %}

### Step5

打包完成就能在執行檔的資料夾下看到 Qt 幫你打包好的東西

{% asset_img Step5.png Step5 %}

### Step6

某些 Visual Studio 的 dll 或是自己需要的 dll 就要靠自己手動丟了

{% asset_img Step6.png Step6 %}

### Step7

有時候不同顯卡用到的 OpenGL 會不一樣  所以如果發生衝突請把這個檔案拿掉 讓他自己去抓電腦系統內的

{% asset_img Step7.png Step7 %}