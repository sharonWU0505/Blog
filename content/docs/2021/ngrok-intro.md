---
title: "Introduction to Ngrok"
date: "2021-03-27T15:00:00+08:00"
draft: false
slug: "ngrok-intro"
tags: ["Tools"]
post_keywords: "localhost,ngrok"
---

開發時，經常會須要從另一台機器連接在 localhost 的網站，或是希望能有個 public URL 測試某些功能，例如：Facebook Open Graph 設置、Chatbot 連接、金流串接。這時就會想有沒有什麼比先部署到伺服器上再 debug 更方便的作法呢？

有的，你可用 [Ngrok](https://ngrok.com/)！

<!--more-->

Ngrok 是一個能將你的 localhost 網址對應到公開 http/https 連結的服務。背後的原理是 Ngrok 建立了一個 TCP 層上的 tunnel（本機內網和 Ngrok 雲端伺服器之間），處理需求轉發至你指定的 port。

## 使用

> [Getting Started - Setup](https://dashboard.ngrok.com/get-started/setup)

- Step 1: 註冊 Ngrok 帳號
- Step 2: 安裝 Ngrok
- Step 3: 連結帳戶（Ngrok 會給你 access token）
- Step 4: 指定 port 開始使用，例如：

```
./ngrok http 80
```

- 啟用後便可以看到連線資料
- 須注意每次拿到的連結都會不同

## 進階方案

> [Pricing](https://ngrok.com/pricing)

Ngrok 也有個付費方案，提供客製 domain、每分鐘更多連線數、TLS tunnel 等服務。
