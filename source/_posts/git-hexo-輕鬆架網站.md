---
title: git+hexo-輕鬆架網站
date: 2019-05-29 20:48:48
tags: 
- note 
- git 
- hexo
category: 
- Tool 
- hexo
---

## 前言
原本一開始 google 用 github 架設網站清一色的是 jekyll，實際用了一下後發現感覺不太合胃口，用起來也不太順
後來稍微再找一下就找到了 hexo ，而且在摸索過程中也幾乎沒甚麼採到雷，所以就選他了XD (而且聽說是台灣人做的可以支持一下)

在這邊也順便記錄一下 hexo 的基本安裝順序，如果你是在資工系有一點經驗的人，對於 git 也有點熟悉度，基本上在一個小時內應該都可以輕鬆搞定拉

環境: Window 10

<!--more-->

## 需求工具
* [git](https://git-scm.com/)
* [nodejs](https://nodejs.org/en/)

## 操作步驟
#### Step0
先將上面連結的 git 跟 nodejs 安裝好，就一般平常的安裝，應該是不需要多做教學拉XD

#### Step1
在個人的 github 上新增 repository 並命名為 “你的github帳號”.github.io
一定要這個名字喔，<font color='red'>請不要打yourname</font>，要換成你的github帳號，不然會出事
{% asset_img step1_git.png %}

#### Step2
先移動到你預計要擺放資料的資料夾並打開 cmd

{% asset_img step2_cmd.png %}

#### Step3
輸入指令安裝 hexo

```bash
npm install -g hexo-cli
```

#### Step4
輸入指令初始化

```bash
hexo init blog
```

#### Step5
進入blog資料夾

```bash
cd blog
```

#### Step6
新增第一篇貼文

```bash
hexo new hello-blog
```

#### Step7
產生靜態網站

```bash
hexo g # g = generate
```

#### Step8
開啟 local 的 server(預設為4000)

```bash
hexo s # s = server
```

如果port被占用造成失敗就換個port

```bash
hexo server -p 5000
```

#### Step9
在瀏覽器輸入 localhost:”port的號碼” 就可以進入 local 的 blog 惹

{% asset_img step9_web.png %}

#### Step10
如果要讓網站對外，上到 github 的話

先安裝一下套件

```bash
npm install hexo-deployer-git --save
```

#### Step11
找到資料夾內的 _config.yml 並修改最下方的 deploy 部分

<font color='red'>請將 your repository 改成之前新增在 github 的 repository.git</font>


{% asset_img step11_config.png %}

#### Step12
清出一下暫存及重新生成一次

```bash
hexo clean
hexo g
```

就可以部屬上去了

```bash
hexo d # deploy
```

#### Step13
只要在網址上打 “你的github帳號”.github.io 就能成功看到惹(有時候要稍微等幾分鐘)

<br/>
<br/>

## 結語
484很簡單呢，我相信對於一般有稍微接觸這領域的應該都不太難才對
不只能用markdown寫網頁，也省去了自己寫那些複雜的框架問題(可以抓別人的框架改)
如果你自己本身是走網頁前端當然也可以自己寫囉
不過小弟對於網頁語言苦手，所以還是抓框架好了><