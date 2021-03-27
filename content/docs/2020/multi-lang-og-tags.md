---
title: "Building Multi-Languages OG Tags"
date: "2020-04-08T19:00:00+08:00"
draft: false
slug: "multi-lang-og-tags"
tags: ["Open Graph", "SEO"]
post_keywords: "open graph,html,meta,seo"
---

之前遇到 __網頁各語言有各自 open graph 設定__ 的需求，所以決定寫一篇文章介紹什麼是 open graph，以及紀錄 Single Page Application (SPA) 如何實現多國語言之 og tag 設置。
## The Open Graph Protocol
> The Open Graph protocol enables any web page to become a rich object in a social graph. For instance, this is used on Facebook to allow any web page to have the same functionality as any other object on Facebook.
> source: https://ogp.me/

- OG tags are used to decide how you website looks on SNS.
- Different SNS (ex: Facebook, Twitter) has its own og rules, but most tags used are the same.
- To turn your web pages into graph objects, you need to add basic metadata to your page.

我們在 Twitter 張貼連結時會看到附圖、標題、說明等內容，即是 open graph 功能的顯現。

![](/images/docs/og.png)

## Basic Metadata

> - Facebook - [A Guide to Sharing for Webmasters](https://developers.facebook.com/docs/sharing/webmasters)
> - Twitter - [Cards](https://developer.twitter.com/en/docs/tweets/optimize-with-cards/overview/markup)
#### Required meta data
- `og:title`: 文章的標題
- `og:type`: 內容的媒體類型
- `og:image`: 用戶分享內容時顯示的圖像網址
- `og:url`: 網頁的標準網址

#### Optional meta data
- `og:description`: 內容的簡短說明
- `og:locale`: 資源的地區設定
- `og:locale:alternate`
- `og:determiner`
- `og:audio`
- `og:video`

#### Example
```
<meta property="og:type" content="article" />
<meta property="og:title" content="title to show on SNS" />
<meta property="og:description" content="describe your website" />
<meta property="og:image" content="thumbnail for website" />
<meta property="og:url" content="your website's link" />
```

---

## Debug Tools

設置 open graph tag 後，如果想測試，可使用以下 debug tools

- Enter the site URL (should be public URL) to preview share dialog on SNS
  - [Facebook share debugger](https://developers.facebook.com/tools/debug/?locale=zh_TW)
  - [Twitter card validator](https://cards-dev.twitter.com/validator)

---
## Limitations
- Open graph meta tags cannot be altered dynamically and the url of the page is constant regardless of search result.
- For SPA, all pages are developed from a single `index.html` file, so all pages share the same og tags.

#### Solution to Build Multi-languages OG Tags
__1. Build multiple static pages for SNS crawlers__

> Source: [Single Page Applications and Open Graph](https://stackoverflow.com/questions/16069501/single-page-applications-and-open-graph)

- Create pages only for og crawlers, which only contain og meta data and a script to redirect back to the SPA.
  - `{site_url}/zh.html`
  - `{site_url}/en.html`
- Send those pages based on languages when sharing for og crawlers to get og tags.

__2. Use Server-Side-Rendering (SSR) or Isomorphic__

- React.js with Next.js
- 大致概念為第一次頁面渲染使用 SSR，後續操作維持 CSR
