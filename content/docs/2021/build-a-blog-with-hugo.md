---
title: "如何以 Hugo 建立部落格？"
date: "2021-03-20T18:00:00+08:00"
draft: false
slug: "build-a-blog-with-hugo"
tags: ["Hugo"]
---

[Hugo](https://gohugo.io/) 是一個用 Go 寫成的靜態網站生成器（Static Site Generator）。號稱

> The world’s fastest framework for building websites.

不管是「網站打包」還是「從零建立網站」的速度都很快。其架構允許使用者以 Markdown 格式撰寫內容，再搭配 layout template 渲染出畫面，對於喜歡用 Markdown 簡潔撰寫文章的人來說很合適！

<!--more-->

與類似的框架 [Wordpress](https://wordpress.com/zh-tw/) 和 [Gatsby](https://www.gatsbyjs.com/) 一樣，Hugo 也有豐富多元且開源的[主題 (theme)](https://themes.gohugo.io/)，讓人方便快速地就建立起自己的網站。

此網站就是以 Hugo 建立的，並使用 [Compose Theme](https://themes.gohugo.io/compose/) 讓我們來看看是怎麼做的吧～

### Getting Started

> Reference: [Hugo - Quick Start](https://gohugo.io/getting-started/quick-start/), [Compose - Install Theme](https://themes.gohugo.io//theme/compose/docs/compose/install-theme/)

- Step 1 - Install Hugo (for MacOS)

```
brew install hugo
```

- Step 2 - New a Site

```
hugo new site my-blog
```

- Step 3 - Install a theme

```
cd my-blog
git init
git submodule add https://github.com/onweru/compose/ themes/compose
cp -a themes/compose/exampleSite/* .
```

- Step 4 - Run Hugo Server

```
hugo server
```

- Step 5 - Open `http://localhost:1313/` in browser and then you'll see a site with the Compose theme

- Step 6 - Add your content and customize the styles!

### Directory Structure

Hugo 有制式的檔案架構，若依照該架構放置內容，便能享有 Hugo 預設的功能，諸如：列表和單篇結構、多國語言、template 組合等等。以下介紹常見 Hugo 架構中各檔案夾的意義

```
my-blog
├── archetypes
├── assets         // scss and js to be processed by Hugo Pipes
├── content        // 網站主要內容都維護在此
├── layouts        // 網站頁面樣式，因為我們有使用主題，如果需要客製化覆蓋主題的樣式，可在此新增檔案
├── static         // 放置網站圖片、CSS、JS 檔案
├── theme          // 主題
└── config.toml    // 設定檔
```

### Host the Site

TBD
