---
title: "Introduction to Cookie"
date: "2020-02-17T09:00:00+08:00"
draft: false
slug: "cookie"
tags: ["HTTP"]
post_keywords: "http,https,cookie,browser"
---

認識 cookie 的筆記。

<!--more-->

## Cookie
> An HTTP cookie (web cookie, browser cookie) is a small piece of data that **a server sends to the user's web browser**. The browser may store it and send it back with the next request to the same server.
> It is one of the methods available for **remembering stateful information** for the stateless HTTP protocol.

#### Purposes
* Session management: logins, shopping carts, game scores, etc.
* Personalization: User preferences, themes, etc.
* Tracking: recording and analyzing user behavior

#### Limitations and Alternatives
* Cookies are sent with every request, so they can worsen performance (especially for mobile data connections).
* Modern APIs for client storage are the **Web storage API (localStorage and sessionStorage)** and **IndexedDB**.


---


## Creating Cookies
* Each cookie is a `key=value` pair.
* When receiving an HTTP request, a server can send a `Set-Cookie` header with the response.

```
HTTP/2.0 200 OK
Content-type: text/html
Set-Cookie: <cookie-name>=<cookie-value>;<cookie-name>=<cookie-value>
```

* The cookie is usually stored by the browser, and then the cookie is sent with requests made to the same server inside a `Cookie` HTTP header.

```
GET /sample_page.html HTTP/2.0
Host: www.example.org
Cookie: <cookie-name>=<cookie-value>;<cookie-name>=<cookie-value>
```
---

## Cookie Keys
* `Secure`
* `HttpOnly`
* `SameSite`
  - `SameSite=None`
    - The browser will send cookies with both cross-site requests and same-site requests.
  - `SameSite=Strict`
    - The browser will only send cookies for same-site requests (requests originating from the site that set the cookie).
  - `SameSite=Lax`
    - Same-site cookies are withheld on cross-site subrequests, such as calls to load images or frames, but will be sent when a user navigates to the URL from an external site; for example, by following a link.

## SameSite Cookie Changes
* **Chrome 80** released in February 2020 adopted new policy for [`SameSite`](https://ianhung0529.medium.com/chrome-80-%E5%BE%8C%E9%87%9D%E5%B0%8D%E7%AC%AC%E4%B8%89%E6%96%B9-cookie-%E7%9A%84%E8%A6%8F%E5%89%87%E8%AA%BF%E6%95%B4-default-samesite-lax-aaba0bc785a3) to prevent Cross Site Request Forgery (CSRF).
