---
title: 讓 Hexo 支援 markdown 語法 
date: 2019-01-14
categories: 
    - Hexo
author: SoJ
tags:
    - Hexo
index_img: img/markdown.jpg
---

# Hexo 設置
首先我要先謝謝 [好想工作室](http://goodideas-studio.com/) 的前輩 [Chris大大](https://dwatow.github.io/)，本文大部分都是參考他的文章，大家可以去逛逛他的筆記，很多技術相關的文章可以參考。
> [Chris 筆記 - Hexo 研究筆記 1](https://dwatow.github.io/2017/06-18-hexo/re-equip-hexo1/)

## 預先安裝
1. NodeJS => 會順便安裝 npm 套件管理系統
```shell
brew install node
```

2. Hexo 主程式 => 透過 npm 安裝
```shell
npm install hexo-cli -g
```

3. 建立一個新資料夾並安裝 hexo 基礎框架
```shell
hexo init blog
```

4. 進入資料夾 `cd blog`，安裝相關套件
```shell
npm install
```

5. 啟動本機 server(本機先查看結果)
```shell
hexo server
```

## 支援 markdown 語法
讓 Hexo 支援 markdown 語法，有一個最大好處，就是因為 [HackMD](https://hackmd.io/) 本身也是支援 markdown 語法，這樣在 HackMD 上的筆記，也可以快速轉換成我們部落格的貼文。

以下請參考 Chris 筆記
1. 刪除原始渲染器
```shell
npm uninstall hexo-renderer-marked
```
2. 安裝套件 hexo-renderer-markdown-it
```shell
npm install git+https://github.com/hexojs/hexo-renderer-markdown-it.git --save
```
3. 安裝渲染器外掛(下面共11個)
```shell
npm install markdown-it-abbr markdown-it-container markdown-it-deflist markdown-it-emoji markdown-it-footnote markdown-it-imsize markdown-it-ins markdown-it-mark markdown-it-regexp markdown-it-sub markdown-it-sup --save
```
4. HackMD 不用裝但 Hexo 要裝的套件
```shell
npm install markdown-it-checkbox --save
```
5. 配置 Hexo 設定檔
```
# Markdown-it config
## Docs: https://github.com/celsomiranda/hexo-renderer-markdown-it/wiki
markdown:
  render:
    html: true # Doesn't escape HTML content so the tags will appear as html.
    # xhtmlOut: false # Parser will not produce XHTML compliant code.
    breaks: true # Parser produces `<br>` tags every time there is a line break in the source document.
    langPrefix: ''
    linkify: true # Returns text links as text.
    typographer: true # Substitution of common typographical elements will take place.
    quotes: '“”‘’' # "double" will be turned into “single”
    # 'single' will be turned into ‘single’
  plugins:
    - markdown-it-abbr
    - name: markdown-it-container
      options: success
    - name: markdown-it-container
      options: info
    - name: markdown-it-container
      options: warning
    - name: markdown-it-container
      options: danger
    - markdown-it-deflist
    - name: markdown-it-emoji
      options:
        shortcuts: {}
    - markdown-it-footnote
    - markdown-it-imsize
    - markdown-it-ins
    - markdown-it-katex
    - markdown-it-mark
    - markdown-it-regexp
    - markdown-it-sub
    - markdown-it-sup
    - markdown-it-checkbox
  anchors:
    level: 1 # Minimum level for ID creation. (Ex. h2 to h6)
    collisionSuffix: 'v' # A suffix that is prepended to the number given if the ID is repeated.
    permalink: true # If true, creates an anchor tag with a permalink besides the heading.
    permalinkClass: header-anchor # Class used for the permalink anchor tag.
    permalinkSymbol: '' # The symbol used to make the permalink.
```
6. 安裝過濾器
   * 支援 流程圖
```shell
npm install hexo-filter-flowchart --save
```

   * UML 循序圖
     
```shell
npm install hexo-filter-sequence --save    
```

---

## 數學表達式
> [Chris 筆記 - hexo加上數學式 MathJax](https://dwatow.github.io/2018/01-28-hexo/katex/)

1. 安裝套件
```shell
npm install markdown-it-katex --save
```
2. 配置 hexo 設定檔
```
markdown:
  plugins:
    - markdown-it-katex
```
3. 加入 CSS
`<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.5.1/katex.min.css">
`

其中 CSS 要加在 `themes>>layout>>_partial>>head.ejs` 的檔案裡，只要加在 `<head>` `</head>` 之間任意位置就可以

---

## 額外安裝
> [hexo-pdf 安裝說明](https://www.npmjs.com/package/hexo-pdf)

hexo-pdf => 可支援 pdf 和 slideShare(簡報)

```shell
npm install --save hexo-pdf
```

### Useage
Normal PDF

```
{% pdf http://7xov2f.com1.z0.glb.clouddn.com/bash_freshman.pdf %}
```

or

```
{% pdf ./bash_freshman.pdf %}
```

Google drive
```
{% pdf https://drive.google.com/file/d/0B6qSwdwPxPRdTEliX0dhQ2JfUEU/preview %}
```

Slideshare
```
{% pdf http://www.slideshare.net/slideshow/embed_code/key/8Jl0hUt2OKUOOE %}
```

---

## 指定程式碼開始行號
範例如下
![](https://i.imgur.com/oOI71pQ.png)

配置 hexo 的 `_config.yml`
```
highlight:
  first_line_number: 'inline' # | 'always1'
```

---

# CSS 相關

## Hackmd 中的特殊圖示

### 範例

**電腦 & 平板**

<i class="fa fa-edit fa-fw"></i> 編輯：只看到編輯器
<i class="fa fa-eye fa-fw"></i> 檢視：只看到結果
<i class="fa fa-columns fa-fw"></i> 同時：同時看到兩邊

**手機**

<i class="fa fa-toggle-on fa-fw"></i> 檢視：只看到結果
<i class="fa fa-toggle-off fa-fw"></i> 編輯：只看到編輯器

### 做法

要顯示上面的特殊圖示要在 `themes>>layout>>_partial>>head.ejs` 的檔案裡，`<head>` `</head>` 之間任意位置加入下方 CSS 條件
`<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">`

## 警告區塊
* 範例
:::success
耶 :tada:
:::

:::info
這是訊息 :mega:
:::

:::warning
注意 :zap:
:::

:::danger
喔不 :fire:
:::

在主題的 `source>>style.css` 或者 `source>>style.less` 或者 `source>>style.styl`
(每個主題的 css 存放路徑或檔名可能不太一樣，要找一下，通常檔名都是 style)
```
.success, .info, .warning, .danger {
  padding: 15px;
  margin-bottom: 20px;
  border: 1px solid transparent;
  border-radius: 4px;
}

.success {
  color: #3c763d;
  background-color: #dff0d8;
  border-color: #d6e9c6;
}

.info {
  color: #31708f;
  background-color: #d9edf7;
  border-color: #bce8f1;
}

.warning {
  color: #8a6d3b;
  background-color: #fcf8e3;
  border-color: #faebcc;
}

.danger {
  color: #a94442;
  background-color: #f2dede;
  border-color: #ebccd1;
}
```

---

### 佈署到 github

1. 安裝佈署套件
```shell
npm install hexo-deployer-git --save
```
2. 配置 hexo 設定檔
```
deploy:
  type: git
  repo: <repository url>
  branch: [branch]
  message: [message]
```
3. 上傳 github
```shell
hexo deploy -g
```

---

### 安裝套件

```
  "dependencies": {
    "hexo": "^3.7.0",
    "hexo-autoprefixer": "^2.0.0",
    "hexo-deployer-git": "^0.3.1",
    "hexo-filter-flowchart": "^1.0.4",
    "hexo-filter-sequence": "^1.0.3",
    "hexo-generator-archive": "^0.1.5",
    "hexo-generator-category": "^0.1.3",
    "hexo-generator-feed": "^1.2.2",
    "hexo-generator-index": "^0.2.1",
    "hexo-generator-json-content": "^3.0.1",
    "hexo-generator-search": "^2.4.0",
    "hexo-generator-tag": "^0.2.0",
    "hexo-helper-qrcode": "^1.0.2",
    "hexo-pdf": "^1.1.1",
    "hexo-recommended-posts": "^1.0.3",
    "hexo-renderer-ejs": "^0.3.1",
    "hexo-renderer-less": "^1.0.0",
    "hexo-renderer-markdown-it": "git+https://github.com/hexojs/hexo-renderer-markdown-it.git",
    "hexo-renderer-stylus": "^0.3.3",
    "hexo-server": "^0.3.1",
    "markdown-it-abbr": "^1.0.4",
    "markdown-it-checkbox": "^1.1.0",
    "markdown-it-container": "^2.0.0",
    "markdown-it-deflist": "^2.0.3",
    "markdown-it-emoji": "^1.4.0",
    "markdown-it-footnote": "^3.0.1",
    "markdown-it-imsize": "^2.0.1",
    "markdown-it-ins": "^2.0.0",
    "markdown-it-katex": "^2.0.3",
    "markdown-it-mark": "^2.0.0",
    "markdown-it-regexp": "^0.4.0",
    "markdown-it-sub": "^1.0.0",
    "markdown-it-sup": "^1.0.0"
  }
```


