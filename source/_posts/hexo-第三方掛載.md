---
title: hexo-第三方掛載
date: 2019-05-29 21:12:22
tags:
- Note
- Hexo
category:
- Tool
- Hexo
---

## 前言
本篇記錄一下常掛在 hexo 部落格的第三方平台使用方式

* [Disqus](#Disqus) (留言板)
* [whos.amung.us](#whos-amung-us) (網站在線人數統計)
* [Amazing Counters](#Amazing-Counters) (網站歷史流量統計)

<!--more-->

---

### Disqus
相信大家應該也希望有人在逛逛你的網站的同時能有可以互動的留言板吧

Disqus 是用來讓部落格的文章有留言回復的功能的常用第三方平台

#### Step0
先到 Disqus 上註冊帳號

#### Step1
點選 Get Started 後選擇 I want to install Disqus on my site

{% asset_img step1_1.png %}

#### Step2
填寫 Website Name, Category, Language 後 Create Site

{% asset_img step2_1.png %}

#### Step3
選擇 basic subscribe(或是你可以付錢訂閱) 後 選擇最下方的 I don’t see my platform listed…(hexo 不屬於上面任何一種)

{% asset_img step3_1.png %}

拉到最下方點選 Configure

{% asset_img step3_2.png %}

填選 Website Name, Website URL(你現在網站的網址) 並送出

{% asset_img step3_3.png %}

#### Step4
在 Disqus 網頁右上方點選 Admin 左上確認為正確的網站

點選 Settings 查看 Shortname

並在 local 的 hexo 部落格資料夾中找到 _config.yml，在內容中加上 disqus_shortname: “剛才看到的Shortname”

{% asset_img step4.png %}

最後在重新 generate 一次就能成功完成

---

### whos-amung-us
在網路上查詢了一下統計人數的掛載方式幾乎都是大陸做的，實在是不敢用RR
所以另外花了點時間找到了用 whos.amung.us 掛統計人數的方式，而方法也相當簡單

<font color='red'>他顯示的是目前在線人數 不是歷史統計! 不是歷史統計! 不是歷史統計!!!</font>
#### Step0
到 [whos.amung.us](https://whos.amung.us/) 的官網往下拉
選擇顏色，大小等等資訊後點下複製

{% asset_img who_step0_1.png %}

#### Step1
到 local 的 themes -> “你用的主題” -> layout -> _widget 下
新增一個 .ejs 檔後將 code 直接貼進去

#### Step2
到 local 的 themes -> “你的的主題” -> _config.yml
找到 widgit 並加上剛剛新增的 .ejs 檔名
重新再 generate 一次就完成惹

{% asset_img who_step2_1.png %}

---

### Amazing-Counters
一樣是一個簡單使用的統計平台，可以顯示歷史瀏覽人數
<font color='red'>不過網站為 HTTP 有資安疑慮的請謹慎使用</font>

{% asset_img amazing_step0.png %}

#### Step0
到 [Amazing Counters](https://amazingcounters.com/sign-up.php) 直接按照步驟做到最後一步並複製最後產生的 code

#### Step1
到 local 的 themes -> “你用的主題” -> layout -> _widget 下
新增一個 .ejs 檔後將 code 直接貼進去

#### Step2
到 local 的 themes -> “你的的主題” -> _config.yml
找到 widgit 並加上剛剛新增的 .ejs 檔名
重新再 generate 一次就完成惹

{% asset_img amazing_step2.png %}