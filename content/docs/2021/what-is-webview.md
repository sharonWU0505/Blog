---
title: "What is WebView?"
date: "2021-02-28T15:00:00+08:00"
draft: false
slug: "what-is-webview"
tags: ["WebView"]
post_keywords: "webview"
---

本篇文章預計介紹什麼是 WebView，以及 Frontend 使用 WebView 時可能會遇到的問題。

A WebView is an __embeddable browser__ that a __native application__ can use to __display web content__.

<!--more-->

## Concepts
- `embeddable browser`: WebView is developed based on engines used by common browsers such as Safari, Chrome, Firefox, Edge. It can just be viewed as a browser.
- `native application`: apps/games in our case
- `display web content`
  - commonly load web content from a `http://` or `https://` location
  - JavaScript running inside the WebView also has the ability to call native system APIs.

## 常見的 WebView 應用
- Facebook/Twitter/Line 的內建瀏覽器
- App 內嵌入廣告（類似 Web 嵌入 iframe）

## Why use WebView?

- 有助於 app 留存和 UX（不必跳出 app）
- 更新彈性較高：修改頁面 layout 或邏輯，只需修改網頁後重新部署。不須以推新版本，並要求使用者更新 app 的方式進行

## Possible problems
- 瀏覽器支援程度
  - 無法使用 `alert()`, `confirm()` 等，要以 `WebChromeClient` override methods ([ref](https://stackoverflow.com/questions/34463498/javascript-alert-message-not-working-in-android-application))
- 開發環境
  - 比照一般網頁的開發方式
  - 如果想看實際在 app 中開啟狀況，還是得將 app 以模擬器跑起來
- UX 問題
  - WebView 的關閉按鈕可能會跟網頁設計的排版衝突
  - scroll、touch move 等事件，不一定和瀏覽器的效果相同，ex: scroll bouncing
- 能否判斷網頁是否是以 WebView 開啟？
  - 判斷 user agent ([ref](https://medium.com/@chienrongkhor/%E5%A6%82%E4%BD%95%E5%88%A9%E7%94%A8javascript%E5%88%A4%E6%96%B7%E7%B6%B2%E9%A0%81%E6%98%AF%E5%90%A6%E5%9C%A8in-app%E7%80%8F%E8%A6%BD%E5%99%A8-webview-%E9%96%8B%E8%B5%B7-ae8aeb209270))
  - 保險一點 [app 開 WebView 時設定 user agent](https://stackoverflow.com/questions/27692828/detect-whether-a-website-runs-in-android-browser-or-webview-of-an-app)
- 身份驗證流程，ex: token 存取，也涉及 UX
  - 連接和 App 相同的伺服器？
  - 視為 App 的延伸並從 App 獲取部分資料？

## References
- https://www.kirupa.com/apps/webview.htm
- https://docs.uniwebview.com/
- 查 WebView 大多都是 app 相關的開發文章，事實上使用 WebView 的話，frontend 也不太需要做什麼處理
