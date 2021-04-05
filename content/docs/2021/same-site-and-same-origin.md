---
title: "Same-Site and Same-Origin"
date: "2021-04-05T14:00:00+08:00"
draft: false
slug: "same-site-and-same-origin"
tags: ["HTTP"]
post_keywords: "http,https,same-site,same-origin,cross-site,cross-origin,cors"
---

之前總以為 same-site 的網站一定也是 same-origin，後來才發現並非如此，是兩個不同的定義。此篇將介紹 same-site 和 same-origin 的定義，讓往後與後端 RD 討論 cookie `SameSite` 和 `Access-Control-Allow-Origin` header 設定時能更順利。

<!--more-->

## Origin

Origin (網域) 是 __scheme__ (或稱 protocol)、__hostname__ (或稱 domain)，和 __port__ 的組合。

```
https://www.example.com:443
```

以上方網址為例
- __scheme__ 是 `https`
- __hostname__ 是 `www.example.com`
- __port__ 是 `443`

因此，網址 `https:www.example.com:443/foo` 的 origin 為 `https:www.example.com:443`。

#### "same-origin" and "cross-origin"

有著同樣 origin 的網站會是 "same-origin"（同源）；其餘則為 "cross-origin" (跨域/跨來源)。

origin A: `https://www.example.com:443`

| origin B | "same-origin" or "cross-origin" to origin A |
| --- | --- |
| `https://www.evil.com:443` | cross-origin: different domains |
| `https://example.com:443` | cross-origin: different subdomains |
| `https://login.example.com:443` | cross-origin: different subdomains |
| `http://www.example.com:443` | cross-origin: different schemes |
| `https://www.example.com:80` | cross-origin: different ports |
| `https://www.example.com:443` | same-origin: exact match |
| `https://www.example.com` | same-origin: implicit port number (443) matches |

#### 跨來源資源共用（[Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/CORS)）

> 當 user agent 請求一個不同源的資源時，會建立一個跨來源 HTTP 請求（cross-origin HTTP request）。基於安全性考量，跨來源 HTTP 請求會受到限制，例如 `XMLHttpRequest` 和 `Fetch API` 都遵守同源政策（same-origin policy）。這代表 API 除非使用 CORS 標頭，否則只能請求與應用程式相同網域的 HTTP 資源。

由於此篇著重比較 same-site 和 same-origin 的差別，所以若想知道更多跨來源資源共用的規範，請見 [MDN Web Docs](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/CORS)。

## Site (eTLD + 1)

"Site" 是指 __TLD (Top-level domain)__ 和它之前的 __domain__。TLDs 包含條列在 Root Zone Database 中的 domain，例如 `.com` 和 `.org`。

以 `https://www.example.com:443/foo` 為例
- __eTLD__ 是 `.com`
- __site (eTLD + 1)__ 是 `example.com`
- eTLD 和 TLD 的差別下方會說明

不過也有另一類的 domain，像是 `.co.jp` 和 `.github.io`。這類 domain 如果只用 `.jp` 和 `.io` 作為 TLD 去定義 site 是不足夠的，而且也缺乏廣泛適用的邏輯去判斷。所以出現了表列的 __eTLDs__ (effective TLDs)，被條列在 [Mozilla Public Suffix List](https://wiki.mozilla.org/Public_Suffix_List) 中。site 也被稱為 `eTLD + 1`。

以 `https://my-project.github.io` 為例
- __eTLD__ 是 `.github.io`
- __site (eTLD + 1)__ 是 `my-project.github.io`

#### "same-site" and "cross-site"

有相同 eTLD + 1 的網站為 "same-site"；其餘則為 "cross-site"。

site A: `https://www.example.com:443`

| site B | "same-site" or "cross-site" to site A |
| --- | --- |
| https://www.evil.com:443 | cross-site: different domains |
| https://login.example.com:443 | same-site: different subdomains don't matter |
| http://www.example.com:443 | same-site: different schemes don't matter |
| https://www.example.com:80 | same-site: different ports don't matter |
| https://www.example.com:443 | same-site: exact match |
| https://www.example.com | same-site: ports don't matter |

而 `cats.github.io` 和 `dogs.github.io` 為 cross-site，因為它們的 eTLD + 1 不同。

#### "schemeful same-site"

從上述的定義中可以知道，__scheme__ 是否相同不影響 "same-site" 的判斷，但為了避免 HTTP 被以 weak channel 使用，[協定](https://tools.ietf.org/html/draft-west-cookie-incrementalism-01#page-8)開始商議將 scheme 納入判斷中，稱為 "schemeful same-site"。

如此一來，`https://www.example.com:443` 和 `http://www.example.com:443` 便為 "cross-site"，因為它們的 scheme 不同。

#### Chrome 80 後針對第三方 cookie 的調整

> Chrome 80+ 後將對所有未設定 `SameSite` 屬性的 Set-Cookie 預設為 `SameSite=Lax`，意味著除了 top level navigate + GET 的請求行為外，其餘 cross-site request 送發 cookie 的行為將預設被關閉。

`SiteSite` values
- `SameSite=None` (with `Secure`)
- `SameSite=Lax`
- `SameSite=Strict`

更詳細的說明請見 [Chrome 80 後針對第三方 Cookie 的規則調整](https://ianhung0529.medium.com/chrome-80-%E5%BE%8C%E9%87%9D%E5%B0%8D%E7%AC%AC%E4%B8%89%E6%96%B9-cookie-%E7%9A%84%E8%A6%8F%E5%89%87%E8%AA%BF%E6%95%B4-default-samesite-lax-aaba0bc785a3)

## How to check if a request is "same-site", "same-origin", or "cross-site"

部分瀏覽器會在請求的 HTTP header 中附上 [`Sec-Fetch-Site`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-Fetch-Site) 值，不過目前普及率不算高。

`Sec-Fetch-Site` 會是以下幾個值：
- `cross-site`
- `same-site` (沒區分 "schemeful same-site")
- `same-origin`
- `none`

## Reference

- [Understanding "same-site" and "same-origin"](https://web.dev/same-site-same-origin/)
