---
title: "Optimize Images for Web Performance"
date: "2021-01-10T09:00:00+08:00"
draft: false
slug: "optimize-images"
tags: ["Performance", "Optimize Images"]
post_keywords: "web performance,optimize images,webp,lazy-loading"
---

本篇將介紹兩種常見的圖片優化方式，分別是使用 WebP 格式和 lazy loading，適時地採用這些方法可以提升網站效能。

<!--more-->

## 方法一：使用 WebP 格式減少圖片載入時間

WebP 是 Google 在 2010 年釋出的圖片格式，和傳統的 JPEG 格式相比之下檔案更小，同時能兼顧影像品質。

> JPEG 2000, JPEG XR, and WebP are image formats that have superior compression and quality characteristics compared to their older JPEG and PNG counterparts. Encoding your images in these formats rather than JPEG or PNG means that they will load faster and consume less cellular data. (from [web.dev](https://web.dev/uses-webp-images/))

#### 瀏覽器支援度

根據 [Can I Use](https://caniuse.com/webp) 的資料，WebP 除了 IE 和 Safari，大部分瀏覽器已經支援，其支援度較 [JPEG 2000](https://caniuse.com/?search=JPEG%202000) 和 [JPEG XR](https://caniuse.com/?search=JPEG%20XR) 高上許多。

#### 轉換圖片為 WebP 格式

- Convertio：線上轉檔服務，不過要注意檔案大小限制
- File Format plugin by Telegraphics：PhotoShop 的外掛，不確定新版 PhotoShop CC 能不能使用
- [cwebp Encoder](https://developers.google.com/speed/webp/docs/cwebp)：Google 官方的 command line utility
- [imagemin-webp](https://www.npmjs.com/package/imagemin-webp)：npm 套件，要寫一小段 script

Webp 轉換套件眾多，可再依照需求挑選。

#### 在 HTML 使用 WebP 圖片

```html
<picture>
  <source srcset="image.webp" type="image/webp">
  <!-- 可多寫幾個 source，瀏覽器讀取的順序是由上而下，直到遇見支援的格式為止 -->
  <img src="fallback.jpg" alt="fallback圖片">
</picture>
```

#### 在 CSS 使用 WebP 圖片

- 要透過 JavaScript 去偵測瀏覽器能不能正常讀取 WebP 圖片，作法是實際載入一張 WebP 圖，然後根據載入成功與否，加上對應的 classes
- 可以選擇用第三方套件，例如 [Modernizr](https://modernizr.com/)，或自己手寫

```css
.webp .div-with-background {
  background-image: url("image.webp");
}

.no-webp .div-with-background {
  background-image: url("fallback.jpg");
}
```

## 方法二：透過 Lazy Loading 延遲載入圖片

Lazy loading 的概念是：網頁在瀏覽時只載入一開始所需的圖片，其餘圖片在後續滾動到畫面時才載入。

#### Native Lazy Loading

目前這項功能在瀏覽器上應沒有預設開啟，必須由使用者手動啟用。

- 啟用功能：打開 Chrome 後到網址輸入 `chrome://flags`，接著搜尋 `lazy`，再啟用這功能：`Enable lazy image loading (#enable-lazy-image-loading)`
- 使用方法：在 HTML 的 img tag 加上 `loading="lazy"`

```html
<img src="my-image.jpg" loading="lazy">
```

- 支援程度：0% (from [Can I Use](https://caniuse.com/loading-lazy-attr))

#### 現行常見作法

- 方法一：使用第三方套件（例如 [lazysizes](https://github.com/aFarkas/lazysizes)、[Lozad.js](https://apoorv.pro/lozad.js/)）
- 方法二：監聽 scroll、resize 和 orientation-change 事件 (not suggested)
  - scroll event 只要使用者滑一下就會排山倒海地觸發，很容易導致效能問題
  - 通常會搭配 debouncing 或 throttling 去抑制觸發的次數或頻率，但又導致程式碼太複雜
- 方法三：使用 Intersection Observer API（[支援度](https://caniuse.com/intersectionobserver)不錯）

#### 實作 Lazy Loading 之步驟（以使用 Intersection Observer API 為例）

- 不讓圖片正常載入：先將圖片 URL 放在 `data-src`

```html
<!-- 無法正常載入的圖片 -->
<img class="img lazy" data-src="cat.jpg">
```

- 監視圖片元素，判斷它們是否進入到畫面中（倚賴 Intersection Observer API）
  - 首先創造一個 Intersection Observer instance
  - 傳入一個 callback function 參數，等偵測到元素進入畫面時 callback function 會被呼叫
  - 使用 instance 的 observe method 開始監視元素
- 元素進入畫面後，再載入圖片
  - 判斷目標元素是否進入畫面
  - 確認目標進入畫面後，把 `data-src` 的值取出，放到 `src` 以顯示圖片
- 使用 `observer.unobserve` 取消監視元素

```javascript
// callback function parameter for observer
function onEnterView(entries, observer) {
  for (let entry of entries) {
    if (entry.isIntersecting) {
      // 監視目標進入畫面
      const img = entry.target
      img.setAttribute('src', img.dataset.src) // 把值塞回 src
      img.removeAttribute('data-src')
      observer.unobserve(img) // 取消監視
    }
  }
}

const watcher = new IntersectionObserver(onEnterView)
const lazyImages = document.querySelectorAll('img.lazy')
for (let image of lazyImages) {
  watcher.observe(image) // 開始監視
}
```
